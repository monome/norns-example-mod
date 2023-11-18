# norns-example-mod

This repo demonstrates how to create a "mod" which can augment/patch/hack matron
(the Lua scripting engine for norns).

## why

A mod lives in the `dust` folder just like normal norns scripts. Unlike norns
scripts, the code in a mod is loaded by matron when it starts up. mods can
modify or extend the Lua environment globally such that the changes are visible
to all scripts.

## how

mods work by registering functions to be called by matron hooks. matron defines
five hooks:

* `system_post_startup` - called after matron has fully started and system state
  has been restored but before any script is run
* `system_pre_shutdown` - called when `SYSTEM > SLEEP` is selected from the menu
* `script_pre_init` - called after a script has been loaded but before its
  engine has started, pmap settings restored, and `init()` function called
* `script_post_init` - called after a script's `init()` function has been called
* `script_post_cleanup` - called after a script's `cleanup()` function has been
  called, this normally occurs when switching between scripts

Multiple mods can each register their own callback function for a given hook.
matron will sort the hooks alphabetically before executing them. This allows mods that add parameters to add them in a consistent order -- eg. mods that need all parameters available before executing should start with `~` to ensure they're at the end.

Any errors generated within a registered function will be caught and ignored
in the maiden REPL, but see [the debugging documentation](https://monome.org/docs/norns/help/#deeper-debugging)
for how you can access them.

mods may additionally provide their own menu page accessible via the `SYSTEM >
MODS` menu.

## layout

A mod is any norns script/project which contains a file called `lib/mod.lua`.
This layout allows a mod to be either a standalone project or simply
functionality bundled along side an existing norns script. A mod is free to
`require` additional modules as needed.

## using

mods by their nature can have a negative impact on system stability or make
system level changes which may not be universally welcome. By default a mod,
even if installed, will not be loaded by matron. One must explicitly enable a
mod and restart matron before its functionality will be available.

To enable a mod go to `SYSTEM > MODS` to see the list of installed mods. mods
which are loaded will have a small dot to the left of their name.
use `E2` to selected an item in the list and then use `E3` to enable or disable
as appropriate. _Unloaded_ mods will show a `+` to the right their name to
indicate that they will be enabled (and thus loaded) on restart. _Loaded_ mods will
show a `-` to the right of their name indicating they will be disabled on
restart.

Loaded mods which have a menu will have ` >` at the end of their name, pressing
`K3` will enter the mod's menu.

## developing

See the `lib/mod.lua` file in this repo for more details.