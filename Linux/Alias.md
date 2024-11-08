If you find yourself typing out long binary name often such as command or in my case mousepad which is text editor.
You can make an alias for that command to enable quick and easy access.

In your home directory ~/ , or $HOME
There is a file .zshrc depending on your environment it could be .bash or something similar.
That file is a like config script that runs when your terminal emulator starts.
And sets some stuff for you to have a good time.
You can see it by running ls -al because it's hidden.

I suggest making a copy of it before you start fiddling with it.
```shell
cp .zshrc .zshrc_backup
```
Use either nano or mousepad to open the file and scroll all the way down.
```shell
nano .zshrc
mousepad .zshrc
```
Here is my example alias for opening mousepad by just writing mp.
```shell
alias mp='mousepad'
```

If the change does not apply run this.
```shell
source .zshrc
```
