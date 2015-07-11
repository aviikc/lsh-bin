# Various Minor Scripts

I prefer using tiny scripts to aliases because it saves me going around to all my open terminals and re-sourcing .profile, and I have them available from contexts that aren't my main shell. This is the collection of those scripts that aren't machine-specific.

Fair warning: these are [shop built jigs](http://robnapier.net/go-is-a-shop-built-jig) and may lack niceties like polite usage strings when given bad parameters. 

## The Scripts

### backup

Perform an rsync backup to 192.168.1.254:backup/$HOST (hardcoded, sorry).

Usage: `backup home` or `backup full`

### bringwindow

Use vmenu to choose from a list of existent X clients to switch to, or bring to the current workspace.

Usage: `bringwindow -a` to switch to window, or `bringwindow -R` to bring the window to your current workspace.

### drawer

Manages 'drawer terminal' type programs, like a generalized version of `guake` or so.

Usage: `drawer NAME [SIDE WIDTH HEIGHT COMMAND]`

`NAME` is a symbolic name for the drawer. It should be a valid POSIX filename.
`SIDE` is the side of the screen to attach the drawer to. Valid values are `left`, `right`, `top`, and `bottom`.
`WIDTH` and `HEIGHT` may be either a size in pixels or a percentage of screen size (`84%`) specifying the dimensions of the window to create.
`COMMAND` is the command with arguments which will actually create the window.

When run, `drawer` looks in `/tmp/drawers.wids/` for a file called `NAME` containing an X window id. If found, that window's `hidden` EWMH hint will be toggled. If the file is not found, `COMMAND` is executed and the resulting window is positioned according `SIDE`, `WIDTH`, and `HEIGHT` and then focused, and the windows X window id is written to `/tmp/drawers.wids/$NAME` to allow future toggling.

Drawer requires `xwininfo` (probably included in your distro's X11 utils package), [wmctrl](http://tomas.styblo.name/wmctrl/) and [my fork of xtoolwait](https://github.com/lharding/xtoolwait) (although I have a pull request pending to merge the changes), and, optionally, [xdotool](http://www.semicomplete.com/projects/xdotool/) (to prod certain window managers into making the created window floating).

### journal

Launch gvim on a new file under `~/writing/journal` with a name generated by the `logdate` command.

### logdate

Output the current time in `YYYYMMDD:HHMM.SS (epoch+UNIXTIME)` format.

### mknote

Use zsh `vared` and `logdate` to append a timestamped line to `~/writing/notes.txt`.

### noteloop

Just runs `mknote` in a loop, for e.g. keeping a drawer term to collect and show notes.

### rclip

Remote clipboard with logging; replaces Pocket, Pinboard, etc with a very small shell script.

Requires `ssh` (public key auth highly recommended) and `xclip` to use clipboard manipulation funcionality.

To use, put in `PATH` on each host you wish to use it on, and also on a net-accessible server, and update the `RSH_CMD` variable at the head of the script with the `ssh` command necessary to log into your server, and `touch rclip.clip; touch rclip.log`. Then:

`rclip copy SELECTION`: store the contents of SELECTION to your `rclip` server.
`rclip paste SELECTION`: read the contents of the `rclip` clip from your server into SELECTION.
`rclip put`: read from `STDIN` and store to the `rclip` clip on your server.
`rclip get`: print the contents of the  `rclip` clip from your server to `STDOUT`.
`rclip tail`: follow the `rclip` log, printing each new clip as it's stored.

`SELECTION` is one of the standard X selection names as understood by `xclip`: `primary`, `secondary`, `clipboard`. Defaults to `clipboard`.

### scut (Super | Simple | Suckless) cut.

An improved version of the POSIX `cut` command.

Usage: `scut FIELDSPEC,[FIELDSPEC[,...]] [-dDELIMITER] [-jJOINER]`

`FIELDSPEC` is either an integer, or a range of the form `n-m`. If it is an integer, that field (zero-based) from each line of `STDIN` will be printed to `STDOUT`. If it is a range, fields `n` (inclusive) through `m` (exclusive) will be printed. `n` also allows the special field separator `^` which causes all fields from the beginning of the line until field `m` to be printed, verbatim, without replacing field separators with field joiners. Similarly, `m` supports the special value `$` which prints from field `n` through the end of the line, verbatim.

`DELIMITER` is any regex supported by the Pyton 2 regex module and is used to split input lines into fields.
`JOINER` is the string used to join fields together in the output. Escape sequences such as '\t' will be processed into their represented characters.

Unlike the `cut` command, fields specified multiple times will be printed multiple times.

### shistory

Use `vmenu` to select a line from `STDIN` (assumed to be output from the `history` command) and print it to `STDOUT`. Intended to be used from your shell's `readline` to place a selection from your history on the command line for editing.

### tman

Run `man` with the given arguments, and display the outputs in a new terminal window.

### vmenu

Run `dmenu` in vertical format, with multi-token matching, and the given arguments.

### www-browser

Invoke `$BROWSER` with the given arguments.
