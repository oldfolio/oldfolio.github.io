## Alpine Email Client

Search all messages in a folder:
```
; t a [search string]
; = select
t = text
a = all fields
```
Command line options:
```
alpine -p [/path/to/alternate-pinerc] -passfile [/path/to/alternate-password-file]
```
Common server strings:
```
{smtp.gmail.com:465/ssl/user=user@gmail.com/novalidate-cert}
{imap.gmail.com:993/ssl/user=user@gmail.com/novalidate-cert}
{imap.gmail.com:993/ssl/user=user@gmail.com/novalidate-cert}[Gmail]/All Mail
```    
If you wish to start Pine with a remote pinerc, add the following argument at
the command line:
```
alpine -p {imap.gmail.com:993/ssl/user=user@gmail.com/novalidate-cert}pinerc-sdf-pine
```
Some settings you might want to tweak:
```
deadletter
enable alternate editor command [allows you to use ^_ to invoke an editor other than pine's default]

sigdashes

header in reply
text in reply

disable-sender / do not generate sender header
mark fcc seen
blank fcc
blank subject
dot-folders / hidden folders
convert dates to localtime
x-x-sender

index-format STATUS MSGNO DATE FROMORTO KSIZE SUBJECT
     [The default when this field is undefined is same as above
     except that the default uses SIZE instead of KSIZE.]
enable aggregate
enable flag
enable full header command
expose hidden config
sort-key = Date or Reverse Date

Default Composer Headers
	From:
	To:
	Cc:
	Fcc:
	Subject:
	Attchmnt:

customized-hdrs / Customized Headers = From: Full Name <user@domain.com>

editor = emacs [or vim]
  [The editor invoked by the ^_ (alternate editor command)]
```
GPG Filters: For either of the following filters to work, you need to create
links to the gpg executable in your ~/.gnupg directory. The links should have
different names since Pine identifies filters by the name of the file the
filter calls.
```
Display Filters _LEADING("-----BEGIN")_ ~/.gnupg/gdecrypt
  [When an incoming message is gnupg encrypted, this calls a link to the
  gpg executable allowing you to decrypt the message.]

Sending Filters ~/.gnupg/gencrypt -ca
  [This calls a link to the gpg executable and allows you to encrypt
  outgoing messages symmetrically (c) and ascii enarmored (a).]

~/.gnupg/gencrypt -a -e -r _RECIPIENTS_ -r myaddress@sdf.org
  [This ascii (a) encrypts (e) to message recipients and to myself.]
```    

Delete Filter: When a message is expunged, this filter moves it to a designated
folder (i.e. Trash). The message is only fully removed when it is expunged from
the designated folder. You probably do not want to use this filter with Gmail.
If you use Gmail, then moving a message to the [Gmail]/Trash folder will remove
it from all other locations, and when expunged from [Gmail]/Trash the message
is gone forever.
```
S(etup) --> R(ules) --> F(ilters)

Nickname: Move Deleted to Trash
Current Folder Type = Email
Message is Deleted? = Yes
Filter Action = Move (then select folder)

For IMAP Collections:	
    separate-folder-and-directory-entries
    quell-empty-directories
    You might also need: enable-lame-list-mode / deficient imap servers
```
Odd characters show up in the body of messages:

This can be caused by a character-encoding mismatch between your terminal
and alpine. Make sure both are set to utf-8.
