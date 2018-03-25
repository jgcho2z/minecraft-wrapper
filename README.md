# Overview #
-------------------------------------------
Wrapper.py is an easy to use Minecraft server wrapper for adding extra functionality into the server without modifying 
the server jar file.  It also comes with a relatively simple and straight-forward - yet powerful - plugin API that can be used
to create Bukkit-like plugins with no server modding.  The API works best when operated in proxy mode.

We also have a gitter channel: [![Join the chat at https://gitter.im/benbaptist/minecraft-wrapper](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/benbaptist/minecraft-wrapper?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

###  **Wrapper.py Versions**

 - [Master branch "stable"](https://github.com/benbaptist/minecraft-wrapper/tree/master):  Stable branch that is only updated with serious bug fixes and major releases. <sup id="a1">[1](#f1)</sup>
 - [Development branch "dev"](https://github.com/benbaptist/minecraft-wrapper/tree/development):  Development branch with newer features.

NOTICE: Wrapper will Accept the minecraft server EULA on your behalf. <sup id="a2">[2](#f2)</sup>


# Features #
-------------------------------------------
Wrapper.py supports the following features:
- [Plugin system](/documentation/plugin_api.md) for adding extra features to a vanilla server.
- Permissions system with group support.
- Proxy mode operation allows you to add extra bukkit-like functionality to plugins:
  - Real `/` command interface.
  - Built in Hub world / Multi-server support!
    - Use the built-in /hub functionality with world configurations set up in the wrapper config, __or__
    - Implement you own customized version with the plugin API by calling
  - Limit entity breeding / spawning.
  - Monitor, Modify, and change:
    - player chat.
    - player block /digging/placement.
    - player inventory.
- Automatic Backups
  - Automatically delete the oldest backups once you reach a specified number of backups
  - Specify which folders and files get backed up
- IRC bridge
  - Controlling server from IRC
  - Achievements, deaths, and whatnot appear on IRC
  - Chat between Minecraft server and IRC channels
- Scheduled reboots
- Web remote for controlling the server and the wrapper through your web browser
- Shell scripts that are called upon certain events (similar to plugin events, but quicker and easier)
- Minecraft 1.7 and later support
- Colorized console logging.


# Installation #

###  **Python Versions**

Python 3.5 + is suggested,
***[However...](/documentation/pyversions.md)***

###  **Dependencies**

Wrapper.py requires the following packages: </br>
- Python packages: `pip, requests, cryptography, bcrypt, setuptools, pkg_resources`
- Tar is required for backups.
 - ***[More...](/documentation/depends.md)***


#### [**LINUX download and setup**](/documentation/linux.md)

#### [**Windows Download and setup**](/documentation/linux.md)

###  **Start Up**

You only need to download Wrapper.py.  The 'wrapper' folder is the source code and is just the extracted version
 of Wrapper.py.  Wrapper.py is a Python-executable archive folder containing the sourcecode.</br>

- Run `python Wrapper.py [--passphrase 'passphrase']` to start (passphrase must be 8 or more characters in length).


    _An alternative method of running wrapper is to run the source package directly.  To do this, clone the repo, copy the
    folder 'wrapper' to the desired location and run it thusly:_<br>
    `python /path/to/wrapperfolder/wrapper`


    Wrapper also takes the following optional arguments:

    ```
      -h, --help           show this help message and exit
      --encoding, -e       Specify an encoding (other than utf-8)
      --betterconsole, -b  Use "better console" feature to anchor your imput at
                           the bottom of the console (anti- scroll-away feature)
                           Also implements jline-like functionality with arrow
                           keys and bash-like command history memory.
      --passphrase, -p     Passphrase used to encrypt all passwords in Wrapper.
                           Please use as fairly long phrase (minimum is 8
                           characters). If not specified, or incorrectly supplied,
                           Wrapper will prompt for a new passphrase before
                           starting!
    ```

    To start wrapper using your passphrase:</br>
    `python Wrapper.py --passphrase "my special passphrase - keep this a secret!"`

    If a passphrase is not supplied, Wrapper will prompt for one:
    ```
    please input a master passphrase for Wrapper.  This passphrase will be used to encrypt
     all passwords in Wrapper.
    >
    ```


 - When you first run Wrapper, you will see the following output as it creates the logging file, a wrapper.properties.json
 file, and then exits.:
    ```
    [15:19:18] [root/WARNING]: Unable to locate logging.json -- Creating default logging configuration
    please input a master passphrase for Wrapper.  This passphrase will be used to encrypt all passwords in Wrapper.  Please use a fairly long phrase (minimum is 8 characters).  You can change the pass-phrase later with /passphrase <new phrase>
    >
    [15:19:30] [Config/WARNING]: Updated wrapper.properties.json file - check and edit configuration if needed and start again.

    ```

- Open the wrapper properties file, set the `["General"]["command"]` item to boot the jar file and java start
 options of your choice.

- Wrapper supports having a separate server and wrapper folder.  This is also recommended, although you can simply put
 Wrapper in the same directory with your server.  Examples (item `["General"]["server-directory"]` in the config file):

     - setting `"../server"` - will set the server folder to a sister directory.
     - or you can use an absolute path: `"/home/user/minecraft/server"`.
     - use the default `"."` to run wrapper inside your server folder.

- Tune the file to your remaining preferences, and then run wrapper again.

- If the server is new (only a server.jar file in the server directory) You will see output similar
 to this:
    ```
    [15:24:10] [root/INFO]: Logging configuration file (logging.json) located and loaded, logging configuration set!
    please input a master passphrase for Wrapper.  This passphrase will be used to encrypt all passwords in Wrapper.  Please use a fairly long phrase (minimum is 8 characters).  You can change the pass-phrase later with /passphrase <new phrase>
    >
    [15:24:16] [Wrapper.py/INFO]: Wrapper.py started - Version [0, 14, 1] (development build #245)
    [15:24:16] [Wrapper.py/WARNING]: NOTE: Server was in 'STOP' state last time  Wrapper.py was running. To start the server, run /start.
    [15:24:16] [Wrapper.py/WARNING]: File 'server.properties' not found.
    [15:24:16] [Wrapper.py/INFO]: Loading plugins...

    ```

    To continue, you will need to enter `/start` to continue running (if you are using proxy mode, be aware that this must be done within 2 minutes or proxy mode will be disabled).

    The server will start and accept the Eula for you:
    ```
    /start
    [15:28:02] [Wrapper.py/INFO]: Starting server...
    [15:28:02] [Wrapper.py/WARNING]: File 'server.properties' not found.
    [15:28:05] [Server thread/INFO]: Starting minecraft server version 1.12.2
    [15:28:05] [Server thread/INFO]: Loading properties
    [15:28:05] [Server thread/WARN]: server.properties does not exist
    [15:28:05] [Server thread/INFO]: Generating new properties file
    [15:28:05] [Server thread/WARN]: Failed to load eula.txt
    [15:28:05] [Server thread/INFO]: You need to agree to the EULA in order to run the server. Go to eula.txt for more info.
    [15:28:05] [Server thread/INFO]: Stopping server
    [15:28:05] [Server Shutdown Thread/INFO]: Stopping server
    [15:28:06] [Wrapper.py/INFO]: Starting server...
    [15:28:06] [Wrapper.py/WARNING]: File 'server.properties' not found.
    [15:28:06] [Wrapper.py/WARNING]: EULA agreement was not accepted, accepting on your behalf...
    [15:28:08] [Server thread/INFO]: Starting minecraft server version 1.12.2
    [15:28:08] [Server thread/INFO]: Loading properties
    [15:28:08] [Server thread/INFO]: Default game type: SURVIVAL
    [15:28:08] [Server thread/INFO]: Generating keypair
    [15:28:09] [Server thread/INFO]: Starting Minecraft server on *:25565
     ...
    [15:28:16] [Server thread/INFO]: Preparing spawn area: 94%
    [15:28:17] [Server thread/INFO]: Done (7.956s)! For help, type "help" or "?"
    [15:28:17] [Wrapper.py/INFO]: Server started
    [15:28:17] [Wrapper.py/INFO]: Proxy listening on *:25566
    ```

### Operating wrapper ###

- Any console command beginning with a slash (/) will be interpreted as a Wrapper.py command.<br>
- Type /help to see a list of Wrapper.py commands.<br>
- To completely shutdown the wrapper, type /halt.</br>

- To enter passwords into the wrapper.properties.config file, use the `/password` console command to enter the applicable password:
    `/password Web web-password <new password>`

Please read our [wiki](https://github.com/benbaptist/minecraft-wrapper/wiki) for additional information and review the issues page before submitting bug reports.<br>
If you run into any bugs, please _do_ report them!

If you have questions, please use our [Gitter page](https://gitter.im/benbaptist/minecraft-wrapper) instead of creating an issue.


# API #
The references for the wrapper plugin API are here:
[Wrapper.py Plugin API](/documentation/plugin_api.md)

#### New Permissions System ####

A file in the wrapper root directory "superOPs.txt" now augments the "Ops.json" file.  Operators in the ops.json file can be assigned a higher (wrapper) OP level.  The contents of the file are laid out just like server.properties (lines of \<something\>=\<value\>).

Sample `superops.txt`:
```
Suresttexas00=5
BenBaptist=9
```
Higher op levels are required to run sensitive wrapper commands like `/perms`.

#### Plugins ####

The modern event list is updated with each build: [Wrapper events](/documentation/events.rst) <sup id="a3">[3](#f3</sup>

Check the 'example-plugins' and 'stable-plugins' folders to see some example plugins.  These are very useful for seeing how the API functions.

- TEMPLATE.py and EXAMPLE.py are mostly just shells of a plugin to work off of.  They contain useful tutorial comments.
- zombie.py is a fun test plugin that leaves behind undead versions of people when killed by undead mobs.
- speedboost.py gives everyone a speedboost when someone dies - similar to survival games.
- poll.py allows players to vote for certain things on the server. It isn't very up-to-date at the moment, however.
- Essentials is a plugin loosely based off of Essentials for Bukkit.
- WorldEdit - is a plugin loosely based on the WorldEdit for Bukkit by sk89q
- SmallBrother is a lightweight logging plugin based on the old Bukkit plugin, BigBrother
- Open.py is a plugin that opens a window with nothing.  This plugin was probably just a test plugin and may not work, but contains example code for accessing packets from the player api.

__Tip__:

If you want to see more error messages and other useful messages while developing plugins or debugging wrapper,
look for the logging.json file and make changes to the "console" section:

```json
...
        "console": {
            "stream": "ext://sys.stdout",
            "formatter": "standard",
            "class": "logging.StreamHandler",
            "filters": [
                "plugin"
            ],
            "level": "INFO" <-- Set to DEBUG for more detailed output
        },
...
```
<br><br>
<sup><b id="f1">1</b></sup> - The old stable branch "master", version 0.7.6, build 83 has now been archived in the ["Original"](https://github.com/benbaptist/minecraft-wrapper/tree/Original) branch. The original
version only supports minecraft versions up to 1.8.    [↩](#a1)

<sup><b id="f2">2</b></sup> - Using wrapper means you also accept the EULA, which will be set to true in the eula.txt file in your server folder. [Mojang EULA](https://account.mojang.com/documents/minecraft_eula)   [↩](#a2)

<sup><b id="f3">3</b></sup> - The original Event list (Wrapper version 0.7.6) - [0.7.6 Wrapper list of events](https://docs.google.com/spreadsheet/ccc?key=0AoWx24EFSt80dDRiSGVxcW1xQkVLb2dWTUN4WE5aNmc&usp=sharing)   [↩](#a3)

