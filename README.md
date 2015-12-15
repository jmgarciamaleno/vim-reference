# Vim-reference.

This is my humble compilation of knowledge about vim.

## Modes.

| Mode | Definition |
|------|------------|
| Normal mode | Default mode. Move around and execute commands. Press `ESC` any time to return to it. |
| Insert mode | Text insert mode. Activated by any insert, append or open command. |
| Replace mode | Text replace mode. Activated pressing `R`. |
| Visual mode | Text selection mode. Activated with `v`, `shift-v` or `ctrl-v`. |
| Command mode | Command line mode. Activated pressing `:`. |

## Registers.

Vim has many *clipboards* called *registers*. Registers can be accesed with `"<registerId>`. Numbered registers (0 to 9) are automatically filled with text from yank and delete commands. Named registers (a to z or A to Z) are the ones you can use as you please. You can see all the registers set typing `:reg`. All the registers and their contents are stored in the *.viminfo* file so you retain them between sessions.

- The default register `"` is the *unnamed register*. When you yank (copy) or delete something you are using it.
- The `+` register (`*` in some OSs) is the system clipboard (*xterm-clipboard* feature required, check vim --version).
- The `0` register always store the content of the last yank comand.
- The `.` register stores your last typed text.

E.g:
- `"aY` Copy the current line to register a.  
- `"+p` Paste content from the register + (system clipboard).

## Objects.

| Object | Real object |
|--------|-------------|
| w | Word. |
| W | Blank-separated word. |
| s | Sentence. |
| p | Paragraph. |
| t | Tag (markup languages). |
| () | Parenthesis. |
| [] | Brackets. |
| {} | Braces. |
| '' | Quotes. |
| "" | Double quotes. |
| <> | Angle brackets. |

You can operate over any object:

- `<command>a<object>` Operate an object.
- `<command>i<object>` Operate inner/inside object.

E.g:
- `ci'` (change inside quotes) Removes the text between the next quotes and enters insert mode.
- `caw` (change a word) Removes the word under the cursor and enters insert mode.
- `vap` (select a sentence) Selects the sentence under cursor.
- `>ap` (indent a paragraph) Indents the paragraph under cursor. 

## Commands.

### Amount and range.

Most commands can be preceded by a number that specifies how many times it is to be performed. E.g:

`2l` Move the cursor 2 steps right.  
`3dd` Delete 3 lines.  
`4fa` Go forward to the fourth occurrence of 'a'.

Most commands can be preceded by a range, the command will work only in the lines within the range.

| Range | Reach |
|-------|-------|
| n,m | Lines n to m. |
| . | Current line. |
| $ | Last line. |
| % | All lines in the buffer. |

E.g:
- `:1.5d` Delete lines 1 to 5.  
- `:.,$y` Yank (copy) from current line to the last line.  
- `:%>` Indent all lines in the buffer.  

You can also select characters/lines/blocks and then press `:` to enter command mode, the selection range is automatically put before your command.

### Moving.

| Command | Action |
|---------|--------|
| h | Left. |
| j | Down. |
| k | Up. |
| l | Right. |
| - | Up, to first non-blank character. |
| + | Down, to first non-blank character. |
| 0 | Start of the current line. |
| ^ | First non blank character of the current line. |
| $ | End of the current line. |
| g\_ | Last non blank character of the current line. |
| gm | Go to the middle of the screen in the current line. |
| #&#124; | Go to the column number # in the current line. |
| f\<char> | Next occurrence of character \<char> in line (inclusive). |
| F\<char> | Previous occurrence of character \<char> in line (inclusive). |
| t\<char> | Next occurrence of character \<char> in line (exclusive). |
| T\<char> | Previous occurrence of character \<char> in line (exclusive). |
| ; | Repeat last f, F, t or T. |
| , | Repeat last f, F, t or T in the opposite direction. |
| gg | Go to the first line of the file. |
| G | Go to the last line of the file. |
| :# | Go to line number #. |
| % | Go to the matching ( ), [ ] or { }. |
| #% | Go to the line on the #% of the file (50% is the middle of the file). |
| } | Go to the next paragraph. |
| { | Go to the previous paragraph. |
| /\<text> | Search forward for \<text>. |
| ?\<text> | Search backward for \<text>. |
| /\<text>\c | Search forward for \<text> case insensitive. |
| ?\<text>\c | Search backward for \<text> case insensitive. |
| * | Search for the word under the cursor forward. |
| # | Search for the word under the cursor backward. |
| ma | Set mark a (a-z or A-Z). |
| 'a | Start of the line with mark a. |
| `a | Go to mark a. |
| :marks | View marks set. |
| H | First line of the current screen. |
| M | Middle line of the current screen. |
| L | Last line of the current screen. |
| ctrl-f | Go forward one full screen. |
| ctrl-b | Go backward one full screen. |
| ctrl-d | Go forward (down) half a screen. |
| ctrl-u | Go backward (up) half a screen. |
| ctrl-y | Move screen down without moving cursor. |
| ctrl-e | Move screen up without moving cursor. |
| z\<enter> | Move screen so the current line is first. |
| zz | Move screen so the current line is middle. |
| w | Next start of word. |
| W | Next start of blank-separated word. |
| b | Previous start of word (before). |
| B | Previous start of blank-separated word (before). |
| e | Next end of word. |
| E | Next end of blank-separated word. |
| ge | Previous end of word. |
| gE | Previous end of blank-separated word. |
| '. | Go back to last edited line. |
| g; | Go back to last edited position. |
| ctrl-w + h | Move to the left window. |
| ctrl-w + l | Move to the right window. |
| ctrl-w + k | Move to the window above. |
| ctrl-w + j | Move to the window below. |

### Editing.

| Command | Action |
|---------|--------|
| i | Insert at cursor. |
| I | Insert at the start of the line. |
| a | Append to cursor position. |
| A | Append to the end of the line. |
| x | Cut character at the cursor. |
| X | Cut character before the cursor. |
| y | Yank (copy). |
| yy or Y | Yank line. |
| d | Delete (cut). |
| dd | Delete line. |
| S | Delete line and insert mode. |
| D | Delete to the end of the line. |
| C | Change to the end of the line. |
| p | Paste after the cursor. |
| P | Paste before the cursor. |
| o | Open a new line below the current one. |
| O | Open a new line above the current one. |
| v | Visual mode, characters. |
| V | Visual mode, lines. |
| ctrl+v | Visual mode, blocks. |
| gv | Reselect previous selection. |
| o | (Visual mode) Change side of selection. |
| r | Replace the character under cursor. |
| R | Replace mode. |
| u | Undo one change. |
| U | Undo all changes in line. |
| ctrl-r | Redo one change. |
| ~ | Change case. |
| gu | Lowercase. |
| gU | Uppercase. |
| J | Join two lines. |
| ctrl-A | Increments the number under cursor. |
| ctrl-X | Decrements the number under cursor. |
| . | Repeat last command. |
| == | Fix line indentation. |
| > | Indent line or selected block. |
| < | Unindent line or selected block. |
| gf | Open file under cursor. |
| gg=G | Go to the first line and fix indentation to the last one (everything). |
| ctrl-n | (Insert mode) Complete next coincidence of typed word. |
| ctrl-p | (Insert mode) Complete previous coincidence of typed word. |
| gd | Go to declaration of identifier under cursor. |
| gD | Go to global declaration of identifier under cursor. |

### Combining commands with objects.

Some handy examples:

| Command | Action |
|---------|--------|
| dw | Delete till start of next word. |
| cw | Change till start of next word (delete and insert mode). |
| caw | Change a word. |
| gUaw | Uppercase a word. |
| dap | Delete a paragraph. |
| vip | Select inner paragraph. |
| ci" | Change inside the next double quotes. |
| cit | Change inside tag (markup languages). |
| 3cw | Change 3 words. |
| va( | Visual select () and its content. |
| 4dd | Delete 4 lines. |
| 3x | Delete 3 characters. |
| 3s | Substitute 3 characters. |
| d/foo | Delete till the next occurence of 'foo'. |
| v?foo | Visual select till the previous occurence of 'foo'. |
| df\<char> | Delete inline to next \<char> (inclusive). |
| vT\<char> | Select inline to previous \<char> (exclusive). |

### Powerful Ex Commands.

Ex commands are commands executed with `:`. The \<search> pattern may be a regular expression. These ones are awesome:

| Command | Action |
|---------|--------|
| :s/\<search>/\<replace>/[\<options>] | Substitute \<search> with \<replace>. Options: g = all occurrences in the line (not only the first), c = Ask for confirmation. |
| :norm \<cmd> | Executes \<cmd> on every line selected. |
| :g/\<search>/\<cmd> | Executes \<cmd> on every line with \<search> |
| :g!/\<search>/\<cmd> | Executes \<cmd> on every line without \<search> |

E.g:
- `:%s/dog/cat/gc` In all the lines, substitute all 'dog' occurrences with 'cat' asking for confirmation.
- `:%norm 0daW` In all the lines, go to the beginning and delete the first WORD.
- `:g/dog/d` Delete all lines containing 'dog'.
- `:g!/cat/d` Delete all lines not containing 'cat'. 
- `:g/cat/y A` Copy all lines containing 'cat' (the final A appends lines to te register).

To insert an `ESC` in your \<cmd> sequence, hit `ctrl-v + ESC`.

### Using shell commands in your buffers.

The `!` command lets you execute shell commands over your buffers. A few examples:

`:%!python -m json.tool` Fix JSON format passing all lines (%) to the python command.  
`:1,5!column -t` Indent/align in columns the lines 1 to 5 using the column command.

Tip: Select first the characters/lines/blocks where you want to apply the command and then press `:`.

### Folding.

To activate the folding capabilities in vim, the foldmethod property must be set.

`set foldmethod=syntax` Common values: indent, syntax.  
`set foldcolumn=4` Number of columns that will be used to show the indentation levels on the left.

| Command | Action |
|---------|--------|
| zo | Open fold. |
| zO | Open fold and subfolds. |
| zc | Close fold. |
| zC | Close all folds. |
| zr | Open everything one level (reduce folding). |
| zR | Open all folds. |
| zm | Close everything one level. |

### Buffers (files opened).

ball = All buffers.

| Command | Action |
|---------|--------|
| :args ~/* | Opens all files under ~/. |
| :e \<file> | Edit/reload file. |
| :ls | List buffers. |
| :b# | Go to buffer # (# can be the number or part of the buffer name). |
| :bn | Go to next buffer. |
| :bp | Go to previous buffer. |
| :bw# | Wipe out buffer #. |
| :bw * + ctrl-a | Expands * into all open buffers and wipe out all. |
| ctrl+6 | Jump between last and current buffer. |
| :bufdo \<cmd> | Executes \<cmd> in all open buffers. |

### Windows.

| Command | Action |
|---------|--------|
| ctrl-w + H | Move current window left. |
| ctrl-w + J | Move current window bottom. |
| ctrl-w + K | Move current window above. |
| ctrl-w + L | Move current window right. |
| :resize # | I.e. :resize 40. |
| :vertical resize # | I.e. :vertical resize 40. |
| ctrl-w + = | Resize all windows to the same size. |
| :sb# | Splits horizontally buffer #. |
| :vert sb# | Splits vertically buffer #. |
| :sf# | Splits horizontally file #. |
| :vert sf# | Splits vertically file #. |
| :windo \<cmd> | Executes \<cmd> in all open windows. |

### Tabs.

| Command | Action |
|---------|--------|
| :tabnew | Create tab. |
| :tabclose | Delete tab. |
| :tabn | Next tab. |
| :tabp | Previous tab. |
| :tabm # | Move tab to position #. |
| :tabm +# | Move tab # positions right. |
| :tabm -# | Move tab # positions left. |
| :tabdo \<cmd> | Executes \<cmd> in all open tabs. |

## Macro recording.

1. Press q\<key> to start recording, do things.
2. Hit q to stop recording.
3. Use @\<key> to execute the macro.

## Tips.

- Avoid to use the right keys of your keyboard (insert, home, page-up, numpad, etc). ESC is your friend.
- The first thing to learn: Move properly through the buffer. This is the best way to not miss the mouse.
- Use marks, to jump fast between important points of your buffer. Use `/` and `?` to reach far text.
- Until you are comfortable moving and yanking, use the selection mode to see what you are yanking.
- Use named registers to copy content that you will need later, so you can yank and delete freely.
- Working with the vim windows is really easy with a few keystrokes, don't be scared of it.
- If you have ssh access to a remote server where you want to edit files, you can of course install your vim config in it but I prefer to mount files locally using 'sshfs' so you can use your local vim to edit files.
- Vim is HUGE and has a slow learning curve at the beginning, but with a bit of effort it is impressively rewarding.

---

<center>Jose Manuel García Maleno - jmgarciamaleno@gmail.com - @jmgarciamaleno</center>

