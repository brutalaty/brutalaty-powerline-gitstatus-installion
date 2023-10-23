# Installing powerline

My personal guide to setting up powerline on bash.
For a more detailed, visit [System Slayer's Youtube guide][1]

## Prerequisites

You will need the python package manager pip:

`sudo apt install python3-pip`

You will also want to have a powerline font installed and set to you terminal emulator. You can download a font from [Nerd Fonts][3].

## Installation

install powerline-status:

`pip install --user powerline-status`

install powerline-gitstatus:

`pip install powerline-gitstatus`

### Find the installed directories

Open a text editor and store the following directories or files.

The powerline daemon directory:

`find / 2> /dev/null | grep /powerline-daemon`

The bash specific powerline.sh file:

`find / 2> /dev/null | grep bash/powerline.sh`

here is an example of what these might look like:

```
/usr/bin/

/usr/share/powerline/bindings/bash/powerline.sh
```

### Add powerline to your .bash_profile

Using the files you stored in the previous step update and append the following to your .bash_profile or relevant file:

```
#powerline
export PATH="$PATH:/usr/bin/"
export LC_ALL=en_US.Utf-8
powerline-daemon -q
POWERLINE_BASH_CONTINUATION=1
POWERLINE_BASH_SELECT=1
source /usr/share/powerline/bindings/bash/powerline.sh
```

At this point, powerline should be working, however powerline-gitstatus needs further steps.

### Setting up theme

Make the theme directories

```
mkdir -p ~/.config/powerline/colorschemes

mkdir -p ~/.config/powerline/themes/shell
```

We need to copy some default theme files. Using the powerline.sh filepath you stored earlier, remove the `/bindings/bash/powerline.sh` from the path and add `/config_files/`, here is an example of the filepath may look like: `/usr/share/powerline/config_files/`. Use this as the base for the next step.

Ok copy time:

```
cp /usr/share/powerline/config_files/colorschemes/default.json ~/.config/powerline/colorschemes/

cp /usr/share/powerline/config_files/themes/shell/default.json ~/.config/powerline/themes/shell/

cp /usr/share/powerline/config_files/themes/shell/default_leftonly.json ~/.config/powerline/themes/shell/
```

alternatively you can copy all of the contents of the config_files directory into the ~/.config/powerline directory.

### Setting up powerline-gitstatus

The following instructions can be found at the [jaspernbrouwer/powerline-gitstatus][2] repository, however they didn't work for me out of the box. Go ahead and follow the official instructions and return if they don't work.

Append the following to the `groups` object of the `.config/powerline/colorschemes/default.json` file:

```
"gitstatus":                 { "fg": "gray8",           "bg": "gray2", "attrs": [] },
"gitstatus_branch":          { "fg": "gray8",           "bg": "gray2", "attrs": [] },
"gitstatus_branch_clean":    { "fg": "green",           "bg": "gray2", "attrs": [] },
"gitstatus_branch_dirty":    { "fg": "gray8",           "bg": "gray2", "attrs": [] },
"gitstatus_branch_detached": { "fg": "mediumpurple",    "bg": "gray2", "attrs": [] },
"gitstatus_tag":             { "fg": "darkcyan",        "bg": "gray2", "attrs": [] },
"gitstatus_behind":          { "fg": "gray10",          "bg": "gray2", "attrs": [] },
"gitstatus_ahead":           { "fg": "gray10",          "bg": "gray2", "attrs": [] },
"gitstatus_staged":          { "fg": "green",           "bg": "gray2", "attrs": [] },
"gitstatus_unmerged":        { "fg": "brightred",       "bg": "gray2", "attrs": [] },
"gitstatus_changed":         { "fg": "mediumorange",    "bg": "gray2", "attrs": [] },
"gitstatus_untracked":       { "fg": "brightestorange", "bg": "gray2", "attrs": [] },
"gitstatus_stashed":         { "fg": "darkblue",        "bg": "gray2", "attrs": [] },
"gitstatus:divider":         { "fg": "gray8",           "bg": "gray2", "attrs": [] }
```

Append the following object to the `left` array in both the

`.config/powerline/themes/shell/default.json` and

`.config/powerline/themes/shell/default_leftonly.json`
files

```
{
    "function": "powerline_gitstatus.gitstatus",
    "priority": 40
}
```

Remove the following from both the default.json and default_leftonly.json: note that the priority may differ:

```
{
    "function": "powerline.segments.common.vcs.branch",
    "priority": 40
},
```

Next lets reset the powerline-daemon process: Resetting the powerline-daemon will let any configuration changes take place.

`powerline-daemon --replace`

[1]: https://www.youtube.com/watch?v=zfm2E4E7Dok
[2]: https://github.com/jaspernbrouwer/powerline-gitstatus#configuration
[3]: https://www.nerdfonts.com/
