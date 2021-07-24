# norns-example-mod

this repo demonstrates how to create a "mod" which can augment/patch/hack matron
(the lua scripting engine for norns).

## why

a mod lives in the `dust` folder just like normal norns scripts. unlike norns
scripts, the code in a mod is loaded by matron when it starts up. mods can
modify or extend the lua environment globally such that the changes are visible
to all scripts.

## how

mods work by registering functions to be called by matron hooks. matron defines
four hooks:

* `system_post_startup` - called after matron has fully started and system state
  has been restored but before any script is run
* `system_pre_shutdown` - called when `SYSTEM > SLEEP` is selected from the menu
* `script_pre_init` - called after a script has been loaded but before its
  engine has started, pmap settings restored, and `init()` function called
* `script_post_cleanup` - called after a script's `cleanup()` function has been
  called, this normally occurs when switching between scripts

multiple mods can each register their own callback function for a given hook.
matron will call each registered function but the order in which the functions
are called is arbitrary. any errors generated within a registered function will
be caught and ignored.

mods may additionally provide their own menu page accessible via the `SYSTEM >
MODS` menu.

## layout

a mod is any norns script/project which contains a file called `lib/mod.lua`.
this layout allows a mod to be either a standalone project or simply
functionality bundled along side an existing norns script. a mod is free to
`require` additional modules as needed.

## using

mods by their nature can have a negative impact on system stability or make
system level changes which may not be universally welcome. by default a mod;
even if installed, will not be loaded by matron. one must explicitly enable a
mod and restart matron before its functionality will be available.

to enable a mod go to `SYSTEM > MODS` to see the list of installed mods. mods
which are loaded will have a small dot to the left of their name.
use `E2` to selected an item in the list and then use `E3` to enable or disable
as appropriate. _unloaded_ mods will show a `+` to the right their name to
indicate that they will be enabled (and thus loaded) on restart. _loaded_ mods will
show a `-` to the right of their name indicating they will be disabled on
restart.

loaded mods which have a menu will have ` >` at the end of their name, pressing
`K3` will enter the mod's menu.

## developing

see the `lib/mod.lua` file in this repo for more details.


