default:

```sh
if test -f /etc/profile.d/git-sdk.sh
then
	TITLEPREFIX=SDK-${MSYSTEM#MINGW}
else
	TITLEPREFIX=$MSYSTEM
fi

if test -f ~/.config/git/git-prompt.sh
then
	. ~/.config/git/git-prompt.sh
else
	PS1='\[\033]0;$TITLEPREFIX:$PWD\007\]' # set window title
	PS1="$PS1"'\n'                 # new line
	PS1="$PS1"'\[\033[32m\]'       # change to green
	PS1="$PS1"'\u@\h '             # user@host<space>
	PS1="$PS1"'\[\033[35m\]'       # change to purple
	PS1="$PS1"'$MSYSTEM '          # show MSYSTEM
	PS1="$PS1"'\[\033[33m\]'       # change to brownish yellow
	PS1="$PS1"'\w'                 # current working directory
	if test -z "$WINELOADERNOEXEC"
	then
		GIT_EXEC_PATH="$(git --exec-path 2>/dev/null)"
		COMPLETION_PATH="${GIT_EXEC_PATH%/libexec/git-core}"
		COMPLETION_PATH="${COMPLETION_PATH%/lib/git-core}"
		COMPLETION_PATH="$COMPLETION_PATH/share/git/completion"
		if test -f "$COMPLETION_PATH/git-prompt.sh"
		then
			. "$COMPLETION_PATH/git-completion.bash"
			. "$COMPLETION_PATH/git-prompt.sh"
			PS1="$PS1"'\[\033[36m\]'  # change color to cyan
			PS1="$PS1"'`__git_ps1`'   # bash function
		fi
	fi
	PS1="$PS1"'\[\033[0m\]'        # change color
	PS1="$PS1"'\n'                 # new line
	PS1="$PS1"'$ '                 # prompt: always $
fi

MSYS2_PS1="$PS1"               # for detection by MSYS2 SDK's bash.basrc

# Evaluate all user-specific Bash completion scripts (if any)
if test -z "$WINELOADERNOEXEC"
then
	for c in "$HOME"/bash_completion.d/*.bash
	do
		# Handle absence of any scripts (or the folder) gracefully
		test ! -f "$c" ||
		. "$c"
	done
fi

```
## Bash prompt escape characters

```
\a     an ASCII bell character (07)
\d     the date in "Weekday Month Date" format (e.g., "Tue May 26")
\e     an ASCII escape character (033)
\h     the hostname up to the first '.'
\H     the hostname
\j     the number of jobs currently managed by the shell
\l     the basename of the shell's terminal device name
\n     newline
\r     carriage return
\s     the name of the shell, the basename of $0 (the portion following the final slash)
\t     the current time in 24-hour HH:MM:SS format
\T     the current time in 12-hour HH:MM:SS format
\@     the current time in 12-hour am/pm format
\u     the username of the current user
\v     the version of bash (e.g., 2.00)
\V     the release of bash, version + patchlevel (e.g., 2.00.0)
\w     the current working directory
\W     the basename of the current working directory
\!     the history number of this command
\#     the command number of this command
\$     if the effective UID is 0, a #, otherwise a $
\nnn   the character corresponding to the octal number nnn
\\     a backslash
\[     begin a sequence of non-printing characters
\]     end a sequence of non-printing characters
```

## ASCII color code 

| Code | Color   |
| ---- | ------- |
| 30   | Black   |
| 31   | Red     |
| 32   | Green   |
| 33   | Yellow  |
| 34   | Blue    |
| 35   | Magenta |
| 36   | Cyan    |
| 37   | White   |
