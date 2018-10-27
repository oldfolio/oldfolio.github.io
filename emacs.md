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

Top of buffer: Meta-< 
Bottom of buffer: Meta-> 
Page down: Ctrl-v 
Page up: Meta-v 
Beginning of line: Ctrl-a 
End of line: Ctrl-e 
Next line: Ctrl-n 
Previous line: Ctrl-p 
Forward on character: Ctrl-f 
Back one character: Ctrl-b 
Forward one word: Meta-f 
Back one word: Meta-b 

Set bookmark: Ctrl-x r m 
List bookmarks: Ctrl-x r l 
. With list open use "r" to rename a bookmark. 
. With list open use "d" to mark bookmark for deletion. 
. With list open use "x" to delete marked bookmark. 

Cut/Copy/Paste 
Select text by setting a beginning mark with Ctrl-space, and then 
moving cursor to end of desired selection. 
Cut selection: Ctrl-w 
Copy selection: Meta-w 
Paste selection: Ctrl-y 
Merge previous line with current: Meta-^ 

Set text width (i.e. fill column): Ctrl-x f [default is 70] 
Display column ruler: Meta-x ruler-mode 
Justify paragraph at 78 characters: Meta-78 Meta-q 

Undo: Ctrl-x u

Search: Ctrl-s 
. A subsequent Ctrl-s takes you to the next occurrence 
. Ctrl-r takes you to the previous occurrence 

Center line: Meta-o Meta-s 
Meta-l Make word lower case 
Meta-u Make word upper case 
Meta-c Capitalize word
```
Enriched Mode

To enter enriched mode, open (*i.e*. 'visit') a new file and type:
```
Meta-x enriched-mode
```
If you open a file that was saved in enriched mode, it will automatically open in enriched mode. One problem I have run into is that it does not seem possible to edit a compressed file in enriched mode.

Some enriched mode formatting reminders:
```
Meta-o-i   (Italic)  
Meta-o-b   (Bold)  
Meta-o-u   (Underline)  
Meta-o-l   (Bold-Italic)  
Meta-o-d   (Return to default text format)  
```
