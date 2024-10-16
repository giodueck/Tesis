# Vim/Neovim tips

For splitting lines after 120 characters with one macro (note: I map `kj` to ESC in insert mode):

```vim
0120lF ��5a�kb
```

or how it is printed in Vim:
```
0120lF <Ignore>a<BS><CR>kj
```