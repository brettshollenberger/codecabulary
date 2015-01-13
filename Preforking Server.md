# Preforking Server

@(UNIX)[UNIX|Forking|System Calls]

In order to save on the overhead required to fork a process, a pre-forking server forks off a pool of child processes, each of which sits waiting for work to come in.