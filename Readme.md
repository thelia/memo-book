# Memo book

[![Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](CC-BY-NC-SA.png)](http://creativecommons.org/licenses/by-nc-sa/4.0/)

This is the source code for the memo book content. It is licenced under the Creative Commons Attribution-Non Commercial-
Share Alike 4.0 license.

All the sources are written in markdown, feel free to export in the format you want.

You can use [Pandoc](http://johnmacfarlane.net/pandoc/index.html) for
converting the markdown if many formats.

For example in HTML :

```bash
pandoc -f markdown -t html src/* -o memo-book.html --highlight-style=pygments -s
```

