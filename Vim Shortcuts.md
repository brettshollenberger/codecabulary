# Vim Shortcuts

Editing with Vim has been described as surgically precise by advanced Vim users, and for good reason: learning its atomic, combinable commands, slowly building up a personalized `.vimrc`, and scripting it with `VimL` gives you a tool of incredible precision, tailored specifically to your own weird nuances as a writer. But you can't stop after you learn the basics of Vim; then you may not even be better off than you were with your former editor, which may have contained some fairly powerful, time-saving commands itself.  # !> assigned but unused variable - commands

You have to invest yourself in Vim a lot more than you do with other editors to reap those divine-sounding levels of usage. I've heard Ben Orenstein's Vim talk a number of times <a href="http://www.youtube.com/watch?v=SkdrYWhh-8s">(check it out on YouTube)</a>, and one of the important things he stresses in that talk is getting to know yourself, and becoming frustrated with wasting time on typing. Only you can know what methods you use on a regular basis, but here are a few of my favorites to help you get started.

## command-t

Command-t is a plugin that allows you to perform fuzzy searches on your repositories like you can in Sublime Text 2. I used this feature so often in that program, that to live without it was simply unbearable. Do yourself a favor and check it out.

## xmpfilter for Rubyists

xmpfilter is by far the most magical thing I've seen in a text editor. If you write Ruby on a regular basis, it's a must have feature that evaluates code for you with a few keystrokes. For instance:

1 + 1 # => 

(1..3).to_a.map { |num| num + 1 } # => 

1 + 1 # => 

It's a very useful feature to keep you from running into a terminal to test out your code.

## Select a ruby text objects

var vir

## Select text object all

ae

## tabular.vim

tt, te, tb, <leader>a /{

## commentary.vim

gc, gc motion

Tim Pope's commentary.vim allows us to comment out lines of code in all languages supported by Vim. Using `gc` and a motion, such as `ap`, we can comment out areas like paragraphs.

## Insert mode deletions

When I type, I often find myself realizing I've misspelled a word early on, and later having to backspace back to my original position. Vim helps with this conundrum with the control-w command, which deletes the previous word while in insert mode. I might also like to delete everything previously on a line, which I can do using control-u. I don't often find myself using the control-h command, which deletes back a letter, since the backspace key is often easier in this scenario. 

## Insert Normal Mode

Insert Normal mode is just like normal mode, except that after performing exactly one action, we'll return to insert mode immediately. For instance, if we're typing, and we want to move our text to the middle of the screen, we could enter Insert Normal mode, hit `zz` to center our text, and then we're right back where we were.

We can enter Insert Normal mode by pressing `ctrl-o`

## Backing up over misspelled words

Control w

## Re-select previous paste

`gp`
