+++
title = 'Learn ~~to love~~ your Terminal'
date = 2024-10-17T07:07:07+01:00
draft = false
+++

I'm a big fan of CLI tools. There's something about a terminal (emulator) that feels so damn special. I still remember the excitement I felt when I edited `~/.bashrc` for the first time to create a command alias or modify the `PATH`. The simple commands I entered had an immediate effect and it felt like magic.

With time, I learnt how to manage my bash dotfiles, how to update system packages, where to configure newly installed open-source tools. It made me learn about my environment and where to look if I get an error. I learnt the importance of automatization in the development process (aka building your own development workflow!) and the importance of maintanance. After I nuked my root with the cursed `rm -rf /` command on WSL2 by failing to perform auto-complete and entering it way too late to remedy the situation but well in time to realize I'm in a pickle, I had to go through restoring my entire environment. And this is when I decided to take a more organized approach to managing my dotfiles. Today I use the [home-manager](https://github.com/nix-community/home-manager) to [declaratively define my environment](https://github.com/Kristina-Pianykh/home-manager), [Oh My Zsh](https://ohmyz.sh/) instead of bash, [kitty](https://github.com/kovidgoyal/kitty) as a terminal emulator and of course [neovim](https://github.com/neovim/neovim) (my love, always) as a code editor.

I was frustrated to realize how many people don't actually know how to use their terminal to talk to the system. I sat down with a dev or two quite a few times to guide them through an issue with authentication setup or cli tool configuration and usage. Even git, unfortunately, belongs to that realm, as well. I had to show how to manage remotes to be able to port code from GitHub to BitBucket. How to do interactive rebase to do commit squash in the terminal (contrary to the belief that this feature is solely provided by GitHub in the form of a big green button on merging a pull request). How to handle merge conflicts because the "clickety-click" approach in the VSCode UI doesn't really help to understand that the a merge conflict is distinguished by nothing more than a mere sequence of text markers. These are the examples that first come to mind but you get the idea. The more you resort to GUIs (IDEs, Web UIs) with git integration without first learning the tool the way it's originally intended and the more you rely on layers upon layers of abstraction created by these tools, the more frustration you'll end up with. Why? Because each abstraction layer means yet another integration surface, and with that more bugs. If you don't understand what layer bears responsibility for what piece of functionality, life ain't gonna be pretty.

Learn your tools and how to use them to interact with your system. Things like Bash, UNIX file system, git CLI, environment virtualization and package management are the foundation and you won't be able to build much outside of your beloved IDE (more often than not the issues arising in your IDE require you to understand what's going on under the hood to fix them). The more comfortable you are with these tools, the easier life would be (and that's the hill I'm ready to die on). Over the past year I made extra effort to use more of common UNIX CLI tools like `cat`, `awk`, `grep`, `xargs` in combination with pipes but also `rg` (optimized `grep`) and `jq` (for parsing JSON).

### Examples

* probably the most frequently used command to create a project `venv` using [poetry](https://python-poetry.org/docs/) and install in it the Python libs from `requirements.txt`
    ```bash
    cat requirements.txt | xargs poetry add
    ```

* starts the only cluster with the name "Main"
    ```bash
    databricks clusters list |\
        awk '{if ($2 == "Main") print $1}' |\
        xargs databricks clusters start
    ```

* send the SIGKILL signal to all Java processes other than `java-language-server` (note a cool little flag `-v` for `grep` for inverted match)
    ```bash
    ps aux |\
        grep '[j]ava' |\
        grep -v java-language-server |\
        awk '{print $2}' |\
        xargs kill -9
    ```


* In my graduation project, I had multiple simulations of a distributed streaming applicaton running across multiple nodes and generating gigabytes of logs.

    The logs look exactly as you might expect in a typical Java application with `Log4j`:

    ![logs](/images/logs.png)

    To inspect what happens in the system with non-deterministic behavior and test my assumptions about its behavior, I wrote some test using [bash bats](https://github.com/bats-core/bats-core). The example test below counts the number of total complex events sent and received to make sure that the "at-most-once" message delivery guarantee is fulfilled.

    ```bash
    @test 'assert no duplicate complex events()' {
      for ((i=0; i<N_NODES; i++)); do
        echo "Node $i"
        total=$(rg -S -e "Received event: complex.*" ${LOG_DIR}/$i.log |\
                awk -F  '|' '{print $2}'|\
                wc -l)
        echo "  Total: $total"

        distinct=$(rg -S -e "Received event: complex.*" ${LOG_DIR}/$i.log |\
                   awk -F  '|' '{print $2}' |\
                   awk '!seen[$0]++'|\
                   wc -l)
        echo "  Distinct: $distinct"

        assert_equal "$total" "$distinct"
      done;
    }
    ```

    `rg` performs a search on the file `${LOG_DIR}/$i.log` using a regular expression (`-e`) with a smart case (`-S`). `awk` is a magic util (that I still don't use enough!) that splits each row with the delimiter `|` and selects the 2nd element of the resulting split list (the element at index `0` being the original unsplit line). The expression `!seen[$0]++` accumulates all the occurrences of duplicate elements into the array `seen` and `wc -l` naturally returns the number of duplicates.
