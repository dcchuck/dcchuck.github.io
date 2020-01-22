---
layout: post
title:  "Conquering Completion in Vim"
date:   2020-01-20 12:00:00 -0400
categories: vim, vim-plugin, plugin
---

# TLDR

`Conqueror of Completion` brings the [Intellisense&trade;<sup>1</sup>](https://docs.microsoft.com/en-us/visualstudio/ide/using-intellisense?view=vs-2019) features you've come to love or envy in tools like VS Code to vim.

If you've come this far on your VIM journey, I assume you have a means of installing plugins near and dear to your own heart so I'll just point you to the Github Repo.

**[Conqueror of Completion Github](https://github.com/neoclide/coc.nvim)**

"Hey! This thing is called `coc.nvim`! I'm not using NeoVim!" If you're not using nvim, you'll need to add a few lines to your `.vimrc`.
```
" Needed to show certain messages in vanilla vim
set hidden
set updatetime=300
set shortmess=aFc
```
For the curious, I'll point you to `:help` for each of these options. Just know that all of the wonderful quickfix windows will not display without these settings.

### I am using Vanilla Vim and Still See (Nothing | Not Much)

Check the version of Vim you are using - some of the floating popups are not available on early Vim 8 versions. I had only partial functionality until updating. The easiest way to see your vim version is right on the home screen. As I type this, I am on `8.1.2250`.

## Why I Love Tab Completion

### Spell it wrong everywhere!

`%s/userNmae/userName` (with the `gc` flag if you're not certain) is something I end up reaching for after writing `userNmae` everywhere instead of `userName`. A more common source of frustration for me is spelling something correct in 9 spaces and incorrectly in 1, something I don't do often given I can rely on the `<TAB>` press to select from the buffer & packages. Typically this will get caught by the language server before I waste a compilation but I'm telling you why I love something, not why I can live without it! It shines in other moments as well, like when you need to use two examples of a misspelling and dont' want to get it wrong (like in this blog post).

### API Discovery

Ever made a super sweet module you knew others would love? Well, the entire API is just a `<TAB>` press away! Method signatures, return types, and everything your code has to offer is right at the user's fingertips. It's not about the 1st time they use it (we should all read the docs), it's about the 50th time.

## How It Works

I first brought in this package to give me the immediate feedback [VS Code](https://code.visualstudio.com/) provided as I was first diving into Typescript. Can VIM do the same? Why, yes! Underneath, `Coc.nvim` talks asynchronously to any language server you install. Your code is piped over to `tsserver`(Typescript) or `rls`(Rust) and the feedback you desire/fear is piped back to the buffer.

In the early days of working with a new language, I find this quick and inexpensive feedback incredibly valuable.

#### Footnotes

`1.` My original draft had a joke about the TradeMark, but Intellisense `is` actually a trademark of the Microsoft Corporation. I find this humurous, as I have seen the word used to describe features not built by Microsoft even though it is 'their' word.
