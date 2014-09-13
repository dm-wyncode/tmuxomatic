

# tmuxomatic [![](http://img.shields.io/pypi/v/tmuxomatic.svg?style=flat)](https://pypi.python.org/pypi/tmuxomatic) [![](http://img.shields.io/pypi/dm/tmuxomatic.svg?style=flat)](https://pypi.python.org/pypi/tmuxomatic)

The other tmux session managers are doing it wrong.  From unnecessary options requiring pages of documentation, to windows defined by a complicated nesting of pane splits.  Instead, session management should be more flexible and more powerful, yet so easy that anybody could use it after just one example.

At the heart of tmuxomatic is the **windowgram**, a better way of defining tmux windows.  The windowgram is a rectangle comprised of alphanumeric characters (0-9, a-z, A-Z).  Each character grouping identifies the name, position, size, and shape of a pane.  It should take only one short example to demonstrate the flexibility and power of the windowgram.

Compare this window from `session_example`, with its screenshot from `tmuxomatic session_example`:

	window example_one        # A new window begins like this, spaces in names allowed

	AAAAAAvvvvvXXXXXTTTT      # The windowgram, defines the shapes and positions of panes
	jjjQQQQQQQuuuuuuTTTT      # Make your own, any size and arrangement, 62 panes maximum
	jjjQQQQQQQuuuuuuTTTT
	jjjQQQQQQQuuuuuuTTTT
	0000llllllllllaaaaaa
	1234llllllllllaaaaaa

	  foc                     # Only three 3-letter commands: Focus, Directory, Run
	  dir ~                   # Unlinked directory is the default for all undefined panes
	A run figlet "A"          # Linked command to pane A, in this case prints a large "A"
	Q run figlet "Q"
	A foc

![](https://github.com/oxidane/tmuxomatic/blob/master/img/example.png)

With tmuxomatic, you'll never have to manually split, position, or size a pane again.  And linking the panes to actions is so simple and logical that you probably won't forget it.  There are no extra file format rules to remember, and typically no command line arguments will be necessary.

For additional features, see `tmuxomatic --help`.



## Flex your windowgrams

Windowgrams are a neat way of arranging workspaces.  For simpler layouts, a windowgram is typed up quickly.  But if you need one with more detail, you may find yourself doing a lot of ASCII art.

Introduced in tmuxomatic 2.0, flex aims to automate the windowgram itself.

Flex is an object-oriented windowgram editor that is visually expressive, naturally worded, logically ordered, minimal, and powerful.  The simple command set can be combined to make any conceivable windowgram -- likely more quickly and more easily than crafting by hand.  Flex is intended for power users who want detailed windowgrams without the tedium of manual entry.

Key concepts used by the flex command set:

* **Pane**: Single character pane identifier representing one pane in a windowgram
* **Group**: String of one or more panes (usually in the form of a rectangle without gaps)
* **Edge**: String of panes that together border one or more edges (the imaginary line between panes)
* **Size**: Sizes are expressed in exact characters, or contextually as percentages or multipliers
* **Direction**: Cardinal directions (up, down, left, right), also used for windowgram and group edges

Flex commands use these and other basic concepts to expand, contract, or otherwise modify a windowgram.

### Flex: Windowgram Creation Demo

Let's use flex to build a windowgram that previously required a lot of typing.  Begin by opening the flex console on the session file `session_flexample`.  Note that flex will create the file for you if it does not already exist.

	% tmuxomatic session_flexample --flex

	flex>

For brevity, the flex console is represented by a `flex>` prompt.  Like tmuxomatic, flex is simple but powerful.  For a full list of commands, type `help` at any time.

We'll be creating a layout for cryptocurrency nodes, specifically Bitcoin, Litecoin, and Namecoin.  We'll have panes for an installation shell (`1`), a disk monitor (`z`).  And for each currency, a title and crash loop on top (`BLN`), and currency statistics on bottom (`bln`).

Begin by creating a new window named `wallets`.

	flex> new wallets

	1

To make it easier to follow, let's `scale` this windowgram to `25` characters wide, by `10` characters tall.  Many flex parameters are flexible; instead of characters we could have used multipliers or percentages.

	flex> scale 25x10

	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111
	1111111111111111111111111

Now let's `add` a new pane on the `right` edge, and make it `50%` of the size of the original windowgram, or `12` characters, whichever you prefer.

	flex> add right 50%

	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000
	1111111111111111111111111000000000000

What I actually have in mind for pane `0` looks like this:

	zzz
	BLN
	BLN
	bln
	bln

There are two ways to do this; one uses `split` and `break`, the other uses `break` and `join`.  We'll use the latter.

First we `break` the new pane `0` into a grid, `3` panes wide by `5` panes high -- this reflects the envisioned shape above.  For readability, we'll make use of the optional parameter so that new panes to start at `A`.

	flex> break 0 3x5 A

	1111111111111111111111111AAAABBBBCCCC
	1111111111111111111111111AAAABBBBCCCC
	1111111111111111111111111DDDDEEEEFFFF
	1111111111111111111111111DDDDEEEEFFFF
	1111111111111111111111111GGGGHHHHIIII
	1111111111111111111111111GGGGHHHHIIII
	1111111111111111111111111JJJJKKKKLLLL
	1111111111111111111111111JJJJKKKKLLLL
	1111111111111111111111111MMMMNNNNOOOO
	1111111111111111111111111MMMMNNNNOOOO

Each `join` parameter is a group of panes to be joined together.  By default, the joined pane is named after the first pane in the group.  But we'll be using the optional rename, by appending `.` followed by the new pane id.  We may now complete the envisioned layout using just one join command.

	flex> join ABC.z DG.B EH.L FI.N JM.b KN.l LO.n

	1111111111111111111111111zzzzzzzzzzzz
	1111111111111111111111111zzzzzzzzzzzz
	1111111111111111111111111BBBBLLLLNNNN
	1111111111111111111111111BBBBLLLLNNNN
	1111111111111111111111111BBBBLLLLNNNN
	1111111111111111111111111BBBBLLLLNNNN
	1111111111111111111111111bbbbllllnnnn
	1111111111111111111111111bbbbllllnnnn
	1111111111111111111111111bbbbllllnnnn
	1111111111111111111111111bbbbllllnnnn

**NOTE: Flex is in development, and this tutorial is a work in progress**

**TODO: rename, drag**

### Flex: Windowgram Modification Demo

**TODO: swap, clone, delete, mirror, flip**

### Flex: Other Commands

**TODO: Outline other commands not covered here: split, rotate, insert**

**TODO: Or perhaps add these somewhere in the above, so the demos cover all commands**



## Installation

This application requires:

* [Python 3](http://www.python.org/getit/) +
* [tmux 1.8](http://tmux.sourceforge.net/) +

There are three ways to install tmuxomatic, in order of convenience:

  * **Automatically** (pip)

    * `pip-python3 install tmuxomatic`

  * **Manually** (python)

    * Download and extract the archive file from https://pypi.python.org/pypi/tmuxomatic
    * In the tmuxomatic directory, run `python3 setup.py install`

  * **From Development** (git)

    * Visit https://github.com/oxidane/tmuxomatic for up-to-date installation instructions
    * `git clone git://github.com/oxidane/tmuxomatic.git`
    * `cp -a tmuxomatic/tmuxomatic /usr/bin`

Verify that you have the current version installed: [![](http://img.shields.io/pypi/v/tmuxomatic.svg?style=flat)](https://pypi.python.org/pypi/tmuxomatic)

* `tmuxomatic -V`

On some systems pip will not upgrade properly, this might be fixable by clearing the pip cache prior to upgrade:

* `rm -rf /tmp/pip-build-root/tmuxomatic`
* `pip-python3 install --upgrade tmuxomatic`

Optional packages:

* `pip-python3 install pyyaml` ... For YAML session file support



## Notes

To use tmuxomatic, you don't have to know everything about [how to use tmux](http://net.tutsplus.com/tutorials/tools-and-tips/intro-to-tmux/), but the knowledge is useful for [customizing the tmux status bar](http://me.veekun.com/blog/2012/03/21/tmux-is-sweet-as-heck/), or [changing the default key bindings](https://wiki.archlinux.org/index.php/tmux#Key_bindings).  These are tmux user preferences, and typically placed in a personal `.tmux.conf` file.



## Copyright and License

Copyright 2013-2014, Oxidane.  All rights reserved.

Distributed under the [BSD 3-Clause License](http://opensource.org/licenses/BSD-3-Clause).  The copyright and license must be included with any use, modification, or redistribution of the source.  See the license for details.

