# Overview #

Regions is the back end implementation that takes care of the 'low-level'
implementation of the regions package; stuff like creation and 
maintenance of region data.  Regions also implements the actual process
of protecting the region from unauthorized player activity.

Regions Manager is the front-end command interface for the Regions
protection plugin and can be used to create, administer, and maintain
regions.  It also does some basic world edit functions.  It is not very 
user friendly and is intended for operation by an admin/staff member.
Regions Manager can itself be used as a back end for a higher level 
player-friendly land claim system

# Set up #

## permissions ## 

After installing the regions plugin files, you need to set up
some permissions so that you (or designated people) can use the 
command interface.  The permissions below can be manually given using wrapper's 
`/permission` command (e.g., `/perms users <username> set region.player True`),
but let's do it the easier way:

Use the GroupsManager plugin:<br>
set your permission to use it (level 10 Superops [./superops.txt] don't
 need to do this):<br>
From the console, type:<br>
`/perms users <username> set groupsmanager.auth True`


In the `/wrapper-data/plugins/groupsmanager` directory, create the 
group manager text files.  First, create the groups definition file:<br><br>
_"groups.txt"_
```
owner
admin
trusted
```


Authorized players (let's call them group 'trusted') get these permissions
by creating a trusted.txt file:<br><br>
_"trusted.txt"_
```
region.player
# region.home  (optional, if you want to give players this command)
```

Admin Staff (group 'admin") need these permissions to administer 
regions(claims) for others:<br><br>
_"admin.txt"_
```
region.wand
region.delete
region.define
region.protect
region.adjust
region.setowner

# own more than one region (staff need this to create additioinal regions
#  for other players while stiff having a claim of their own).
region.multiple

# inherit trusted
trusted
```

Optionally, you can set a higher "owner" level to give more (dangerous) 
 commands:<br><br>
_"owner.txt"_
```
# You can do a lot of damage if you misuse these three!
region.copy
region.replace
region.fill

region.dumps

# inherit admin (which will inherit trusted)
admin
```

From in-game, run the following command to load the files into the wrapper's 
permission system:<br><br>
```
/loadgr
```

Easy!, now just give yourself the appropriate group permission:<br><br>
admin/staff - `/perms user <username> group admin`<br>
player - `/perms user <player> group trusted`<br>

# Using Regions #

## create a region ##

 1) get the wand:  `//wand`.
 2) Using the wand, right click and left click on two opposing blocks
  (_note: you can also manually input the positions by using the
  `//pos1 <x> <y> <z>` and `//pos2 <x> <y> <z>` commands_).
 3) Once the corners are selected, decide on a name for the region and type (I'll use
  'myproperty' here):<br> `//rg define myproperty`<br>
 4) now select the region for further edits:<br>`//rg use myproperty`<br>
 5) You must now define the height and depth of the region (I'll use 5 and 256):<br>
  `//rg floor 5`<br>
  `//rg roof 256`<br>
 6) The region is now created!  The next step is to activate protection:<br>
  `//rg protect on`

  __NOTE__ _Region names must be unique.  For instance, only one person_
   _could actually call their region 'myproperty'..._

## Editing region properties ##

The command format is:<br>
`//rg set [region_name] break|access|place|ban|unban|remove <playername>`<br><br>

Regions have these attributes and lists associated with them:
- Owner
- Break players
- Place players
- Access players
- Banned players
- The region definition (the two corners that define it, dimension, and
 whether it is protected).

### Abilities / region properties ###
- The Owner can administer any of the player lists, even if he only has
     region.player permission.
- Break players can break/mine things inside the region.
- Place players can place blocks, operate switches, open chests, and use items inside the region.
- Access players can perform certain actions in the region.  For instance:
    - Use buckets to place water/lava.
    - Use items like bows or eat
- Banned players cannot enter your region.  Once they step into your
    region, they are teleported some distance back along their original path.  If
    they persist, they will be stuck at your border (although they can still
    walk away).
<br>

### Changing the owner ##

When a staff member creates a region for another player, the next step is to
transfer ownership of the region to the player:<br>
`//rg set owner <playername>`<br>

### Adding a player to your region ###

To add a player to you region, you give him each of the permissions you want
 him to have in your region:<br><br>

Set `//rg use myproperty` so that you don't need to use the region_name:<br>
`//rg use myproperty`<br><br>
Set the player for full permission in your region:<br>

```
//rg set break <playername>
//rg set place <playername>
//rg set access <playername>
```
<br>


### Removing and banning/unbanning a player ###

To remove a player from all access lists:
<br>
(using the region)`//rg set remove <playername>`<br><br>

To ban a player:<br>
`//rg set ban <playername>`<br>

To ban a player:<br>
`//rg set unban <playername>`<br>

### Resizing a region ###

To resize a region,  make new 'pos1' and 'pos2' selections.  Then type:<br>
`//rg resize myproperty` (or whatever region name you use).
<br>


### Draw the region boundaries on your client screen ###

Using the region (`//rg use [...]`, run the draw sub-command:<br>
`//rg draw`<br>

Depending on the roof and floor settings, you may not see the entire thing.<br>


### Delete a region ###

You must type the region name for this command:<br>
`//rg delete myproperty`
<br>

### Show a region's metadata ###

To see all the data assocated with a region, run "show":<br>
`//rg show <region>`  (you must type in the region name).

### Locate a region or list regions ###

The "find" command can be used to locate or list regions.<br>

You can locate a region based on:
 - Owner - The owner of the region
 - name - The name or part of the name of the region)
 - region - List all regions in the present "minecraft region" (16x16 chunks)
 - near - List all regions near by (with-in a 50 block square radius by default).
 - here - Where you are standing.

"here" and "region" are based on where you are standing and require no arguments:<br>
```
//rg find here
//rg find region
```

"Near" can also be used with no argument, if you wish to use the default square radius.
<br>
```
//rg find owner <playername>
//rg find name <matching name text>
//rg find near [number besides 50]
```
<br>

### Goto a region ###

The goto subcommand allows you to goto a region:<br>
`//rg goto myproperty`
<br>

This will place you "somewhere" in your claim (by a random 'spreadplayers' command).

Those with the 'regions.home' permission can use the `//home` command, which will
send the player to the first region it finds in this order:<br>
1) The first region it finds where they are the owner.
2) The first region it finds where they are on the "AccessPlayers" list.

//home places the user near the POS1 of the region using a tight 2 block
 spreadplayers radius.


### Debugging, examining, or modifying the regions data files ###

The regions data Storage files are `regions.pkl` and `files.pkl` and are in their
plugin folder 'com.suresttexas00.regions'.  The pickling is usually "human readable",
but they are not easy to read or edit.  To get 'pretty' json copies of these files, run:<br>
`//jsondumps`
<br>

The files `files.json` and `regions.json` will be written into the main wrapper
folder.

if you delete the pickle files (when wrapper is off) and substitute these json
files back into the '/wrapper-data/plugins/com.suresttexas00.regions' folder,
wrapper will load these files and convert them to pickle format again.  Therefore,
it is possible to edit the regions data manually.  Be warned that a mistake could
corrupt your entire regions protection system!


