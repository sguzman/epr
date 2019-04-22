# `$ epr`

![Screenshot](https://raw.githubusercontent.com/wustho/epr/master/screenshot.png)

Terminal/CLI Epub reader written in Python 3.7 with features:

- Remembers last read file (just run `epr` without any argument)
- Remembers last reading state for each file (per file saved state written to `$HOME/.config/epr/config` or `$HOME/.epr` respectively depending on availability)
- Adjustable text area width
- Adaptive to terminal resize
- Supports EPUB3 (no audio support)
- Secondary vim-like bindings
- Supports opening images

## Limitations

- Minimum width: 22 cols
- Supports regex search only
- Doesn't support hyperlinks
- Some known issues mentioned at the bottom

## Dependencies

- `curses` (Linux) or `windows-curses` (Windows)

Standalone Windows binary available in release page which will let you run `epr` without having python3.7 nor `windows-curses` installed on Windows.

## Quickly Read from History

Rather than invoking `epr /path/to/file` each time you are going to read, you might find it easier to do just `epr STRINGS.`

Example:

``` shell
$ epr dumas count mont
```

If `STRINGS` is not any file, `epr` will choose from reading history, best matched `path/to/file` with those `STRINGS.` So, the more `STRINGS` given the more accurate it will find.

Run `epr -r` to show list of all reading history.

## Opening an Image

Just hit `o` when `[IMG:n]` (_n_ is any number) comes up on a page. If there's only one of those, it will automatically open the image using viewer, but if there are more than one, cursor will appear to help you choose which image then press `RET` to open it and `q` to cancel.

## Vanilla or Markdown?

If you'd like to read epub in markdown format, checkout `vX.X.X-md` (which _requires_ module `html2text`) from release page.
Useful when you read more nonfiction reference epub (like manual or documentation) than fiction one.

## Usages

```
Usages:
    epr             read last epub
    epr EPUBFILE    read EPUBFILE
    epr STRINGS     read matched STRINGS from history

Options:
    -r              print reading history
    -d              dump epub
    -h, --help      print short/long help

Key binding:
    Help            : ?
    Quit            : q
    Scroll down     : DOWN      j
    Scroll up       : UP        k
    Page down       : PGDN      J   SPC
    Page up         : PGUP      K
    Next chapter    : RIGHT     l
    Prev chapter    : LEFT      h
    Beginning of ch : HOME      g
    End of ch       : END       G
    Open image      : o
    Search          : /
    Next Occurence  : n
    Prev Occurence  : N
    Shrink          : -
    Enlarge         : =
    TOC             : t
    Metadata        : m
```

## Known Issues

- Search function can't find occurences that span across multiple lines

  Only capable of finding pattern that span inside a single line, not sentence.
  So works more effectively for finding word or letter rather than long phrase or sentence.

  As workarounds, You can increase text area width to increase its reach or dump
  the content of epub using `-d` option, which will dump each paragraph into a single line separated by empty line (or lines depending on the epub), to be later piped into `grep`, `rg` etc. Pretty useful to find book quotes.

  Example:

  ```shell
  # to get 1 paragraph before and after a paragraph containing "Overdue"
  $ epr -d the_girl_next_door.epub | grep Overdue -C 2
  ```

- "-" chapters in TOC

  This happens because not every chapter file (inside some epubs) is given navigation points.
  Some epubs even won't let you navigate between chapter, thus you'll find all chapters named as
  "-" using `epr` for these kind of epubs.

- Skipped chapters in TOC

  Example:

  ```
  Table of Contents
  -----------------

	  1. Title Page
	  2. Chapter I
	  3. Chapter V
  ```

  This happens because Chapter II to Chapter IV is probably in the same file with Chapter I,
  but in different sections, e. g. `ch000.html#section1` and `ch000.html#section2.`

  But don't worry, you should not miss any part to read. This just won't let you navigate
  to some points using TOC.

## Inspirations

- https://github.com/aerkalov/ebooklib
- https://github.com/rupa/epub
