Dotfiles
========================
Dotfiles rule your configuration. They can dictate the appearance of your command line, let you know which branch of a git repository you're working off of, and much more.

By default, your dotfiles are invisible and live in the home user directory.To see your dotfiles, open a Terminal window and type:

	defaults write com.apple.Finder AppleShowAllFiles TRUE

Relaunch Finder, and you'll now be able to navigate to your home user directory and see files like .bashrc and .profile.

Hiding your dotfiles again is as easy as:

	defaults write com.apple.Finder AppleShowAllFiles FALSE

If you're new to dotfiles, you're in luck: developers love to share their configurations in libraries such as:

[GitHub's Dotfiles](http://dotfiles.github.io) and [Dotfiles.org](http://dotfiles.org).

Launch Academy founder, Dan Pickett, shares his dotfiles at on his [Personal GitHub](https://github.com/dpickett/dotfiles).

Before you go copying others' work and tinkering around in your dotfiles, be sure to backup your current files. Since dotfiles rule your configuration, they can easily screw up your OS. Having backups to revert to will allow you to tinker safely.

For a more detailed look at what your individual dotfiles can do, check out our list of dotfiles.
