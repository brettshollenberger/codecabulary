#bash_profile & .bashrc

The .bash_profile and .bashrc are the place to put information that only applies to the bash (e.g. the program to start other programs) itself. This information could include alias and function definitions, shell options, and prompt settings.

By default, these files do not exist on Mac OSX. If you want to run functions from your command line, you should create a .bash_profile and .bashrc file by starting a Terminal window and typing:

```
  #> cd ~/
  #> touch .bash_profile
  #> touch .bashrc
```

These files will then appear in your home directory. For information on how to view invisible files (all dotfiles are invisible by default) view [Codecabulary: Dotfiles](https://github.com/brettshollenberger/codecabulary/blob/master/dotfiles/dotfiles.md).

You can now edit these files (they're blank by default) using your favorite text editor. Sublime Text, the Launch Academy editor of choice, can be run from the command line by typing:

```
  #> subl filename
```

For information on setting up a [symlink](https://github.com/brettshollenberger/codecabulary/blob/master/generalterms/symlink.md) to Sublime Text, view [Codecabulary: Setting Up Sublime](https://github.com/brettshollenberger/codecabulary/blob/master/sublime/sublime.md).

What the bash?
========================
The .bash_profile is executed for login shells, meaning any Mac OSX Terminal Window by default. Most other graphic user interfaces (GUIs) that emulate terminals tend to use .bashrc instead.

Since it can be a hassle to maintain two separate configuration files for login and non-login shells, you can source .bashrc from your .bash_profile by adding the following lines to your .bash_profile:

```
if [ -f ~/.bashrc ]; then
   source ~/.bashrc
fi
```

And then storing common settings in .bashrc. This change will automatically call .bashrc when you open a console instead of .bash_profile.



