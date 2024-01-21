Sway Workspace Renamer
======


This is a fork of `swaywsr`, a small program that uses Sway [IPC Interface](https://github.com/swaywm/sway/blob/master/sway/sway-ipc.7.scd) to change name of a workspace based on its content. Workspace name is composed from `app_id` (Wayland) or `WM_CLASS` (X11) window property.


## Installation

Download latest pre-compiled release or build yourself:

```sh
cargo build --release
```


## Usage

Launch the program and let it listen for events in the bacground, e.g in Sway config:

```sh
exec_always swaywsr --remove-duplicates
```


### Options

The `toml` file has four fields:

1. `icons` to assign icons to classes.
2. `aliases` to assign alternative names to be displayed.
3. `general` to assign the separator, default icon as well as the end character for protected workspaces.
4. `options` to assign additional flags like `--no-names` or `--verbose`

A very basic preset for font-awesome is configured, to enable it use the option `--icons awesome` (requires `font-awesome` to be installed).

Example config can be found in `assets/example_config.toml`

```toml
[icons]
Firefox = ""

[aliases]
"org.gnome.Nautilus" = "Files"

[general]
separator = " | "
ignore_ending = "."
```

Run `swaywsr --help` an overview of available flags:

```shell
$ swaywsr -h
Changes Sway workspace names based on their content

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

`swaywsr` depends on numbered workspaces, your Sway configuration has to reflect this:

```
bindsym $mod+1 workspace number 1
```


## Contributors

* [Pedro Scaff (pedroscaff)](https://github.com/pedroscaff)
* [Andrew Savchenko (savchenko)](https://github.com/savchenko)


## Attribution

Thanks [Daniel Berg (roosta)](https://github.com/roosta) for the original [i3wsr](https://github.com/roosta/i3wsr) implementation. This program would not be possible without
[swayipc-rs](https://github.com/JayceFayne/swayipc-rs), a rust library for controlling sway-wm through its IPC interface.
