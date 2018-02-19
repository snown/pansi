% PANSI(1) Pansi User Manual
% Logan Holmes
% February 15, 2018

# NAME

pansi - The print command for your ansi codes

# SYNOPSIS

pansi [*-h|--help*]  
pansi --color-table  
pansi [*options*] <*text*> [[*options*] <*text*>]...  

# DESCRIPTION

Pansi prints text to the screen, similar to `printf` or `echo`, but with options to also include
ANSI control commands, setting the text colors, attributes, and controlling the cursor.

Pansi also provides a nice color table to show what text features are available in the current
terminal, and how the colors look.

# OPTIONS

-h, \--help
: Print a brief reminder help message

\--color-table
: Print out a nicely formatted table of available colors and attributes.

\--no-restore
:	Prevent the defaults from being set at the end of the command. By default the ANSI codes are cleared out at the end because typically you just want to modify the text passed to it, but sometimes it's nice to keep some of these settings around for subsequent text.

-n, \--new-line
:	Insert a newline after the command finishes. By default no new line is inserted so that you can continue writing, and potentially use pansi mid-text.  

	This only occurs at the very end of the ${script_name} command, so if you are using the chaining 
	format the newline won't be inserted until after everything.

## Attributes:

\--bold
:	Make text bold

\--faint, \--dim
:	Make text faint. Kind of the opposite of **--bold**

\--italic
:	Make text italic

\--fraktur
:	Change text to fraktur [Not often supported]

\--underline
:	Make text underlined

\--double-underline
:	Make text underlined twice! [Not often supported]

\--blink
:	Make text blink

\--reverse, \--inverse
:	Invert background and foreground colors

\--invisible, \--conceal
:	Make text hidden

\--strikethrough, \--crossedout
:	Make text crossed out

\--frame
:	Make text framed [Not often supported]

\--encircle
:	Make text encircled [Not often supported]

\--overline
:	Make text overlined [Not often supported]

## Colors:

\--fg <*colorname*>[*-intense*]
:	Set the foreground text color to the specified *colorname*.
	
    The color name options are:
    *black*, *red*, *green*, *yelow*, *blue*, *magenta*, *cyan*, and *white*.
	
	Additionally you can get an alternate (lighter/brighter) color by appending "-intense" to the colorname.

\--default
:	Reset foreground text color to the terminal's default color

## Background Color:

All background colors except 'default' can optionally have '-intense' appended to the end of the option. Doing so will use the alternative (lighter/brighter) version of the specified color.

\--bg <*colorname*>[*-intense*]
:	Set the background text color to the specified *colorname*.
	
    The color name options are:
    *black*, *red*, *green*, *yelow*, *blue*, *magenta*, *cyan*, and *white*.
	
	Additionally you can get an alternate (lighter/brighter) color by appending "-intense" to the colorname.

\--bg-default
:	Reset text background color to the terminal's default color

## Color Aliases:

For both foreground and background, there is a set of grey color aliases, that map to the following colors:

grey, gray
:	Alias for "white"

dark-grey, dark-gray
: Alias for "black-intense"

Additionally there is an alternative way to call for the "intense" colors, by prefixing the color name with either "light-" or "bright-"

**--fg** light-red, **--fg** bright-red  
**--bg** light-blue, **--bg** bright-blue

## Cursor:

Optional number arguments on cursor options will default to "1"

\--up [*n*]
:	Move cursor up by "n" rows

\--down [*n*]
:	Move cursor down by "n" rows

\--forward, \--fwd, \--right [*n*]
:	Move cursor forward by "n" columns

\--backward, \--back, \--left [*n*]
:	Move cursor backward by "n" columns

\--next-line, \--next-row [*n*]
:	Move cursor to the beginning of the line "n" rows down

\--prev-line, \--prev-row [*n*]
:	Move cursor to the beginning of the line "n" rows up

\--row, \--line [*n*]
:	Move cursor to the "n" row. Not relative to current position

\--column, \--col [*n*]
:	Move cursor to the "n" column. Not relative to current position

\--position [[*row*][*,col*]]
:	Move cursor to the specified "row" and "col". Missing options default to "1", if ommitted completely it will default to position "1,1"

\--tab-forward, \--tab-fwd, \--tab-right [*n*]
:	Move cursor forward "n" tab stops

\--tab-backward, \--tab-back, \--tab-left [*n*]
:	Move cursor backward "n" tab stops

\--save-cursor
:	Saves the cursor position. Restores the cursor after command finishes, unless "--no-restore" is also used

\--restore-cursor
:	Restore the cursor to it's saved position

\--hide-cursor
:	Hide the cursor during command execution. Cursor will be shown after the command finishes, unless "--no-restore" is also used

\--show-cursor
:	Show a cursor if hidden

## Display Manipulation:

--insert-chars [*n*]
:	Insert blank characters, shifting the line right

--erase-chars [*n*]
:	Erase "n" characters

--repeat-char [*n*]
:	Repeat the last character "n" times

--erase-display [*mode*]
:	Clears part of the screen according to "mode", cursor position does not change:

	|      Mode | Result                                                                       |
	|----------:|:-----------------------------------------------------------------------------|
	|   down, 0 | From the cursor to the bottom of the screen (**Default**)                    |
	|     up, 1 | From the cursor to the beginning of the screen                               |
	|    all, 2 | Entire visible screen, sometimes placing the cursor in the upper left corner |
	| scroll, 3 | Same as 'all' but also clears the scrollback buffer                          |


--erase-line [*mode*]
:	Erase part of the line according to the 'mode', cursor position does not change:
	
	|     Mode | Result                                               |
	|---------:|:-----------------------------------------------------|
	| right, 0 | From the cursor to the end of the line (**Default**) |
	|  left, 1 | From the cursor to the beginning of the line         |
	|   all, 2 | The entire line                                      |

--delete-lines [*n*]
:	Erase "n" lines starting with current and moving up. Lines below move up

--insert-lines [*n*]
:	Add "n" blank lines above current. The current line and below move down

--scroll-up [*n*]
:	Entire display moved up, new lines added at bottom of screen

--scroll-down [*n*]
:	Entire display moved down, new lines added at top of screen

## Resets:

\--reset-foreground, \--reset-fg
:	Reset foreground text color to the terminal's default color. (Alias for **--default**)

\--reset-color
:	Reset foreground and background color to default.

\--reset-text
:	Reset foreground, background, and attributes to default.

\--reset
:	Reset everything (color, attributes, and cursor)

# EXAMPLES

Print a installer progress message, with a bold blue arrow, then default description text:

	pansi --fg blue --bold "==>" --reset-text " Starting a new installation"

Piping Pansi to other tools:

	cat <<< "$(pansi --underline "Holiday"): $(pansi --fg bright-green "St. Patrick's Day")"

Progress Indicator:

	progress=0
	meter_width=80
	readout=$(( ${meter_width} - 3 ))
	fill_space=$(( ${meter_width} - 7 ))

	pansi --no-restore --hide-cursor --bold "[" --forward ${fill_space} "] 0%"
	while [[ ${progress} -le 100 ]]; do
	  pansi --no-restore --column ${readout} --erase-line right "${progress}%" --column 2
  
	  fill=$(printf "%.0f" $(bc -l <<< "(${progress} / 100) * ${fill_space}"))
	  if [[ ${fill} -le 1 ]]; then
	    printf "%s" ">"
	  elif [[ $progress -lt 25 ]]; then
	    pansi --no-restore --erase-chars ${fill_space} "=" --repeat-char $(( ${fill}-1 )) ">"
	  elif [[ $progress -lt 50 ]]; then
	    pansi --no-restore --erase-chars ${fill_space} --fg blue "=" --repeat-char $(( ${fill}-1 )) ">"
	  elif [[ $progress -lt 75 ]]; then
	    pansi --no-restore --erase-chars ${fill_space} --fg cyan "=" --repeat-char $(( ${fill}-1 )) ">"
	  elif [[ $progress -lt 100 ]]; then
	    pansi --no-restore --erase-chars ${fill_space} --fg green "=" --repeat-char $(( ${fill}-1 )) ">"
	  elif [[ $progress -eq 100 ]]; then
	    pansi --newline --col --fg green "[=" --repeat-char $(( ${fill_space}-1 )) "]"
	  fi
  
	  sleep 0.01
	  (( progress++ ))
	done
		