<h1 align="center">SKSEPlugins</h1>
Scripted template for setting up SKSE64 development environment for the latest CommonLibSSE-NG. (Multi targeting AE/SE/VR)  

[中文指南](https://github.com/gottyduke/PluginTutorialCN)


<h2 align="center">Differences from CommonLibSSE-NG-ExamplePlugin</h2>

+ Unified workspace setup. Managing all plugin projects together, build as needed.  
+ Enabled CommonLibSSE in Visual Studio IDE, able to view/modify/recompile CLib code on the fly.
+ Integrated building-testing-deploying with post build control tool.
+ Supports custom maintained fork of CommonLibSSE-NG.
+ Supports plugin configuration files in Visual Studio IDE.


<h2 align="center">Requirement</h2>

+ [CMake](https://cmake.org)
+ [Git](https://git-scm.com)
+ [Visual Studio 17 2022](https://visualstudio.microsoft.com)


<h2 align="center">Bootstrap</h2>

Clone this repo onto your local environment, execute the command in `PowerShell` with **admin privilege(needed for `bootstrap`)**:  
```powershell
git clone https://github.com/gottyduke/SKSEPlugins
cd .\SKSEPlugins
powershell -executionpolicy bypass -command "& .\!rebuild.ps1 -bootstrap"
```  
`bootstrap` process is necessary for setting up environment variables!  

<h2 align="center">Update</h2>

To get the latest `SKSEPlugins` updates:  
```powershell
cd <Path/To/SKSEPlugins>
git pull origin master
.\!Rebuild -bootstrap -update
```

<h2 align="center">New Project</h2>

```powershell
!MakeNew <PluginName> 
    [-install 'PluginMO2InstallName'] 
    [-message 'Short description for plugin project'] 
    [-vcpkg 'Lib1[Feature1, Feature2]', 'Lib2', 'Lib3']
```
To generate a basic project:
```powershell
.\!MakeNew MyBasicPlugin
```

To generate a project with `imgui` and `xbyak` vcpkg dependencies:  
```powershell
.\!MakeNew MyProject -vcpkg 'imgui', 'xbyak'
```  
The `-install` parameter specifies the name for installation, used in post build control for MO2 support.

<h2 align="center">Generate VS Solution</h2>

```powershell
!Rebuild <AE|SE|VR|ALL|PRE-AE|FLATRIM>
    [-C|-Custom] 
    [-N|-NoBuild]
    [-WhatIf]
    [-DBG]
    [-D]
```
|Configuration|Runtime targeting|
|-|-|
|`AE`|Anniversary edition only(1.6.317+)|
|`SE`|Special edition only(1.5.3+)|
|`VR`|VR edition only(1.3.64)|
|`ALL`|Default; All editions|
|`PRE-AE`|Special edition(1.5.3+) and VR edition(1.3.64)|
|`FLATRIM`|Anniversary edition(1.6.317+) and speical edition(1.5.3+)|  

Configuration parameter is case-insensitive. `ALL` configuration is set by default and therefore can be omitted.  

|Switch|Description|
|-|-|
|`-C\|-Custom`|Use designated custom CommonLib for solution|
|`-N\|-NoBuild`|Disable the CommonLib compilation after generating solution|
|`-WhatIf`|Do not initiate CMake generator; Preview `CMakeLists.txt` instead|
|`-DBG [Project]`|Toggle debugger build for `Project`, i.e. testing suites|
|`-D [CMAKE_ARGUMENTS]`|Pass additional arguments to CMake generator, same as CMake format|

To generate a solution targetting special edition(1.5.3+) and anniversary edition(1.6.317+):  
`.\!Rebuild flatrim`  

To generate a solution targetting all editions with custom CommonLib(`ALL` is omitted):  
`.\!Rebuild -c`  

To generate a solution targetting special edition(1.5.3+) only and do not compile CommonLib after `!Rebuild`:  
`.\!Rebuild se -n`  
[![Report.png](https://i.postimg.cc/rpmByPWv/Report.png)](https://postimg.cc/rDBnQgfJ)

<h2 align="center">Post Build Control</h2>

A handy post build control tool to help with developing-testing-deploying plugin in a speedy & convenient fashion.  
[![Post-Build-Control.png](https://i.postimg.cc/L8jgsxk6/Post-Build-Control.png)](https://postimg.cc/K1v8qrsd)  
Deploy straight into MO2 with proper file structure, along with all supported files automatically. `scripts(.psc)`, `shockwave files(.swf)`, `configuration files(.ini|.toml|.json)`.  
[![MO2.png](https://i.postimg.cc/pdhs6bKX/MO2.png)](https://postimg.cc/R3m1WYPj)  
Refresh MO2(`F5`) and start debugging! Or you can deploy straight into game's data folder, and launch desired SKSE without the struggle of MO2. Once done testing, you may also optionally remove the deployed plugins for game data's integrity.  
[![Log.png](https://i.postimg.cc/KjRjnNdV/Log.png)](https://postimg.cc/WqcsVMp6)  

<h2 align="center">VCPKG</h2>

`!Rebuild` auto gathers vcpkg dependencies from sub projects and build them if needed.  

<h2 align="center">Add/Remove files</h2>

Supports **directly** adding/removing files in VS by right clicking `Add New Files`, it will be carried over to the source folder upon building. When reloading VS projects after the second build, the newly added files may lose focus in VS tabs if they were open before reloading, simply re-open them from VS solution explorer.   

<h2 align="center">CommonLib</h2>

`!Rebuild` builds with [default CommonLibSSE-NG](https://github.com/CharmedBaryon/CommonLibSSE-NG). To use a custom CommonLib, then append the switch `-c` or `-custom` to the `!Rebuild` command. Custom CommonLib has to be prepared in `bootstrap` process first!  
However, `!Rebuild` uses custom pregenerated `CMakeLists.txt` in place of the default one came with CommonLibSSE.

<h2 align="center">DKUtil</h2>

This build setup bundles `DKUtil` header library, full documentation can be found [here](https://github.com/gottyduke/DKUtil).


<h2 align="center">Migrating to SKSEPlugins</h2>

Simply use `!MakeNew` script to make new plugin project with the same name, then copy every source file **except** `main.cpp`, `PCH.h`, and `Version.h` into the new project's `src` folder. API or external header files can be placed in `include` folder. After doing so, manually check the `CMakeLists.txt` and `vcpkg.json` to address missing dependency if there's any. You may change the versioning after first successful build or manually changing the two above-mentioned files.  

Do a full `!Rebuild` with the new project, and modify new project's `main.cpp` accordingly to adopt your old plugin's changes within VS.  

---
<p align="center">Author: Dropkicker & Maxsu @ 2022</p>
