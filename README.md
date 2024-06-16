Hacky OneLoader fork that:
- adds support for the official macOS port of OMORI, which does not use the /www subfolder
- brings back support for nwjs 0.77.0 (required by the Apple Silicon compatibility patch) which was present in 1.2.1
- tells the user to restart the game if that's required to apply changes to package.json

To install on a Mac:
1. If you are on a Apple Silicon machine: Apply [this compatibility patch](https://github.com/SnowpMakes/omori-apple-silicon/blob/master/patch.md)
2. Download [77Loader](https://github.com/nift4/77Loader/archive/refs/heads/master.zip) and extract the ZIP file
3. In your Steam library, right-click OMORI and open local game files
4. Right-click the file `OMORI.app` and press "View package contents"
5. Enter `Contents`, then `Resources`, then `app.nw` folders
6. In another Finder window, open the `www` folder you extracted from the ZIP file earlier, copy all files inside it (NOT the folder itself) and paste it into the `app.nw` folder, then replace files when asked
7. Have fun!

If you want to install this on a Windows machine for the restart notification feature or just for easier modpack distribution, this is working just fine - you can install exactly like you would install OneLoader.

Please note that I have only tested this on an Apple Silicon and a Windows machine, but feel free to hit me up if there are any issues with Intel Macs or if you need help in general.

Original README below:

# OneLoader
High performance mod loader for OMORI

## Credits/Licensing
This project contains in part or in full the following projects:

- [GOMORI](https://github.com/gilbert142/gomori) | License: **MIT** | Degree of use: Certain encryption primitives, the index.html used for backwards compatibility
- [node-stream-zip](https://github.com/antelle/node-stream-zip) | License: **MIT** | Degree of use: Library used in full to process mods bundled in zip files
- [JSON-Patch](https://github.com/Starcounter-Jack/JSON-Patch) | License: **MIT** | Degree of use: Library used in full to apply patches to game data
- [Rollup](https://github.com/rollup/rollup) | License: [Multiple](https://github.com/rollup/rollup/blob/master/LICENSE.md) | Degree of use: Building plugins shipped as ES modules

Additionally, I'd like to thank the developers of GOMORI for kick-starting the modding community and laying the necessary ground work for modloader development.

## Installation:
Please reference the [mods.one modding wiki](https://omori.wiki.mods.one/installing_oneloader) for instructions on how to install

## Making mods
Because this modloader aims to be a drop-in GOMORI replacement, you can follow mod making instructions from the [GOMORI repository](https://github.com/gilbert142/gomori). As more features are added to OneLoader that GOMORI doesn't implement, they will be implemented here.

## Usage
- **Logging:** OneLoader writes a text file into the game's main folder called `latest.log`. It contains a timestamped list of everything that has happened since the game started that has relevance to the debugging of OneLoader.
- **Command line parameters:** While OMORI by default doesn't accept any command line parameters, and worse, breaks, OneLoader allows you to specify command line options and itself accepts two:
  - `--no-mods`: Start the game in safe mode, skip loading and injection of any mods
  - `--dump-overlay`: Dumps all "built" files to a directory on disk for debugging purposes. Can be used to create modloader-less mods that simply replace game files
- **Conflict resolution:** When you add any mods that conflict with each other, please be prepared to use a mouse to select which mod to prefer during the conflict resolution step.

## Advantages and Disadvantages vs GOMORI
### Advantages
- OneLoader is capable of loading large mods in a short amount of time
- OneLoader has a significantly smaller memory footprint (Tested on the [OMOCORD MOD](https://mods.one/mod/omocord), GOMORI used 1.6 GB on game main menu, OneLoader used 200 MB). This means that you can actually run larger amounts of mods on weaker machines (or powerful machines hindered by high idle memory usage or a low amount of memory available in the first place)
- OneLoader has a semi-graceful system for handling file conflicts: When 2 mods attempt to modify the same file, OneLoader will prompt the user asking which mod they would like to prioritize. GOMORI on the other falls back to undefined/undocumented behaviour and throws an error.
- OneLoader produces a human-readable and fairly precise log file which can significantly aid in mod and modloader debugging.
- On SSD systems, OneLoader is much gentler when it comes to write cycles: The only writes it does are when creating logs, meaning around 50 KB per game session. GOMORI on the other hand has to patch all game files on disk every time it starts, causing significant wear with large mods or mod packs.
- OneLoader doesn't leave any residue in main game files and assets (Unlike GOMORI's .BASIL files mandatory for restoring removed mods)
- OneLoader doesn't require that your zips and mod folders be named precisely following the mod id, they can have any name.
- OneLoader will run even without a `mods` folder or a `save` folder present. It will simply create them.

### Disadvantages
- GOMORI has theoretically marginally higher runtime performance than OneLoader, although the difference is small enough that on most machines it doesn't matter.
- GOMORI doesn't lock the mods folder, allowing you to edit it while the game is running
- OneLoader cannot load mods that utilize the `exec` field in the `mod.json`. (According to internal analysis, no mod on mods.one makes use of that field)

## Reporting issues
If you encounter any issue or bug, please open a github issue in this repository and attach the following:
- List of all mods you have installed (download links or files)
- A copy of the `latest.log` file from the game session where the bug/error occured

## Get in touch with the developer
You can reach me via Discord through DMs ( Rph#9999 is my tag ), via the [OMORI discord](https://discord.gg/omori) (Simply tag me in modding-discussion) or via the [mods.one discord](https://discord.gg/EDTMF85Hnp)
