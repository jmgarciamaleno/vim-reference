# Vim-reference

My humble compilation of knowledge about vim. This is just the stuff I use, vim has way more.

## Index

* [Opening](#opening)
* [Saving](#saving)
* [Closing](#closing)
* [Modes](#modes)
* [Buffers (files opened)](#buffers-files-opened)
* [Tabs](#tabs)
* [Windows](#windows)
* [Registers](#registers)
* [Folding](#folding)
* [Vimgrep](#vimgrep)
* [Vimdiff](#vimdiff)
* [Macro recording](#macro-recording)
* [Objects](#objects)
* [Amount and range](#amount-and-range)
* [Moving in buffer](#moving-in-buffer)
* [Moving in line](#moving-in-line)
* [Editing](#editing)
* [Powerful Ex commands](#powerful-ex-commands)
* [Shell commands](#shell-commands)
* [Regular expressions](#regular-expressions)
  * [Identifiers](#identifiers)
  * [Sets](#sets)
  * [Examples](#examples)
* [Tips](#tips)

## Opening

Vim can be launched from the usual app icon or from the console.

| Console command | Action |
|-----------------|--------|
| vim | Launch vim |
| vim \<file1> \<file2> ... | Launch vim with a file or files opened |
| vim * | Launch vim with all the files in the current folder opened |

## Saving

| Command | Action |
|---------|--------|
| :w | Write the current buffer |
| :wa | Write all the opened buffers |
| :wa! | Force to write all the opened buffers |

## Closing

| Command | Action |
|---------|--------|
| :q | Close buffer without saving changes |
| :qa | Close all buffers without saving changes (exit vim) |
| :qa! | Force to close all buffers without saving changes (exit vim) |
| :x | Exit vim saving all changes |

## Modes

`:help vim-modes`

Vim has 6 basic modes and 6 additional modes. These are the ones I use:

| Mode | Definition |
|------|------------|
| Normal mode | Default mode. Move around and execute commands. Pressing `ESC` almost always returns to it |
| Visual mode | Text selection mode. Activated with `v`, `shift-v` or `ctrl-v` |
| Insert mode | Text insert mode. Activated by any insert, append or open command |
| Command-line mode | Command line mode. Activated pressing `:` |
| Replace mode | Text replace mode. Activated pressing `R` |

## Buffers (files opened)

`:help buffers`

ball = All buffers.

| Command | Action |
|---------|--------|
| :e \<file> | Edit/reload file |
| :w | Write buffer to disk |
| :wa | Write all buffers to disk |
| :args ~/\* | Opens all files under ~/ |
| :ls | List buffers |
| :b# | Go to buffer # (# can be the number or part of the buffer name) |
| :bn | Go to next buffer |
| :bp | Go to previous buffer |
| :bw# | Wipe out buffer # |
| :bw * + ctrl-a | Expands * into all open buffers and wipe out all |
| ctrl-6 | Jump between last and current buffer |
| #ctrl-6 | Jump between buffer number # and current buffer |
| :bufdo \<cmd> | Executes \<cmd> in all open buffers |

## Tabs

`:help tabpage`

| Command | Action |
|---------|--------|
| :tabnew | Create tab |
| :tabclose | Delete tab |
| :tabn | Next tab |
| gt | Next tab |
| :tabp | Previous tab |
| gT | Previous tab |
| :tabm # | Move tab to position # |
| :tabm +# | Move tab # positions right |
| :tabm -# | Move tab # positions left |
| :tabdo \<cmd> | Executes \<cmd> in all open tabs |

## Windows

`:help windows`

| Command | Action |
|---------|--------|
| ctrl-w + h | Jump to the window to the left |
| ctrl-w + j | Jump to the window below |
| ctrl-w + k | Jump to the window above |
| ctrl-w + l | Jump to the window to the right |
| ctrl-w + H | Move current window left |
| ctrl-w + J | Move current window down |
| ctrl-w + K | Move current window up |
| ctrl-w + L | Move current window right |
| :resize # | Resize current window to height = # |
| :vertical resize # | Resize current window to width = # |
| ctrl-w + = | Resize all windows to the same size |
| :sb# | Splits horizontally buffer # |
| :vertical sb# | Splits vertically buffer # |
| :sf \<file> | Splits horizontally \<file> |
| :vertical sf \<file> | Splits vertically \<file> |
| :windo \<cmd> | Executes \<cmd> in all open windows |

## Registers

`:help registers`

Vim has many *clipboards* called *registers*. Registers can be accessed with `"<regId>`. Numbered registers (0 to 9) are automatically filled with text from the yank, cut an delete commands. Named registers (a to z or A to Z) are the ones you can use as you please. You can see all the registers set typing `:reg`. All the registers and their contents are stored in the *.viminfo* file so you retain them between sessions.

- The default register `"` is the *unnamed register*. When you yank (copy), cut or delete something you are using it.
- The `+` register (`*` in some OSs) is the system clipboard (*xterm-clipboard* feature required, check vim --version).
- The `0` register always stores the content of the last yank comand.
- The `.` register always stores your last typed text.

E.g:
- `"aY` Copy the current line to register a.  
- `"+p` Paste content from the register + (system clipboard).

## Folding

`:help folding`

To activate the folding capabilities in vim, the foldenable and foldmethod properties must be set.

`set foldenable` Activate folding.  
`set nofoldenable` Deactivate folding.  
`set foldmethod=syntax` How to indent. Common values: indent, syntax.  
`set foldcolumn=4` Number of columns that will be used to show the indentation levels on the left.

| Command | Action |
|---------|--------|
| zo | Open fold |
| zO | Open fold recursively |
| zc | Close fold |
| zC | Close fold recursively |
| zr | Open everything one level (reduce folding) |
| zR | Open all folds |
| zm | Close everything one level (more folding) |
| zM | Close all folds |

## Vimgrep

`:help vimgrep`

`:vimgrep` is the vim tool to search text in files or folders using its own powerful patterns. Matches are written to the vim error list, accesible with `:copen`.

| Command | Action |
|---------|--------|
| :vimgrep /\<pattern>[\c]/[g][j] \<file> ... | Search for \<pattern> in the given file/s or folder/s. Optional flags: \c = case insensitive, g = all occurrences in line, j = don't open the first match file |
| :copen | Open the error list (matches found) |
| :cn | Next match |
| :cp | Previous match |

To search files under subfolders, the `**` wildcard may be used (:help starstar-wildcard).

E.g:

- `:vimgrep /cat\c/g cats/**/*.txt | copen` Search for 'cat' case insensitive, matching all occurrences of each line in .txt files under the 'cats/' folder recursively and open the list of matches.

## Vimdiff

`:help diff`

Vim can open up to four files to work with their differences. You can open them from the console using the `vimdiff` command:

`vimdiff <file1> <file2> [<file3> [<file4>]]`

Or you can open the buffers that you want to compare in vim windows and set the diff mode in all of them:

`:windo diffthis`

| Command | Action |
|---------|--------|
| ]c | Jump to the next difference |
| [c | Jump to the previous difference |
| do | Obtain the difference from the other buffer |
| :diffget | Obtain the difference from the other buffer |
| dp | Put the difference in the other buffer |
| :diffput | Put the difference in the other buffer |
| :diffupdate | Refresh the buffers |

To leave the diff mode and return to normal mode:

`:windo diffoff!`

## Macro recording

`:help recording`

1. Press `q<key>` to start recording, do things.
2. Hit `q` to stop recording.
3. Use `@<key>` to execute the macro.

## Objects

`:help objects`

| Object | Real object |
|--------|-------------|
| w | Word |
| W | Blank-separated word |
| s | Sentence |
| p | Paragraph |
| t | Tag (markup languages, e.g: html div) |
| () | Parenthesis |
| [] | Brackets |
| {} | Braces |
| '' | Quotes |
| "" | Double quotes |
| <> | Angle brackets |

You can operate over any object:

- `<cmd>a<object>` Operate over an object.
- `<cmd>i<object>` Operate inner/inside an object.

Some handy examples:

- `dw` (Delete to word) Delete till the start of the next word.
- `caw` (Change a word) Remove the word under the cursor and enters insert mode.
- `gUaw` (Uppercase a word) Uppercase all the characters of the word under cursor.
- `ci'` (Change inside quotes) Remove the text between the next quotes and enters insert mode.
- `dit` (Delete inside tag) Remove the text between the opening and closing marks of the tag.
- `vas` (Select a sentence) Select the sentence under cursor.
- `>ap` (Indent a paragraph) Indent the paragraph under cursor. 

## Amount and range

Most commands can be preceded by a number that specifies how many times it is to be performed. E.g:

`2l` Move the cursor 2 steps right.  
`3dd` Delete 3 lines.  
`4fa` Go forward to the fourth occurrence of 'a'.

Most commands can be preceded by a range, the command will work only in the lines within the range.

| Range | Reach |
|-------|-------|
| n,m | Lines n to m |
| . | Current line |
| $ | Last line |
| % | All lines in the buffer |

E.g:
- `:1.5d` Delete lines 1 to 5  
- `:.,$y` Yank (copy) from current line to the last line  
- `:%>` Indent all lines in the buffer  

You can also select characters/lines/blocks and then press `:` to enter command mode, the selection range is automatically put before your command.

## Moving in buffer

| Command | Action |
|---------|--------|
| h | Left |
| j | Down |
| k | Up |
| l | Right |
| - | Up, to first non-blank character |
| + | Down, to first non-blank character |
| gg | Go to the first line of the file |
| G | Go to the last line of the file |
| :# | Go to line number # |
| % | Go to the matching ( ), [ ] or { } |
| #% | Go to the line on the #% of the file (50% is the middle of the file) |
| } | Go to the next paragraph |
| { | Go to the previous paragraph |
| /\<text>[\c] | Search forward for \<text>, optional \c flag = case insensitive |
| ?\<text>[\c] | Search backward for \<text>, optional \c flag = case insensitive |
| n | (After search) Jump to the next found occurrence |
| N | (After search) Jump to the previous found occurrence |
| :noh | No highlight. Hide the highlighted search finds |
| * | Search for the word under the cursor forward |
| # | Search for the word under the cursor backward |
| ma | Set mark a (a-z or A-Z) |
| 'a | Start of the line with mark a |
| `a | Go to mark a |
| :marks | View marks set |
| H | First line of the current screen |
| M | Middle line of the current screen |
| L | Last line of the current screen |
| ctrl-f | Go forward one full screen |
| ctrl-b | Go backward one full screen |
| ctrl-d | Go forward (down) half a screen |
| ctrl-u | Go backward (up) half a screen |
| ctrl-y | Move screen down without moving cursor |
| ctrl-e | Move screen up without moving cursor |
| z\<enter> | Move screen so the current line is first |
| zz | Move screen so the current line is middle |
| '. | Go back to last edited line |
| g; | Go back to last edited position |

## Moving in line

| Command | Action |
|---------|--------|
| 0 | Start of the current line |
| ^ | First non blank character of the current line |
| g\_ | Last non blank character of the current line |
| $ | End of the current line |
| gm | Go to the middle of the screen in the current line |
| #&#124; | Go to the column number # in the current line |
| f\<char> | Next occurrence of character \<char> in line (inclusive) |
| F\<char> | Previous occurrence of character \<char> in line (inclusive) |
| t\<char> | Next occurrence of character \<char> in line (exclusive) |
| T\<char> | Previous occurrence of character \<char> in line (exclusive) |
| ; | Repeat last f, F, t or T |
| , | Repeat last f, F, t or T in the opposite direction |
| w | Next start of word |
| W | Next start of blank-separated word |
| b | Previous start of word (before) |
| B | Previous start of blank-separated word (before) |
| e | Next end of word |
| E | Next end of blank-separated word |
| ge | Previous end of word |
| gE | Previous end of blank-separated word |

## Editing

| Command | Action |
|---------|--------|
| i | Insert at cursor |
| I | Insert at the start of the line |
| a | Append to cursor position |
| A | Append to the end of the line |
| x | Cut character at the cursor |
| X | Cut character before the cursor |
| y | Yank (copy) |
| yy or Y | Yank line |
| d | Delete (cut) |
| dd | Delete line |
| S | Substitute line |
| D | Delete to the end of the line |
| C | Change to the end of the line |
| p | Paste after the cursor/below the current line |
| P | Paste before the cursor/above the current line |
| o | Open a new line below the current one |
| O | Open a new line above the current one |
| v | Visual mode, characters |
| V | Visual mode, lines |
| ctrl-v | Visual mode, blocks |
| gv | Reselect previous selection |
| o | (Visual mode) Change side of selection |
| r\<char> | Replace the character under cursor with \<char> |
| R | Replace mode |
| u | Undo one change |
| U | Undo all changes in line |
| ctrl-r | Redo one change |
| ~ | Change case |
| gu | Lowercase |
| gU | Uppercase |
| J | Join two lines |
| ctrl-a | Increments the number under cursor |
| ctrl-x | Decrements the number under cursor |
| . | Repeat last command |
| == | Fix line indentation |
| > | Indent line or selection |
| < | Unindent line or selection |
| gf | Open file under cursor |
| gg=G | Go to the first line and fix indentation to the last one (everything) |
| ctrl-n | (Insert mode) Complete next coincidence of typed word |
| ctrl-p | (Insert mode) Complete previous coincidence of typed word |
| gd | Go to declaration of identifier under cursor |
| gD | Go to global declaration of identifier under cursor |

## Powerful Ex commands

Ex commands are commands executed with `:`. The \<search> pattern may be a regular expression:

| Command | Action |
|---------|--------|
| :m | Move line |
| :s/\<search>/\<replace>/[\<flags>] | Substitute \<search> with \<replace>. Flags: g = all occurrences in the line (not only the first), c = Ask for confirmation |
| :normal \<normal_cmd> | Executes \<normal_cmd> (normal mode command) on every line selected |
| :g/\<search>/\<ex_cmd> | Global. Executes \<ex_cmd> (command line command) on every line with \<search> |
| :g!/\<search>/\<ex_cmd> | Global. Executes \<ex_cmd> (command line command) on every line without \<search> |

E.g:
- `:m$` Move the current line to the last line.
- `:%s/dog/cat/gc` In all the lines, substitute all 'dog' occurrences with 'cat' asking for confirmation.
- `:%normal 0daW` In all the lines, go to the beginning and delete a WORD.
- `:g/dog/d` Delete all lines containing 'dog'.
- `:g!/cat/d` Delete all lines not containing 'cat'. 
- `:g/cat/y A` Copy all lines containing 'cat' (the final A appends lines to te register).
- `g/cat/normal ICat here! -> ` Insert 'Cat here! -> ' at the beginning of all the lines containing 'cat'.
- `g/^#.*\.$/s/.$//` In all the lines that begin with '#' and end with '.', replace the final '.' with nothing.

To insert an `ESC` in your \<cmd> sequence, hit `ctrl-v + ESC`.

## Shell commands

The `!` command lets you execute shell commands over your buffers. A few examples:

`:%!python -m json.tool` Fix JSON format passing all lines (%) to the python command.  
`:1,5!column -t` Indent/align in columns the lines 1 to 5 using the column command.  
`!!cat /home/user/foo.txt` Replaces the current line with the contents of '/home/user/foo.txt'.

Tip: Select first the characters/lines/blocks where you want to apply the command and then press `:`.

## Regular expressions

Regular expressions can be used in the \<search> and \<replace> patterns of commands like `/`, `?`, `:s`, `:normal` and `:g`.

### Identifiers

| Identifier | Meaning |
|------------|---------|
| . | Any single character except newline |
| * | Zero or more occurrences of any character |
| + | One or more occurrences of any character |
| \s | Blank space |
| [...] | Any single character specified in the set |
| [^...] | Any single character not specified in the set |
| ^ | Anchor, beginning of the line |
| $ | Anchor, end of line |
| \\< | Anchor, begining of word |
| \\> | Anchor, end of word |
| \(...\) | Grouping, usually used to group conditions |
| \n | Contents of nth group |

### Sets

| Set | Meaning |
|-----|---------|
| [A-Z] | The set from capital A to capital Z |
| [a-z] | The set from lowercase a to lowercase z |
| [0-9] | The set from 0 to 9 |
| [./=+] | The set containing ., /, =, and + |
| [-A-F] | The set from capital A to capital F and the dash (dashes must be specified first) |
| [0-9 A-Z] | The set containing all capital letters and digits and a space |
| [A-Z][a-zA-Z] | In the first position, the set from capital A to capital Z, in the second position, the set containing all letters |

### Examples

| RegExp | Meaning |
|--------|---------|
| /Cat/ | Matches if the line contains the string 'Cat' |
| /^CAT$/ | Matches if the line contains just the string 'CAT' |
| /\^[a-zA-Z]/ | Matches if the line starts with any letter |
| /\^[a-z].+/ | Matches if the first character of the line is a-z and there is at least one more of any character following it |
| /1234$/ | Matches if line ends with 1234 |
| /\(10&#124;20\)/ | Matches if the line contains 10 or 20. Note the use of ( ) with the pipe symbol to specify the 'or' condition |
| /[0-9]*/ | Matches if there are zero or more numbers in the line |
| /\^[^#]/ | Matches if the first character is not a '#' in the line |

Notes:
- Regular expressions are case sensitive.
- Regular expressions are to be used where a pattern is specified.

## Tips

- Avoid to use the keys on the right side of the keyboard (insert, home, page-up, numpad, etc). `ESC` is your friend.
- The first thing to learn: Move properly through the buffer:
  - Use motion commands (`gg`, `G`, `H`, `M`, `L`, `w`, `W`, `f<char>`, `%`, `}`, `]]`, etc).
  - Use search commands (`/`, `?`, `*`, `#`) to reach far text.
  - Use marks to jump between important points of your buffer.
  - Decrease the key repeat delay and increase the key repeat speed of your keyboard config to move faster.
- Until you are comfortable moving and yanking, use the selection mode to see what you are yanking. Remember that the `0` register always store the content of your last yank command.
- Use named registers to copy content that you will need later, so you can yank and delete freely to the default register.
- Working with the vim windows is really easy with a few keystrokes, don't be scared of it.
- If you have ssh access to a remote server where you want to edit files, you can of course install your vim config in it but I prefer to mount files locally using `sshfs` so you can use your local vim to edit files.
- Vim is HUGE and has a slow learning curve at the beginning, but with a bit of effort it is impressively rewarding.

---

<center>Jose Manuel García Maleno - josemanuelgm@protonmail.com</center>

