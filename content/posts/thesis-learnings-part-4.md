+++
title = 'Thesis Learnings, Part 4'
date = 2024-08-10T07:07:07+01:00
draft = false
+++

## On Java LSP support and editor[^1]

A note on IDE. [I use neovim btw](https://github.com/letieu/btw.nvim?tab=readme-ov-file) but that was until God blessed me with Java. Is what you probably expect me to say. And indeed setting up a Java LSP for neovim turned out as difficult as they say. I abondoned my beloved code editor and asked daddy aka my employer for a premium license for IntelliJ. I spent a bit of time rewriting some of the most basic functionality from neovim config in Lua to Vim script to still benefit from Vim motions in the IntelliJ IDE. I have to admit the IDE made many things easier but after a while I still turned back to neovim. Buckled up and divided into the LSP setup once again. I ended up with [`java-language-server`](https://github.com/georgewfraser/java-language-server) (as opposed to the more popular [`jdtls`](https://github.com/mfussenegger/nvim-jdtls) but it gave me the most basic things I needed: code diagnostics and quick remedy strategies (hashtag `lsp_code_actions`), signature help, autocomplete suggestions, etc. Although granted, there’re still many things I’m unhappy about (e.g. symbol signature and third-party code, more stable cross-project referencing) but this is most likely due to a skill issue.
The end line to this is that despite some LSP issues, I still found myself more productive writing Java in neovim as opposed to the fully featured IntelliJ IDE. Yet another tribute to the power of well ingrained development workflows and habits that neovim inspires.

[^1]: This post is part of the series on my 6-month ride through a project for my CS graduation thesis.
