# Change About This Mac

Want to show your actual CPU? Want an absolutely cursed image in your about this Mac? Follow this easy guide!

#### Requirements

* macOS Mojave/Catalina
* Xcode for plist/hex editor
  * Alternative .plist editor of choice
  * Alternative hex editor of choice
  * Alternate .strings editor of choice
* [ThemeEngine](https://github.com/alexzielenski/ThemeEngine/releases) to edit .car files(Mojave only, see note below)
* SIP must be disabled temporarily. It can be reenabled afterwards

Although this guide may work on actual Macs, no promise is made. This guide is intended for hackintoshes only, changes made are purely cosmetic.

##### Catalina Compatability

Currently, ThemeEngine is not working on Catalina, as it is the only method I could find of editing .car files, this means you cannot currently change the icon in About this Mac.

In addition, on Catalina, root must be mounted read/write. To do this paste in a terminal `sudo mount -uw /` then hit enter. This will prompt for your password, enter it and you will be able to change all files on your Catalina system.

### Preparation

Create 2 folders in a location of your choice\(Terminal commands assume desktop\), one named backup & another folder named modified. The backup folder serves as a backup obviously, while we will make actual changes in the modified folder.

## Change icon(Currently Mojave only)

#### Prerequisite

As stated above, ThemeEngine only works on Mojave at the moment, it is required for editing .car files

Create or download yourself a squared, preferably 512x512 .png file you will use

#### Steps to replace

Open a Terminal window and paste the following `cp /Applications/Utilities/System\ Information.app/Contents/Resources/Assets.car ~/Desktop/modified/ && cp /Applications/Utilities/System\ Information.app/Contents/Resources/Assets.car ~/Desktop/backup/`  This will copy the original file to both locations.

Open ThemeEngine and click the File option and choose Open, Select the file in your modification folder and open Assets.car. Scroll down on the left sidebar until you get to SystemLogo:

![Before](.gitbook/assets/before-icon.png)

There are versions for both light and dark mode, with a '@2x' scale version for each. \(By default, the 2x scaled versions are the 2 middle ones\).

To replace the system icon, drag a .png file over the version you wish to replace:

![After](.gitbook/assets/after-icon.png)

When you're done, hit ⌘WinKey+S to save the Assets.car file

After this, return to the terminal window and type `sudo cp ~/Desktop/modified/Assets.car /Applications/Utilities/System\ Information.app/Contents/Resources/Assets.car` Hit enter, then it will prompt for your password. Enter your password, this replaces the original file with the new modified one.

NOTE: If you get permission error after entering the command, you have SIP enabled

After the file is replaced, you can open About This Mac and see your changes:

![This is cursed](.gitbook/assets/iconapplied.png)

## Changing Mac model name

Copy the original file for backup with

`cp ~/Library/Preferences/com.apple.SystemProfiler.plist ~/Desktop/backup`

Type in a terminal `open ~/Library/Preferences` and search for com.apple.SystemProfiler.plist

Open it in your plist editor of choice:

![Screen Shot 2019-09-26 at 22.19.20](.gitbook/assets/systemprofilerbefore.png)

Under CPU Names should be a string named with the last 4 digits of your serial number and language code.

Edit the Value part however you want. I'm going to set my motherboard name here for example:

![Screen Shot 2019-09-26 at 22.23.12](.gitbook/assets/systemprofilerafter.png)

Save the file and reboot for changes to take effect:



![Screen Shot 2020-01-08 at 8.12.38 PM.png](https://raw.githubusercontent.com/elsorino/Edit-About/master/2020/01/08-20-12-40-Screen%20Shot%202020-01-08%20at%208.12.38%20PM.png)

## Changing CPU name

Copy the original file with `cp /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/en.lproj/AppleSystemInfo.strings ~/Desktop/backup/ && cp /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/en.lproj/AppleSystemInfo.strings ~/Desktop/modified/`

This assumes you are using english as your system language.

The next step depends on what the CPU shows up in About this Mac already.

##### Unknown cpu



![Screen Shot 2020-01-08 at 7.49.04 PM.png](https://raw.githubusercontent.com/elsorino/Edit-About/master/2020/01/08-19-49-11-Screen%20Shot%202020-01-08%20at%207.49.04%20PM.png)

If your CPU shows up as unknown, then change the Unknown string to the right of UnknownCPUKind

##### Non-unknown Processor

If like me, your processor partially shows, then you instead will instead change SpeedAndTypeFormat to IntelSpeedAndTypeFormat. After this, to the right of IntelSpeedAndTypeFormat, if you wish to change the CPU frequency edit the "%1\$d" string. To edit the rest of it, replace the "%2\$@" with what you want. I will be putting my CPU name there, while moving the CPU frequency part so it appears after the CPU name.

![Screen Shot 2020-01-08 at 8.03.43 PM.png](https://raw.githubusercontent.com/elsorino/Edit-About/master/2020/01/08-20-03-48-Screen%20Shot%202020-01-08%20at%208.03.43%20PM.png)

Once you are done, save and exit xcode.

To apply your changes, insert the following into a terminal `sudo cp ~/Desktop/modified/AppleSystemInfo.strings /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/en.lproj/AppleSystemInfo.strings` This will prompt for your password, enter it and hit enter.

After this, open About this Mac to see your changes:

![Screen Shot 2020-01-08 at 8.14.53 PM.png](https://raw.githubusercontent.com/elsorino/Edit-About/master/2020/01/08-20-14-59-Screen%20Shot%202020-01-08%20at%208.14.53%20PM.png)

## Changing OS name

#### Preparation

The name you wish to use must be exactly 12 characters, spaces are allowed so if you wish to put less, just add spaces. Anything more or less will result in a malformed About This Mac.

#### Steps to edit

Start by backing up the file with `cp /System/Applications/Utilities/System\ Information.app/Contents/MacOS/System\ Information ~/Desktop/modified/ && cp /System/Applications/Utilities/System\ Information.app/Contents/MacOS/System\ Information ~/Desktop/backup/`

NOTE: On Mojave, it is located on /Applications instead of /System/Applications

After this, `cd ~/Desktop/modified` and run this command to remove the signature from the executable: `codesign --remove-signature System\ Information` This is required since we are modifying the binary directly.

If you're using Xcode, to open the binary in the hex editor, open Xcode &gt; File &gt; Open &gt; Desktop/modified/System Information Once it is opened hit View &gt; Navigators &gt; Show project Navigator. After this right click on the app in the newly open app &gt; Open as Hex:

![Screen Shot 2019-09-26 at 23.44.37](.gitbook/assets/hexeditor.png)

Hit CMD/Winkey + F to search for MacOS Mojave/Catalina. On the very left side, this should be numbered at 334495.

Now edit the MacOS Mojave/Catalina text to anything less or equal to 12 characters. Remember that if it is less than 12 characters, add extra spaces until it is 12 characters.

After this, save and exit Xcode.

Enter this into the terminal to replace the original binary with our modified one:

`sudo cp ~/Desktop/modified/System\ Information /Applications/Utilities/System\ Information.app/Contents/MacOS/System\ Information`

After this, you can open About this Mac and your results should appear:

![Finalresult](.gitbook/assets/finalresult.png)

## Finish

Now that you're is done, you can reboot with SIP enabled, and you should be good!

Errors? Something wrong? Open an issue or add me on Discord at elso#0307 and we'll discuss it.




