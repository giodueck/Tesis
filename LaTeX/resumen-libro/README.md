# Resumen libro
## Initial compilation
Bibliography compiled initially with `biber main`. As the bib file is in another directory, make sure to do it there if this fails.

## Normal compilation
Compiled with [vimtex](https://github.com/lervag/vimtex), which uses `latexmk`. Also updates when `libro.bib` changes.

`main.tex.latexmain` hints VimTex towards treating `main.tex` as the main file, even when starting
the compiler in another file.
