---
layout: post
title: "iTerm 2 Shortcuts"
date: 2014-01-27 10:39:45 +0100
comments: true
categories: terminal iterm shortcuts
---

<a href="http://www.iterm2.com/">iTerm 2</a> is a pretty awesome OSX terminal. It is highly customizable, supports themes (including <a href="https://github.com/altercation/solarized/tree/master/iterm2-colors-solarized">solarized</a>) and has powerful features.

But by default there are a few things that it doesn't do for you espcially in terms of per word browsing/deletion.

Everyione who used a text editor on OS X now how to navigate easly through words. Roughly:

 + <kbd>⌥</kbd><kbd>⌫</kbd> Deletes the previous word
 + <kbd>⌥</kbd><kbd>⌦</kbd> Deletes the next word
 + <kbd>⌥</kbd><kbd>→</kbd> Moves to next word
 + <kbd>⌥</kbd><kbd>←</kbd> Moves to previous word

Now there are similar shortcuts in bash but some of them use the <kbd>⌃</kbd> aka: <kbd>ctrl</kbd>

 + <kbd>⌃</kbd><kbd>w</kbd> Deletes the previous word
 + <kbd>⌥</kbd><kbd>d</kbd> Deletes the next word
 + <kbd>⌥</kbd><kbd>f</kbd> Moves to next (forward) word
 + <kbd>⌥</kbd><kbd>b</kbd> Moves to previous (backward) word

 Wouldn't be neat to have the exact same shortuts as in OSX ? It's very easy with iTerm2.


Open up the Preferences -> Keys Tab and map the following keys as follow:

+ <kbd>⌥←Delete</kbd>: Send Hex Code: `0x1B 0x08`
+ <kbd>⌥→Del</kbd>: Send escape code `d`
+ <kbd>⌥→</kbd>: Send escape sequence `f`
+ <kbd>⌥←</kbd>: Send escape sequence `b`

This should look similar to this:

<img src="/images/posts/iterm/shortcuts.png" />