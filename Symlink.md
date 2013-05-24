Symbolic Links (symlinks)
========================

Symlinks are like nicknames for files or directories you want to reference. They're like telling your computer, "When I say 'John,' what I mean is 'John Jacob Jingleheimer Schimdt.'"

For the more technically minded, a symlink is the equivalent of an alias (Mac) or a shortcut (Windows) that you type at your command line. Instead of creating a copy of the file, the symlink, which is a shorter, easier to remember name, points to the full path of the file or directory itself.

```
  #> ln -s
```

Creates a symbolic link (ln stands for hard link; the -s flag stands for symbolic). The symbolic link then takes two arguments: the full path to reference, and the shorter name you want to use.

```
  #> ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" ~/bin/subl
```

Creates a link to /Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl via the shorter bin/subl.

Now you can type
```
  #> bin/subl filename
```

To open the corresponding file in Sublime Text 2.
