

# tmuxomatic [![](http://img.shields.io/pypi/v/tmuxomatic.svg?style=flat)](https://pypi.python.org/pypi/tmuxomatic) [![](http://img.shields.io/pypi/dm/tmuxomatic.svg?style=flat)](https://pypi.python.org/pypi/tmuxomatic)

The other tmux session managers are doing it wrong.  From unnecessary options requiring pages of documentation, to windows defined by a complicated nesting of pane splits.  Instead, session management should be more flexible and more powerful, yet so easy that anybody could use it after just one example.

At the heart of tmuxomatic is the **windowgram**, a better way of defining tmux windows.  The windowgram is a rectangle comprised of alphanumeric characters (0-9, a-z, A-Z).  Each character identifies the position, size, and shape of a pane.  It should take only one short example to demonstrate the flexibility and power of the windowgram.

Compare this window from `session_example`, with its screenshot from `tmuxomatic session_example`:

	window example_one        # A new window begins like this, spaces in names are allowed

	AAAAAAvvvvvXXXXXTTTT      # The windowgram, it defines the shapes and positions of panes
	jjjQQQQQQQuuuuuuTTTT      # Make your own, of any size and arrangement, 62 panes maximum
	jjjQQQQQQQuuuuuuTTTT
	jjjQQQQQQQuuuuuuTTTT
	0000llllllllllaaaaaa
	1234llllllllllaaaaaa

	  foc                     # Three 3-letter commands to remember: Focus, Directory, Run
	  dir ~                   # Unlinked directory, becomes default for all undefined panes
	A run figlet "A"          # Linked to pane A, this command prints a large "A" in pane A
	Q run figlet "Q"
	A foc

![](https://github.com/oxidane/tmuxomatic/blob/master/img/example.png)

With tmuxomatic, you'll never have to manually split, position, or size a pane again.  And linking the panes to actions is so simple and logical that you probably won't forget it.  There are no extra file format rules to remember, and typically no command line arguments will be necessary.

For additional features, run `tmuxomatic --help`.  For example, there's a feature to scale your windowgram larger or smaller -- by any multiplier, on either axis -- to help with fine-tuning.  This is useful if you need very small panes on a window.



## Flex your windowgrams

**Flex is in development, and this tutorial will be expanded as new commands are added**

Windowgrams are a neat way of arranging workspaces.  For general layouts, a windowgram is typed up quickly.  But sometimes you need a windowgram fit to specification, and to deliver, you may find yourself doing a lot of ASCII art.  Not nearly as frustrating as manual pane splits, but somewhat inefficient.

Fortunately there's a better way: Flex.  Introduced in tmuxomatic 1.1.0, flex aims to bring the windowgram to an even greater potential.  I'll walk you through the construction of a windowgram that would otherwise require a lot of typing.  Begin by opening the flex console on a test session file.  `tmuxomatic session_flexample --flex`

Inside this tutorial, the flex console is represented by `flex>`.  Like tmuxomatic, flex is simple but powerful.  For a list of commands type `help`.  Let's make a new window named, `administration`.

	flex> new administration

	1

A single pane window represented by a single character.  Let's scale this to be more readable.  Scale uses multipliers, percentages, or exact characters.  Let's go for 25 characters wide, 10 characters tall.

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

**TODO: add, break, join, split, drag**



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

