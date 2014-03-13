# Vim Shortcuts

## commentary.vim

Tim Pope's commentary.vim allows us to comment out lines of code in all languages supported by Vim. Using `gc` and a motion, such as `ap`, we can comment out areas like paragraphs.

## Insert mode deletions

When I type, I often find myself realizing I've misspelled a word early on, and later having to backspace back to my original position. Vim helps with this conundrum with the control-w command, which deletes the previous word while in insert mode. I might also like to delete everything previously on a line, which I can do using control-u. I don't often find myself using the control-h command, which deletes back a letter, since the backspace key is often easier in this scenario. 

## Insert Normal Mode

Insert Normal mode is just like normal mode, except that after performing exactly one action, we'll return to insert mode immediately. For instance, if we're typing, and we want to move our text to the middle of the screen, we could enter Insert Normal mode, hit `zz` to center our text, and then we're right back where we were.

We can enter Insert Normal mode by pressing `ctrl-o`