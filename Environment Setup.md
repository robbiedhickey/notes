# Environment Setup

## Errors when running InstallDevRuntime.W7.vbs

* Registering Generic Storage - GenricStorage.dll failed to load
* Registering GSCompression - GSCompression.dll failed to load
* Registering Dyna Zip - duzactx.dll, duzsactx.dll, dzactz.dll, dzsactx.dll failed to load
* Installing DevSwitch Utility - Installation package cannot be opened
* Installing EFile View generator - Installation package cannot be opened

For the DLL failures, I looked in SetupWebDevPC.W7.bat file to see what it was doing. It is copying dll dependencies from the following locations, none of which reference the DLLs that are missing for the install:

* \\CR-RSFILERS-V01\delivery\Web Server\insource rs\wwwroot\server\*.dll
* \\CR-RSFILERS-V01\delivery\Utilities\RIACryptUtil\*.dll
* \\CR-RSFILERS-V01\delivery\Utilities\clrstuff\system32\msvc*.dll

## IIS Setup

I Followed instructions in $/OnesourceIncomeTax/RS/wip/Build/dotNet/OneSourceIncomeTaxWeb/LocalWebSiteConfiguration/ReadMe.txt. 

It created the TrunkWebSite and BranchWebSite, but errors on CreateWipWebSite.bat. Message indicates "failed to add duplicate collection element OIT" and "failed to add duplicated collection element OITWebWip". The OIT is the app pool so that makes sense, but I can't explain the OITWebWip.

I checked %windir%\system32\inetsrv\config\applicationHost.config and did not see OITWebWip. 

### Solution

Had to delete the default web site, it was internally referenced as OITWebWip.

