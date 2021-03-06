# mider
Utility to evade root detection where Magisk hide is insufficient.
# Disclaimer
I do not, will not, and will NEVER condone nor support nefarious uses of this utility of any kind.
Including, but not limited to, cheating, hacking, malware, spyware, etc.<br/>
If you have come here for any such purpose, please kindly show yourself out.
# Usage
Place somewhere it can execute, or as I've found, you can execute from internal storage by prefacing it with `sh` as in the aliases described later.<br/>
`mider` will display usage.<br/>
`mider hide` will hide root files and Magisk.<br/>
`mider unhide` will restore everything.<br/>
May add as a Magisk module in the future.
# Things to be aware of
The Magisk app REALLY doesn't handle it's files being misplaced.
While I have yet to notice adverse effects, I recommend avoiding opening the app while root is hidden.
Upon restoring, if the Magisk app reports no installation, it is necessary to kill the app, after which it will function normally.<br/>
TODO: Investigate the implications of killing and restarting it on restore.<br/>
Also, as a consequence of the app not seeing a valid install, you'll be inundated with update notifications unless you disable automatic checks.<br/>
TODO: Peruse Masgisk sources and determine a viable workaround that allows update checks to still be aware of the version... I doubt it can be done, however.
# Requirements
Not fully sure what Magisk versions would be supported, but it was created as of 19.
I will check back on the changelogs as to when the current directory structure was put in place.
# What it does
Pretty straightforward and simple shell script.
It simply moves all of Magisk's sensitive files to a temporary directory.
The unhide feature just moved them back and relinks them.
Basically, you are temporarily unrooted while hidden, and it's restored when unhidded.
Note that the contents of `/sbin` are restored at boot by Magisk's boot image.
# Why it might be necessary
Kind of a long story, but to keep it short, basicIntegrity and CTSprofile that most SafetyNet check apps return are... crude at best.
SafetyNet does MUCH more than these checks...
The bulk of my research on the matter is summed up nicely by this awesome fellow's write-up found here:<br/>
https://koz.io/inside-safetynet/<br/><br/>
The gist of it is that the API returns a bunch of anonymized data regarding the state of your device, which an app can send serverside to check.
One such attestation reports if root exists.
Most apps likely don't bother checking most of this extra info, or they are unaware that it is available.
# Tip to make the process easier
I got really annoyed having to execute the thing from my localstorage `/sdcard`, so I made some aliases to make things even easier.
Simply modify these to fit your environment.
`alias mide='su -c sh /sdcard/mider hide' unmide='/sbin/mide/magisk su -c sh /sdcard/mider unhide'`
This allows for the super easy one line usage I show in the screenshot.
Explanation: `-c` sends a command to be executed as root, leaving you in your own session, so you aren't root when it's done.
This is nice because you don't need to worry about accidentally leaving an open root session.
To make things even easier, Terminal Emulator (jackpal.androidterm) has the option "Initial Command" where this alias may be entered so you don't have to worry about trying to figure out a `.profile` or other rc file.
# Security bonus
Most users who root, often don't need it after boot, or after they've done what they wanted.
Even in other use cases, root is usually something you don't need often.
Disabling it entirely like this can help protect your device from potential threats targeting root.
Personally, I immediately run `mider hide` immediately after Magisk's modules have initialized, and my kernel settings have been applied.
The only thing I have use for it afterwards is if I have need to lift my clock restrictions for a more demanding app.
# Alternative easier solution
So, this was also my solution's alpha, if you will.
As `/sbin/` is restored at boot, a simple `su -c rm -rf /sbin` works if you don't even care to have root after bootup.
However, some modules also operate from `/sbin`, so this disrupts them too.
You could just delete the specific files that `mider` does though.
I think I'll add that as a 3rd option as a future feature. 
