# Vim

For now, I use Vim to write my code on WSL (that's because [CS1010](https://wenbo-notes.gitbook.io/cs1010-notes) uses vim :joy:). So, I decide to migrate the vim setup from CS1010 on my pc.

## Ubuntu

Since setting up vim and managing plugins on Windows is tedious and not elegant. I decide to use Vim on my WSL (Ubuntu). Below are the necessary configuration files:

{% @github-files/github-code-block url="https://github.com/mendax1234/Vim-config" %}

### Steps

Copy the `.vimrc` to your HOME directory. You can do this by using

```bash
git clone https://github.com/mendax1234/Vim-config.git
```

to clone the whole repository somewhere on your pc, `cd` to the repo's root directory and then use

```bash
cp ./.vimrc .
```

to copy `.vimrc` to your HOME directory (`~`)

Then, copy the `.vim` folder to your HOME directory also using&#x20;

```bash
cp -r ./.vim/ .
```

Now, the setup is done!

## Windows

The way Windows manages vim plugins is a bit different from how Ubuntu/macOS manages it. Basically, on windows, the equivalent of `.vim/` on Unix/macOS is called `vimfiles/`, which is located under your HOME directory.

### Steps

1. Copy the `.vimrc` to your HOME directory.
2. Copy all the files from the `.vim/colors/` to the `vimfiles/colors/` on your Windows machine.
3. Copy the `.vim/pack/` folder to the `vimfiles/`.

It's all done!

## Useful Tips

### Copy paste your Code

To copy your code written on WSL Vim to your Windows clipboard. We can do a vim visual selection then do the command:

```vim
:'<,'>w !clip.exe
```

The following command saves the whole file to the clipboard (not the requirement):

```vim
:w !clip.exe
```
