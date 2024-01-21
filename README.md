Sway Workspace Renamer
======

This is a fork of `swaywsr`, a small program that uses [Sways's](https://swaywm.org/) [IPC Interface](https://github.com/swaywm/sway/blob/master/sway/sway-ipc.7.scd) to change the name of a workspace based on its contents.

It is a port of [i3wsr](https://github.com/roosta/i3wsr).

## Details

The chosen name for a workspace is a composite of the `app_id` (wayland native) or `WM_CLASS` X11 window property for each window in a workspace.

## Installation
Build a release binary,

```sh
cargo build --release
```

## Usage
Just launch the program and it'll listen for events if you are running sway.
Another option is to put something like this in your sway config

```
exec_always $PATH_TO_RELEASE_BINARY
```

### Options


You must provide a config file that is passed using the `--config path_to_file.toml` option. The `toml` file has four fields:
- `icons` to assign icons to classes
- `aliases` to assign alternative names to be displayed 
- `general` to assign the separator, default icon as well as the end character for protected workspaces.
- `options` to assign additional flags like `--no-names` or `--verbose`

You can configure icons for the respective classes, a very basic preset for font-awesome is configured, to enable it use the option `--icons awesome` (requires font-awesome to be installed).

If you have icons and don't want the names to be displayed, you can use the `--no-names` flag.

Example config can be found in `assets/example_config.toml`

```toml
[icons]
# font awesome
TelegramDesktop = ""
Firefox = ""
Alacritty = ""
Thunderbird = ""
# smile emoji
MyNiceProgram = "😛"

[aliases]
TelegramDesktop = "Telegram"
"Org.gnome.Nautilus" = "Nautilus"

[general]
seperator = ""
ignore_ending = "."
```

For an overview of available options

```shell
$ swaywsr -h
Change i3-wm workspace names based on its contents

Usage: swaywsr [OPTIONS]

Options:
  -i, --icons <ICONS>      Set to no to display only icons (if available) [possible values: awesome]
  -n, --no-names           Set to no to display only icons (if available)
  -v, --verbose            Output debug information
  -c, --config <CONFIG>    Path to toml config file
  -r, --remove-duplicates  Remove duplicate entries in workspace
  -h, --help               Print help
  -V, --version            Print version
```

## Configuration

This program depends on numbered workspaces, since we're constantly changing the
workspace name. So your sway configuration need to reflect this:

```
bindsym $mod+1 workspace number 1
```

If you don't necessarily bind your workspaces to only numbers, or
you want to keep a part of the name constant you can do like this:

```
bindsym $mod+q workspace number 1:[Q]
```

This way the workspace would look something like this when it gets changed:

```
1:[Q] Emacs|Firefox
```
You can take this a bit further by using a bar that trims the workspace number and be left with only
```
[Q] Emacs|Firefox
```

## Contributors
* [Pedro Scaff (pedroscaff)](https://github.com/pedroscaff)
* [Andrew Savchenko (savchenko)](https://github.com/savchenko)

## Attribution
Thanks [Daniel Berg (roosta)](https://github.com/roosta) for the original [i3wsr](https://github.com/roosta/i3wsr) implementation. This program would not be possible without
[swayipc-rs](https://github.com/JayceFayne/swayipc-rs), a rust library for controlling sway-wm through its IPC interface.
