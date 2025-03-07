+++
title = 'Hard (drive) adventures'
date = 2025-03-06T07:07:07+01:00
draft = false
+++

## Paranoya or common sense?

It's only recently that I started paying close attention to digital hygiene. In fact, I clearly remember the feeling of lingering sticky anxiety learning about IT security at the uni. We were given multiple assignments to 'hack' things. Reverge-engineer a binary to crack the password by utilizing a side-channel attack or decipher a message encrypted with [AES-256](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (widely used today) in the [CBC mode](https://de.wikipedia.org/wiki/Cipher_Block_Chaining_Mode). The hacking part was extremely fun. The realization how easy it can be given enough time and resources was not. Before the uni course was over, I had changed all my passwords to randomly generated and set up 2FA wherever possible. The paranoya was real.

Now a couple of years later, I've finally found the time (and patience!) to deal with my personal data. My main goals were:

1. migrate from Yandex Disc to another cloud provider (cause I don't trust Yandex with my privacy)
2. backup my data to a physical storage (cause I don't trust cloud providers in general to store my data reliably)

In my defense, Yandex had been in my life long before I used anything other than Microsoft Word for text editing (thanks dad). Needless to say, nuking my sensitive data stored there was long overdue. I picked Google Drive as a cloud storage replacement. You might accuse me of not choosing to host my own cloud instance instead and I hear you. For better or worse, I tend to settle for convience and ease of use instead of going "all the way". Google Drive has the advantage of seemless integration in the Google ecosystem; it serves its purpose and wouldn't have the overhead of software maintance and managing my own server. And to be frank, that's not necessarily something I want to do at the moment.

Backing up to a physical storage was more interesting, though.

## Partitioning

The 2TB external hard drive I got was pre-formatted with `NTFS` for Windows. Interestingly, the partition metadata was stored in an MBR table. Creating a GPT partition did overwrite the previous one but an attempt to create a file system gave a warning about the conflicting GPT table stored at the end of the storage space of the device with the remnants of the MBR record in the "boot sector": the offset in the storage before the beginning of the partition. So I wiped the drive clean by redirecting the 0-byte stream from `/dev/zero`:

```
dd seek=0 of=/dev/sda bs=10M if=/dev/zero oflag="direct" status=progress
```

Creating a fresh GPT partition with `fdisk` was a piece of cake.

```
❯ sudo fdisk -l /dev/sda
Disk /dev/sda: 1.82 TiB, 2000365289472 bytes, 3906963456 sectors
Disk model: Elements 2621
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 0xe4909079

Device     Start        End    Sectors  Size Type
/dev/sda1   2048 3906961407 3906959360  1.8T Linux filesystem
```

You might be wondering why partitioning an external hard drive for the purpose of storing your data offline makes sense at all and trust me, I asked myself the same question. Why not just **rawdog** the file system right into the raw bytes? While it's possible, it's generally not recommended because certain software expects some partition metadata to be able to recognize and mount the device at all.

What I found interesing is that `fdisk` defaults to the 2048-sector offset in the beginning of the storage space and some in the end. The first offset is there for [partition alighment to maximize performance of the disk operations](https://www.thomas-krenn.com/en/wiki/Partition_Alignment_detailed_explanation) and the buffer in the end - for storing the GPT metadata about the partition.

## Encryption

It might have been an overkill, since I'm not gonna carry this hard drive around. But because I already had my experience with it when installing Arch and it's natively supported by Linux, I went for the `LUKS` partition encryption with [`dm_encrypt`](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system). Works like a charm.

```
cryptsetup -v luksFormat /dev/sda1
cryptsetup open /dev/sda1 drive     # decrypt
```

Once the decryption password is set and the block device is decrypted, it's mountable under `/dev/mapper/drive`.


## File System

I was choosing between [`exFAT`](https://en.wikipedia.org/wiki/ExFAT) and [`ext4`](https://wiki.archlinux.org/title/Ext4) The most appealing thing about `exFAT` is its cross-platform compatibility but I decided against it in favor of `ext4` (Linux compatible only) because it lacked journaling for preventing data loss and corruption on crash.

Now let's format and mount this bad boy:

```
mkfs.ext4 /dev/mapper/drive
mount /dev/mapper/drive <mount_dir>
```

Mounting will require elavated privileges so any read/write operations will similarly ask for `sudo`. That's rather awkward. You can change it though:

```
sudo chmod 777 <mount_dir>
```

The same happens automatically when you open the drive in some DE GUI for file management (Gnome in my case).

If we run `lsblk` for listing block devices, we can see the device has been successfully mounted. I think it's also pretty neat to check out the layering with the block device at the top level (`sda`), followed by the encrypted partition (`sda1`) and the file system (`drive`) at the core.

```
❯ lsblk -f -o NAME,FSTYPE,FSAVAIL,FSUSE%,MOUNTPOINTS
NAME        FSTYPE      FSAVAIL FSUSE% MOUNTPOINTS
sda
└─sda1      crypto_LUKS
  └─drive   ext4           1.8T     1% /mnt
```

Phew, quite like a long journey, wasn't it? And we haven't written any files yet!
