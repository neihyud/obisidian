---
title: Vim
tags: [Devops]

---

###### tags: `Devops`

# Vim
- **Quy ước**: 
    - [ _ text]: có hoặc không
    - [ + Tab]: keyboard 
    - [ @number]: chỉ số


History => !<stt_cua_lenh> 
Hoặc ctrl+r => đánh 1 phần lệnh

---
| Chế độ	| Mô tả 
| -------- | -------- 
| Normal     | Mặc định; để điều hướng và chỉnh sửa đơn giản
|Insert | Để chèn và sửa đổi văn bản rõ ràng
|Command Line | Đối với các hoạt động như saving, exiting, etc

- **vim [name_file]**: open file
- **vim -o[number] [ _ file_name ...]**: mở [number] window ngang
- **:w [name_file]** = write, đặt tên cho file hiện tại
- **:wq** = write and quit
- **:q!** = exit not save

- **Tạm dừng vim**: **ctrl + Z**, **:stop**, **:suspend**
    => quay lại: ở terminal run **`fg`**
    
## Buffer
https://github.com/iggredible/Learn-Vim/blob/master/ch02_buffers_windows_tabs.md
- Khi bạn mở một tệp trong Vim, dữ liệu sẽ được liên kết với bộ đệm. Khi bạn mở 3 tệp trong Vim, bạn có 3 bộ đệm.
- vim file1 file2: vim hiển thị file1 n tạo ra 2 buffer (:ls để hiển thị)
- **:split [name_file]**: thêm một window để xem [name_file] theo chiều dọc
- **:bnext** = tiến bộ đệm tiếp theo
- **:bprevious** = đi đến bộ đệm trước đó
- **:buffer + n**: đưa đến bộ đệm n
---
- **:tabnew [name_file]**: thêm một window mới theo chiều ngang
- **:tabclose**: close tab hiện tại
- **:tabnext**: go to next tab
- **:tabprevios**: go to previous tab
- **:tablast**: last tab
- **:tabfirst**: first tab
---
- **:qall**: thoát tất cả buffers
- **:qall!**: thoát không lưu
- **:wqall**: lưu và thoát tất cả

## Search File
### Mở và chỉnh sửa tệp
- **`:edit [name_file]`**: mở file nếu tồn tại hoặc tạo mới
- **`:edit *.[extension][+Tab]`**: tìm kiếm các file có phần mở rộng (extension) trong folder hiện tại
- **`:edit **/*.[extension][+Tab]`**: tìm kiếm trong project
- **`:edit [path_folder]`**: trình khám phá tệp (gui)
### Find and Path
- `:find` tìm file ở dạng path còn `:edit` thì không
- **`:vim /pattern/ [path_file]`**: tìm kiếm tất cả trong file thoả mãn pattern (**/*. => tìm kiếm trong tất cả folder)
    - :copen        Open the quickfix window
    - :cclose       Close the quickfix window
    - :cnext        Go to the next error
    - :cprevious    Go to the previous error
    - :colder       Go to the older error list
    - :cnewer       Go to the newer error list
- **`:grep -R "pattern" [file_path]`**: tg tự trên

### Browsing File With Netrw
- `**:Explore**`     Starts netrw on current file
- `**:Sexplore**`    No kidding. Starts netrw on split top half of the screen
- `**:Vexplore**`    Starts netrw on split left half of the screen

## Gramma

- Quy tắc: Verd + Nount

```
- Danh từ

h    Left
j    Down
k    Up
l    Right
w    Move forward to the beginning of the next word
}    Jump to the next paragraph
$    Go to the end of the line
G    Go to the end of the document
gg    Move first document
```

```
- Động từ

y    copy text (yank)
d    Delete text and save to register
c    Delete text, save to register, and start insert mode
p    Paste
u    Undo
```

- **y$**: copy mọi thứ từ vị trí hiện tại đến cuối dòng
- **yy**: copy toàn bộ dòng
- **d[ @n]w**: xoá vị trí hiện tại đến n từ tiếp theo ( n có thể empty)
- **dd**: xoá toàn bộ dòng
- **cc**: xoá toàn bộ dòng và bật insert
- **c}**: xoá vị trí hiện tại đến cuối đoạn hiện tại và bật insert
- **[ @n]G**: chuyển đến dòng n

- Có hai loại đối tượng văn bảng: đối tượng bên trong và đối tượng bên ngoài

```
i + object    Inner text object
a + object    Outer text object
```
    
    - Đối tượng văn bản nội bộ (Inner text object) chọn đối tượng bên trong mà không bao gồm khoảng trắng hoặc các đối tượng xung quanh. 
    - Đối tượng văn bản bên ngoài (Outer text object) chọn đối tượng bên trong bao gồm cả khoảng trắng hoặc các đối tượng xung quanh. 

- **di{**: xoá toàn bộ nội dung trong dấu {} mà không xoá dấu {}
- **da{**: xoá toàn bộ nội dung trong dấu {} và dấu {}

- đối tượng văn bản phổ biến
```
w         A word
p         A paragraph
s         A sentence
( or )    A pair of ( )
{ or }    A pair of { }
[ or ]    A pair of [ ]
< or >    A pair of < >
t         XML tags
"         A pair of " "
'         A Pair of ' '
`         A pair of ` `
```

```
Chế độ visual mode:
v: chế độ visual mode để chọn văn bản.
V: chọn toàn bộ dòng.
Ctrl + v: chế độ visual block để chọn một khối văn bản.
```

## Moving in File

```
[count] + motion

w     Move forward to the beginning of the next word
W     Move forward to the beginning of the next WORD
e     Move forward one word to the end of the next word
E     Move forward one word to the end of the next WORD
b     Move backward to beginning of the previous word
B     Move backward to beginning of the previous WORD
ge    Move backward to end of the previous word
gE    Move backward to end of the previous WORD

0     Go to the first character in the current line
^     Go to the first nonblank char in the current line
g_    Go to the last non-blank char in the current line
$     Go to the last char in the current line
n|    Go the column n in the current line

f    Search forward for a match in the same line
F    Search backward for a match in the same line
t    Search forward for a match in the same line, stopping before match
T    Search backward for a match in the same line, stopping before match
;    Repeat the last search in the same line using the same direction
,    Repeat the last search in the same line using the opposite direction

H     Go to top of screen
M     Go to medium screen
L     Go to bottom of screen
nH    Go n line from top
nL    Go n line from bottom
```

### Cuộn
```
Ctrl-E    Scroll down a line
Ctrl-D    Scroll down half screen
Ctrl-F    Scroll down whole screen
Ctrl-Y    Scroll up a line
Ctrl-U    Scroll up half screen
Ctrl-B    Scroll up whole screen

zt    Bring the current line near the top of your screen
zz    Bring the current line to the middle of your screen
zb    Bring the current line near the bottom of your screen
```
### Find

```
/    Search forward for a match
?    Search backward for a match
n    Repeat last search in same direction of previous search
N    Repeat last search in opposite direction of previous search


*     Search for whole word under cursor forward
#     Search for whole word under cursor backward
g*    Search for word under cursor forward
g#    Search for word under cursor backward

```
==> nhập từ tìm kiếm thì nhấn n để tìm

### Mark

```
mX    Mark position with mark "X", X can a-zA-Z
`a    Jump to line and column "a"
'a    Jump to line "a"
```
### Jump

```
'       Go to the marked line
`       Go to the marked position
G       Go to the line
/       Search forward
?       Search backward
n       Repeat the last search, same direction
N       Repeat the last search, opposite direction
%       Find match
(       Go to the last sentence
)       Go to the next sentence
{       Go to the last paragraph
}       Go to the next paragraph
L       Go to the the last line of displayed window
M       Go to the middle line of displayed window
H       Go to the top line of displayed window
[[      Go to the previous section
]]      Go to the next section
:s      Substitute
:tag    Jump to tag definition
```

## Inser Mode

```
i    Insert text before the cursor
I    Insert text before the first non-blank character of the line
a    Append text after the cursor
A    Append text at the end of line
o    Starts a new line below the cursor and insert text
O    Starts a new line above the cursor and insert text
s    Delete the character under the cursor and insert text
S    Delete the current line and insert text, synonym for "cc"
gi   Insert text in same position where the last insert mode was stopped
gI   Insert text at the start of line (column 1)
```

### Delete chunk in Insert Mode


```
Ctrl-h    Delete one character
Ctrl-w    Delete one word
Ctrl-u    Delete the entire line
```

## Auto complete

```
Ctrl-N             Find the next word match
Ctrl-P             Find the previous word match
```
























































































































































































































































































## ./vimrc

```vim=
set hidden

"automatic install"
let data_dir = has('nvim') ? stdpath('data') . '/site' : '~/.vim'
if empty(glob(data_dir . '/autoload/plug.vim'))
  silent execute '!curl -fLo '.data_dir.'/autoload/plug.vim --create-dirs  https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

call plug#begin('~/.vim/plugged')
Plug 'junegunn/fzf.vim'
call plug#end()

"relativenumber/ norelativenumber, number/ nonumber => format line"
set relativenumber number 
```

-:noh: tắt đánh dấu


## Command
```
# CURSOR MOVEMENTS
##############################################################################


h                   move left
j                   move down
k                   move up
l                   move right
w                   jump by start of words (punctuation considered words)
W                   jump by words (spaces separate words)
e                   jump to end of words (punctuation considered words)
E                   jump to end of words (no punctuation)
b                   jump backward by words (punctuation considered words)
B                   jump backward by words (no punctuation)
ge                  jump backward to end of words
0                   (zero) start of line
^                   first non-blank character of line
$                   end of line
-                   move line upwards, on the first non blank character
+                   move line downwards, on the first non blank character
<enter>             move line downwards, on the first non blank character
gg                  go to first line
G                   go to last line
ngg                 go to line n
nG                  go To line n
:n                  go To line n
)                   move the cursor forward to the next sentence.
(                   move the cursor backward by a sentence.
{                   move the cursor a paragraph backwards
}                   move the cursor a paragraph forwards
]]                  move the cursor a section forwards or to the next {
[[                  move the cursor a section backwards or the previous {
CTRL-f              move the cursor forward by a screen of text
CTRL-b              move the cursor backward by a screen of text
CTRL-u              move the cursor up by half a screen
CTRL-d              move the cursor down by half a screen
H                   move the cursor to the top of the screen.
M                   move the cursor to the middle of the screen.
L                   move the cursor to the bottom of the screen.
fx                  search line forward for 'x'
Fx                  search line backward for 'x'
tx                  search line forward before 'x'
Tx                  search line backward before 'x'


##############################################################################
# BOOKMARKS
##############################################################################


:marks              list all the current marks
ma                  make a bookmark named a at the current cursor position
`a                  go to position of bookmark a
'a                  go to the line with bookmark a
`.                  go to the line that you last edited


##############################################################################
# INSERT MODE
##############################################################################


i                   start insert mode at cursor
I                   insert at the beginning of the line
a                   append after the cursor
A                   append at the end of the line
o                   open (append) blank line below current line
O                   open blank line above current line
Esc                 exit insert mode


##############################################################################
# EDITING
##############################################################################


r                   replace a single character (does not use insert mode)
R                   enter Insert mode, replacing characters rather than inserting
J                   join line below to the current one
cc                  change (replace) an entire line
cw                  change (replace) to the end of word
C                   change (replace) to the end of line
ct'                 change (replace) until the ' character (can change ' for any character)
s                   delete character at cursor and substitute text
S                   delete line at cursor and substitute text (same as cc)
xp                  transpose two letters (delete and paste, technically)
u                   undo
CTRL-r              redo
.                   repeat last command
~                   switch case
g~iw                switch case of current word
gUiw                make current word uppercase
guiw                make current word lowercase
gU$                 make uppercase until end of line
gu$                 make lowercase until end of line
>>                  indent line one column to right
<<                  indent line one column to left
==                  auto-indent current line
ddp                 swap current line with next
ddkp                swap current line with previous
:%retab             fix spaces / tabs issues in whole file
:r [name]           insert the file [name] below the cursor.
:r !{cmd}           execute {cmd} and insert its standard output below the cursor.


##############################################################################
# DELETING TEXT
##############################################################################


x                   delete current character
X                   delete previous character
dw                  delete the current word
dd                  delete (cut) a line
dt'                 delete until the next ' character on the line (replace ' by any character)
D                   delete from cursor to end of line
:[range]d           delete [range] lines


##############################################################################
# COPYING AND MOVING TEXT
##############################################################################


yw                  yank word
yy                  yank (copy) a line
2yy                 yank 2 lines
y$                  yank to end of line
p                   put (paste) the clipboard after cursor/current line
P                   put (paste) before cursor/current line
:set paste          avoid unexpected effects in pasting
:registers          display the contents of all registers
"xyw                yank word into register x
"xyy                yank line into register x
:[range]y x         yank [range] lines into register x
"xp                 put the text from register x after the cursor
"xP                 put the text from register x before the cursor
"xgp                just like "p", but leave the cursor just after the new text
"xgP                just like "P", but leave the cursor just after the new text
:[line]put x        put the text from register x after [line]


##############################################################################
# MACROS
##############################################################################


qa                  start recording macro 'a'
q                   end recording macro
@a                  replay macro 'a'
@:                  replay last command


##############################################################################
# VISUAL MODE
##############################################################################


v                   start visual mode, mark lines, then do command (such as y-yank)
V                   start linewise visual mode
o                   move to other end of marked area
U                   upper case of marked area
CTRL-v              start visual block mode
O                   move to other corner of block
aw                  mark a word
ab                  a () block (with braces)
ab                  a {} block (with brackets)
ib                  inner () block
ib                  inner {} block
Esc                 exit visual mode

VISUAL MODE COMMANDS
--------------------

>                   shift right
<                   shift left
c                   change (replace) marked text
y                   yank (copy) marked text
d                   delete marked text
~                   switch case

VISUAL MODE SHORTCUTS
---------------------

v%                  selects matching parenthesis
vi{                 selects matching curly brace
vi"                 selects text between double quotes
vi'                 selects text between single quotes

##############################################################################
# SPELLING
##############################################################################


]s                  next misspelled word
[s                  previous misspelled word
zg                  add word to wordlist
zug                 undo last add word
z=                  suggest word


##############################################################################
# EXITING
##############################################################################


:q                  quit Vim. This fails when changes have been made.
:q!                 quit without writing.
:cq                 quit always, without writing.
:w                  save without exiting.
:wq                 write the current file and exit.
:wq!                write the current file and exit always.
:wq {file}          write to {file}. Exit if not editing the last
:wq! {file}         write to {file} and exit always.
:[range]wq[!]       same as above, but only write the lines in [range].
ZZ                  write current file, if modified, and exit.
ZQ                  quit current file and exit (same as ":q!").


##############################################################################
# SEARCH/REPLACE
##############################################################################


/pattern                    search for pattern
?pattern                    search backward for pattern
n                           repeat search in same direction
N                           repeat search in opposite direction
*                           search forward, word under cursor
#                           search backward, word under cursor
set ic                      ignore case: turn on
set noic                    ignore case: turn off
:%s/old/new/g               replace all old with new throughout file
:%s/old/new/gc              replace all old with new throughout file with confirmation
:argdo %s/old/new/gc | wq   open multiple files and run this command to replace old 
                            with new in every file with confirmation, save and quit


##############################################################################
# MULTIPLE FILES
##############################################################################


:e filename         edit a file in a new buffer
:tabe filename      edit a file in a new tab (Vim7, gVim)
:ls                 list all buffers
:bn                 go to next buffer
:bp                 go to previous buffer
:bd                 delete a buffer (close a file)
:b1                 show buffer 1
:b vimrc            show buffer whose filename begins with "vimrc"
:bufdo <command>    run 'command(s)' in all buffers
:[range]bufdo <command> run 'command(s)' for buffers in 'range'


##############################################################################
# WINDOWS
##############################################################################


:sp f               split open f
:vsp f              vsplit open f
CTRL-w s            split windows
CTRL-w w            switch between windows
CTRL-w q            quit a window
CTRL-w v            split windows vertically
CTRL-w x            swap windows
CTRL-w h            left window
CTRL-w j            down window
CTRL-w k            up window
CTRL-w l            right window
CTRL-w +            increase window height
CTRL-w -            decrease window height
CTRL-w <            increase window width
CTRL-w >            decrease window width
CTRL-w =            equal window
CTRL-w o            close other windows
zz                  Centers the window to the current line


##############################################################################
# QUICKFIX WINDOW
##############################################################################


copen               open quickfix window
cclose              close quickfix window
cc [nr]             display error [nr]
cfirst              display the first error
clast               display the last error
[count]cn           display [count] next error
[count]cp           display [count] previous error


##############################################################################
# PROGRAMMING
##############################################################################


%                   show matching brace, bracket, or parenthese
gf                  edit the file whose name is under or after the cursor
gd                  when the cursor is on a local variable or function, jump to its declaration
''                  return to the line where the cursor was before the latest jump
gi                  return to insert mode where you inserted text the last time
CTRL-o              move to previous position you were at
CTRL-i              move to more recent position you were at


##############################################################################
# PLUGINS > ACK
##############################################################################


:Ack                Search recursively in directory
o                   to open (same as enter)
go                  to preview file (open but maintain focus on ack.vim results)
t                   to open in new tab
T                   to open in new tab silently
q                   to close the quickfix window


##############################################################################
# PLUGINS > CHEAT
##############################################################################


:Cheat              open cheat sheet (with autocomplete)
<leader>ch          open cheat sheet for word under the cursor


##############################################################################
# PLUGINS > GIST
##############################################################################


:Gist               post whole text to gist
:Gist XXXXX         get gist XXXXX
:Gist -l            list my gists


##############################################################################
# PLUGINS > GUNDO
##############################################################################


:GundoToggle        show undo tree


##############################################################################
# PLUGINS > LUSTYJUGGLER
##############################################################################


<Leader>lj          show open buffers


##############################################################################
# PLUGINS > NERDCOMMENTER
##############################################################################


<leader>cc          comment out line(s)
<leader>c<space>    toggle the comment state of the selected line(s)


##############################################################################
# PLUGINS > NERDTREE
##############################################################################


:NERDTreeToggle     show / hide file browser
:NERDTreeFind       show current file in file browser
:Bookmark name      bookmark the current node as "name"

FILE
----

o                   open in prev window
go                  preview
t                   open in new tab
T                   open in new tab silently
i                   open split
gi                  preview split
s                   open vsplit
gs                  preview vsplit

DIRECTORY
---------

o                   open & close node
O                   recursively open node
x                   close parent of node
X                   close all child nodes of current node recursively
e                   explore selected dir

BOOKMARK
--------

o                   open bookmark
t                   open in new tab
T                   open in new tab silently
D                   delete bookmark

TREE NAVIGATION
---------------

P                   go to root
p                   go to parent
K                   go to first child
J                   go to last child
CTRL-j              go to next sibling
CTRL-k              go to prev sibling

FILESYSTEM
----------

C                   change tree root to the selected dir
u                   move tree root up a dir
U                   move tree root up a dir but leave old root open
r                   refresh cursor dir
R                   refresh current root
m                   show menu
cd                  change the CWD to the selected dir

TREE FILTERING
--------------

I                   hidden files
f                   file filters
F                   files
B                   bookmarks

OTHER
-----

q                   close the NERDTree window
A                   zoom (maximize-minimize) the NERDTree window
?                   toggle help


##############################################################################
# PLUGINS > PDV
##############################################################################


CTRL-P              generate PHP DOC


##############################################################################
# PLUGINS > PICKACOLOR
##############################################################################


:PickHEX            choose color in system color picker


##############################################################################
# PLUGINS > SNIPMATE
##############################################################################


<tab>               expand snippet


##############################################################################
# PLUGINS > SPARKUP
##############################################################################


CTRL-e              execute sparkup (zen coding expansion)
CTRL-n              jump to the next empty tag / attribute


##############################################################################
# PLUGINS > SURROUND
##############################################################################


cs'"                change surrounding quotes to double-quotes
cs(}                change surrounding parens to braces 
cs({                change surrounding parens to braces with space
ds'                 delete surrounding quotes
dst                 delete surrounding tags
ysiw[               surround inner word with brackets
vees'               surround 2 words (ee) with quotes '


##############################################################################
# PLUGINS > TABULAR
##############################################################################


:Tabularize /,      line the selected lines up on the commas


##############################################################################
# PLUGINS > TAGLIST
##############################################################################


:TlistToggle        open / close taglist window
<enter>             jump to tag or file
<space>             display the tag prototype


##############################################################################
# PLUGINS > UNIMPAIRED
##############################################################################


[space              new line above
]space              new line below
[e                  exchange line above
]e                  exchange line below
[x                  XML encode
]x                  XML decode (with htmlentities)
[q                  jump to previous quickfix item
]q                  jump to next quickfix item
[Q                  jump to first quickfix item
]Q                  jump to last quickfix item


##############################################################################
# PLUGINS > VIM-FUGITIVE
##############################################################################


:Git                run a git command
:Gstatus            git status : - to (un)stage , p to patch, C to commit
:Gcommit            git commit
:Gread              empty the buffer and revert to the last commit
:Gwrite             write the current file and stage the results
:Gmove              git mv
:Gremove            git rm
:Glog               git log
:Gdiff              perform a vimdiff against the current file of a certain revision
:Gblame             open blame information in a scroll bound vertical splitt
:Gbrowse            open github


##############################################################################
# PLUGINS > VIM-MARKDOWN-PREVIEW
##############################################################################


:Mm                 preview markdown document in webbrowser


##############################################################################
# PLUGINS > VIM-PEEPOPEN
##############################################################################


<Leader>p           open the current directory with the peepopen application (fuzzy search)


##############################################################################
# PLUGINS > VIM-SYMFONY
##############################################################################


:Sview              open template file
:Saction            open action file
:Smodel             open model file
:Sfilter            open filter file
:Sform              open form file
:Spartial           open partial file / write selected content in partial + include
:Scomponent         open component file / write selected content in component + include
:Salternate         open alternate model file (class - table class)
:Symfony            execute task


##############################################################################
# PERSONAL .VIMRC
##############################################################################


<leader>ev          edit vimrc file
<leader>sv          reload vimrc file
<leader>sh          show syntax highlighting groups for word under cursor

<space>             page down
jj                  exit insertion mode
<leader>q           close the current window

<leader>/           clear the search register

<leader>h           toggle hidden characters 

<leader>W           strip all trailing whitespace

CTRL-h              go to left window
CTRL-j              go to down window
CTRL-k              go to top window
CTRL-l              go to right window
<leader>w           open vertical split window and activate

%%                  will expand to current directory
<leader>ew          open file from current directory
<leader>es          open file in split window from current directory
<leader>cd          change directory to parent dir of current file
##                  will expand to webroot

:Wrap               wrap text
<F2>                toggle wrapped text

<F3>                toggle spell check

<F4>                toggle light/dark background

<F5>                underline with dashes
<F6>                underline with double lines

<leader><up>        bubble line(s) up
<leader><down>      bublle line(s) down

:Ltag               load tags file
:Project            cd to project and load tags file
<leader>t           show current tag for word under cursor
<leader>st          show current tag for word under cursor in split window
<leader>tj          show current tag list for word under cursor
<leader>stj         show current tag list for word under cursor in split window

CTRL-<space>        show omnicomplete menu

<leader>b           surround with strong tags
<leader>i           surround with em tags

CTRL-p              generate PHP DOC

<leader>a           run Ack

<leader>md          preview markdown

<leader>s           preview in safari

<leader>x           colorpicker

<leader>n           toggle Nerdtree
<leader>N           close Nerdtree
<leader>f           find current file in Nerdtree

<leader>l           toggle Taglist
<leader>L           close Taglist

<leader>ph          set filetype to php.html
<leader>r           reload all snipmate snippets

CTRL-<tab>          switch between buffers

CTRL-y              go to next tag of attribute in sparkup plugin

<leader>g           toggle Gundo window

IMG<CR>             show image browser to insert image tag with src, width and height
b                   insert image tag with dimensions from NERDTree 
                    (http://stackoverflow.com/questions/5707925/vim-image-placement)
```