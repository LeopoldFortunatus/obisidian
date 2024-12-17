-----Recommended Nexus Mods-----  
[RE_Kenshi](https://steamcommunity.com/linkfilter/?u=https%3A%2F%2Fwww.nexusmods.com%2Fkenshi%2Fmods%2F847)[www.nexusmods.com] (Performance and stability)  Done
[CTD fix](https://steamcommunity.com/linkfilter/?u=https%3A%2F%2Fwww.nexusmods.com%2Fkenshi%2Fmods%2F506)[www.nexusmods.com] (Fixes glitches)   Done
[Engine Mesh Updater](https://steamcommunity.com/linkfilter/?u=https%3A%2F%2Fwww.nexusmods.com%2Fkenshi%2Fmods%2F1212)[www.nexusmods.com] (Fixes crashes)  
[Performance Fix](https://steamcommunity.com/linkfilter/?u=https%3A%2F%2Fwww.nexusmods.com%2Fkenshi%2Fmods%2F1216)[www.nexusmods.com] (Reduces load times)

mods on Linux - https://www.nexusmods.com/kenshi/articles/157

To run any of SCARaw's batch jobs on linux (and any unfortunately-windows program for kenshi, really), if you're using steam (aka proton) to run the game, you can just copy/paste this:  
`WINEPREFIX=~/.steam/steam/steamapps/compatdata/233860/pfx wine /path/to/Whatever_Job.bat`  
(if you use a non-standard path for Steam, you'll obviously need to change where that first part points to to whatever you changed it to)  
  
This will use the "windows environment" you're already using for Kenshi to run the program. I've done this for MeshUpdater, PSO, PerformanceFix, and stuff like RE_Kenshi. All works fine  
  
  
And just to note it cuz a lot of pages redirect here, for some of these they'll give you dlls to place next to your Kenshi.exe. To make sure wine loads those, you need to something like this to your launch options in steam:  
`WINEDLLOVERRIDES="dxgi,d3d11=n,b" %command%`  
(this example is for the vulkan mod, but add into those quotes whatever other stuff some particular mod tells you too)  
https://wiki.winehq.org/Wine_User's_Guide#WINEDLLOVERRIDES=DLL_Overrides

Example:
```
export WINEPREFIX=~/.steam/steam/steamapps/compatdata/233860/pfx

~/.steam/debian-installation/steamapps/common/Proton\ Hotfix/files/bin/wine ~/.steam/debian-installation/steamapps/common/Kenshi/downloads/RE_Kenshi_installer-847-0-2-17-1731714189/RE_Kenshi_v0.2.17.exe
```
