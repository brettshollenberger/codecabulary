Setting Up Sublime Text 2
========================

Sublime Text 2 is the text editor of choice at Launch Academy. It can be souped up with packages to color-code and autocomplete your Ruby, snippets to make typing common code faster, and much more.


Start With Your Symlink
========================
To get Sublime setup nicely, we recommend first creating a Sublime [symlink](https://github.com/brettshollenberger/codecabulary/blob/master/generalterms/symlink.md) by typing

```
  #> ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" ~/bin/subl
```

into a command line terminal, and then running:

```
  #> export EDITOR='subl -w'
```

This will have you set up so that you can type:

```
  #> subl filename
```

to open files directly in Sublime Text 2.

Use A Package Manager
========================
ST2 allows devs to install packages that extend the functionality of the editor. In order to install these packages, you'll need a package manager like [Sublime Package Control](http://wbond.net/sublime_packages/package_control/installation).

To install Sublime Package Control, open Sublime and press ctrl+`. This shortcut opens the console. Then paste in:

```
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print('Please restart Sublime Text to finish installation')
```

Sublime is written in Python, and uses this bit of Python code to install Package Control.




