## EMACS
Your ~/.emacs.d/[init.el](dot-files/emacs.d-init.el.txt) file.

Open the console version of emacs instead of the X window version:
```
emacs -nw
```
 
```
Activate menu items: Meta-` [Meta-tickmark (below tilde)]
Close without saving: Ctrl-x Ctrl-c
Save: Ctrl-x Ctrl-s
Save as: Ctrl-x Ctrl-w
```
Open file or create new:
```
Ctrl-x Ctrl-f
```
This can be used to open a file on a remote server. Backspace over the default directory (~/) and enter:
```
/[ssh:]user@server.net#<port-number>:/path/to/file
```
Alternatively, you can use the "Host" specified in an SSH config file to identify a remote server:
```
/ssh:Host:/path/to/file
```

```
Kill buffer: Ctrl-x k
List buffers: Ctrl-x Ctrl-b
Open second window: Ctrl-x 2
Move to other window: Ctrl-x o
Close current window: Ctrl-x 0
Increase window size: Ctrl-x ^
Pull up directory listing: Ctrl-x d 
```
