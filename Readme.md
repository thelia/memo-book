# Memo book

This is a draft for a memo book (a kind of cheat sheet).

All the sources are written in markdown, feel free to export in the format
you want.

You can use [Pandoc](http://johnmacfarlane.net/pandoc/index.html) for
converting the markdown if many formats.

For example in HTML :

```bash
pandoc -f markdown -t html src/* -o memo-book.html --highlight-style=pygments -s
```