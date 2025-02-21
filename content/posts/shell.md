+++
title = 'Go Shell from Scratch'
date = 2025-02-20T07:07:07+01:00
draft = false
+++

https://github.com/Kristina-Pianykh/go-shell

## Why?

There are certain things that every programmer must build. Opinions differ with regard to what such these things are supposed to be but my "bucket list" includes a CPU emulator, a compiler/interpreter, a small operating system, and a shell. I've done an [emulator of a RISC-V processor](https://github.com/Kristina-Pianykh/RISC-V-emulator) and a very shallow and patchy version of an interpreter at the univirsity (both in C) but the shell part escaped me. Back then it was a pretty ambitious task for the time we were given to implement it and it was surely way over my head. Guess the implementation language for the true UNIX experience with all the classical concepts like process forking and file descriptor duplication for pipes (spoiler: it was also C). I was quite overwhelmed and had to eventually abandon the task. Now, a few years later, I could finally get back to it, only this time I chose to do it in Go. Although arguably a less conventional choice of a language for this purpose, I'd set the goal of extending my Go knowledge for professional purposes but I also consider it rather efficient to DO THE THING FAST while not getting entangled in language design features.

## Scope

When you think of a shell, what comes to mind? I bet bash and sh will be the first in the line. We've all touched it at some point and have a good idea about it. Or do we?

One of the fundamental things to understand about the classical shells is the distinction between a command-line interpreter and a scripting langugage (apart from understanding the difference between a shell and a terminal but this is a whole other story). The shells you know combine both of these functionalities. My goal was certainly not to design a new language and I wanted to focus instead on creating the environment to interact with the operating system the "UNIX way".

With that in mind, I limited the scope of the project to the following features:

• Shell builtins: `echo`, `type`, `pwd`, `cd`

• File system navigation

• File descriptor redirection

• SIGINT handling for cancelling a currently running process or not yet entered input on `Ctrl+C`

• Autocomplete with `Tab` for shell builtins and executables on `PATH`

• Pipes as IPC (inter-process communication)

## Interesting Observations

- Input autocompletion is trickier than you would think. The input via stdin in the terminal is not submitted to a program until the user has pressed `Enter`. The reason for it is the translation, or pre-processing, layer in the kernel of the operating system that buffers every key stroke to handle some special key presses like `backspace` or `delete` for modifying input or `Ctrl+C`/`Ctrl+D` for job control. This all happens in the terminal in the so-called **cooked mode** as opposed to the **raw mode** which disables the kernel buffering and pre-processing. By enabling the raw mode, we can then capture every key stroke from the shell stdin bypassing the kernel buffering.

    The dangerous territory since you run the risk of fucking up your cursor position and text placement. It's fun cause you learn what ANSI codes to use for controlling the cursor in the terminal and manually manage the translation but, if used in conjunction with goroutines, it can easily lead to "interesting" situations if not handled with care. And before you ask me, yes, racey Goroutines came bite me in the ass as I observed inconsistent and hard to reproduce behavior when the cursor in my shell would suddenly start jumping around.
- A pipe must never block.

    The pipe is considered closed when one of its ends is closed. The trivial case is when the writing process completes writing and closes the write end of the pipe. Since a write operation blocks, closing the write end of the pipe doesn't lead to deadlocks and allows the program to proceed naturally. What happens, however, if a write operation never completes?

    ```bash
    $ yes | head -n 5
    y
    y
    y
    y
    y
    $
    ```
    `yes` simply outputs `y` on a new line in an infinite loop. `head` reads the first 5 lines before closing its stdin. While the second command terminates successfully, the program continues to block since the write never reaches the end. Solution: send a SIGPIPE signal (boken pipe) to the writing program once its peer has closed the read end of the pipe.

    Running both processes as goroutines would allow for asynchronous communication between them, bypassing the blocking nature of a write operation. The piping is then considered complete when either the writer (trivial) or the reader (SIGPIPE) is closed, which should help us avoid deadlock situtations.
