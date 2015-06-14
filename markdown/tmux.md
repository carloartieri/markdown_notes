#tmux
---

##Installation
To install tmux on OS X, first install [homebrew](http://brew.sh/):

	ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	
Then use homebrew to install tmux:

	brew install tmux
	
</br>
##Configuration

To **change** tmux defaults, create the file **~/.tmux.conf**. The tmux configuration file is simply a string of tmux commands that are run, in order, when tmux launches. The configuration file can be reloaded from within a tmux session with:

	tmux source-file ~/.tmux.conf

The following is my ~/.tmux.conf, which shows the options that I've set: 

	#Set tmux keys to 'vi' mode
	set-option -g status-keys vi
	set-option -g mode-keys vi

	#Enable mouse support
	set-window-option -g mode-mouse on
	set-option -g mouse-select-window on
	set-option -g mouse-select-pane on
	set-option -g mouse-resize-pane on

	#Alter colors
	set-option -g status-bg red     #Status background
	set-option -g status-fg white   #Status foreground
	set-window-option -g window-status-current-bg blue      #Highlight active window

	#Rebind keys 
	set-option -g prefix C-a        #Rebind prefix to CTRL-a
	bind-key C-r source-file ~/.tmux.conf   #Reload config with CTRL-b, r (hold CTRL)
	bind-key C-a last-window        #Double tap the prefix key to go to last window
	         
	#General options
	set-option -g history-limit 10000       #Change the scrollback hist to 10k lines
	set-option -g display-time 2000         #Display status messages for 2 seconds
	set-option -g base-index 1              #Set window base index to 1 instead of 0
	set-window-option -g pane-base-index 1  #Set pane base index to 1 instead of 0

</br>
##Use
	tmux		Launch tmux

**Mouse support:**</br>
Enabling mouse support in tmux (which is awesome) requires using a terminal app that enables passing mouse commands to programs running inside the terminal. The terminal.app doesn't do this by default (but [see here](http://stackoverflow.com/questions/24027009/how-do-i-scroll-in-tmux-using-the-mouse)). However, [iTerm2](https://www.iterm2.com/) does - so install that.

Note that tmux completely 'captures' the mouse while in use, which means that you can't do normal copy/paste operations to the system clipboard. However, in iTerm2, **holding the ALT key** temporarily prevents mouse commands from passing to tmux, allowing you to copy/paste/scroll as normal. </br></br>


The general way to use commands in tmux is to press the '**prefix**' key (CTRL-b by default), release, then press the command of choice. However, here's a super-pro hack:

**Repurpose the CAPSLOCK key to be \<prefix>:**</br>

1. In OS X, turn off the function of CAPSLOCK via:
	**System Preferences --> Keyboard --> Modifier Keys --> Caps Lock Key --> No Action**
2. Download [Seil](https://pqrs.org/osx/karabiner/seil.html.en).
3. Reset CAPSLOCK to 109, which corresponds to F10. 
4. Finally add the following to ~/.tmux.conf:

		set-option -g prefix F10
		bind-key F10 last-window 	#Double tap CAPLOCK to go to last window


Once in tmux, all tmux **external commands** can be listed with:

	tmux list-commands (lscm)		List tmu external commands/options

* Note that in all examples the long form of the command is followed by its alias in parentheses. Using either produces the same output.

Furthermore, a general help interface that lists all **internal commands** can be accessed with:

	<prefix> ?		List all tmux internal commands
	
	CTRL-s (/)		Search list
	q				Exit list


###Sessions
Each instance of tmux is called a **session**, which can include multiple windows (tabs) and panes. It's common to have a different tmux session for each project, for example. By default, sessions are named '0', but you can launch a tmux session with a name with:

	tmux new-session -s [name]		Launch a new tmux session
	tmux rename-session [name]		Rename session from within tmux

You can **detach** from a tmux session and return to the standard terminal prompt (i.e., leaving tmux running in the background) by pressing:

	<prefix> d						Exit tmux and leave session running in background

From the command prompt, you can list active tmux sessions with either of:

	tmux list-sessions (ls)			List names of active tmux sessions

You can then **attach** the tmux session, picking up where you left off, with:

	tmux attach -t [name]			Reload tmux session

From within tmux, you can switch between sessions with the following commands:

	<prefix> s		Select active sessions interactively
	<prefix> )		Switch to next session
	<prefix> (		Switch to previous session

While in the interactive session manager, you can press SPACEBAR or the right arrow key to view all active widows and select a specific window to jump to.

###Windows
Individual tmux sessions can use multiple **windows** (or tabs, as they're known in other terminal programs). The following commands control creating and switching to windows:

	<prefix> c			Open a new window (tab)
	<prefix> &			Kill window (prompts y/n confirmation)
	
	<prefix> w			Open the 'window chooser'
	<prefix> p/n		Switch to previous/next window
	<prefix> l			Switch to last window
	<prefix> [num]		Switch to window [num] (e.g., <prefix> 1)
	<prefix> f [TEXT]	Find window containing [TEXT]
	
Windows can also be renamed individually via:

	<prefix> ,			Open a prompt to rename window
	
Note that the above prompt can also be used to move a window between different sessions by specifying the session name followed by '**:**' (no spaces). A number after the colon will specify an index for the window.
	

###Panes

Another feature of tmux is the ability to divide a window into multiple **panes** (e.g., dividers that allow you to have multiple 'windows' running within a window). 

	<prefix> %				Split window vertically
	<prefix> "				Split window horizontally
	<prefix> [arrow keys]	Move to pane in direction indicated
	<prefix> x 				Kill pane (or type 'exit')
	<prefix> z				Toggle zoom pane to full window
	<prefix> q				Show index of all panes (briefly)

There are five preset pane layouts that can be cycled through as well:

	<prefix> SPACEBAR		Cycle through preset pane layouts
	
With mouse mode enabled (see above), you can move and resize panes by clicking a dragging. However, you can also press **\<prefix> q**, which shows a number on each pane that you can press to jump to the pane directly.

You can also convert a pane to its own window with **\<prefix> !**.

###Copy Mode

To **copy/paste**, you must enter copy mode, after which you can navigate with the arrow keys and begin selecting text by pressing SPACEBAR:

	<prefix> [		Enter copy mode
	
	Once in copy mode, use arrow keys to move:
	
	SPACERBAR			Begin selecting text
	ALT-w				Copy text to paste buffer
	
	<prefix> :, capture-pane 	Copies entire pane to paste buffer 
	
	<prefix> ]		Paste last copied item
	
Note that with mouse mode enabled, simply highlighting text copies it to the paste buffer.

Copying the most recent copied item from the paste buffer is accomplished with **\<prefix> ]**. However, all copied items can be selected and pasted interactively by entering the paste buffer with **\<prefix> =**. 				  	

	

	