Hub references in the following places:

* Release\WebMisc\Register.bat
* Release\Web\ProvisionTransfer.vbs
* Release\Web\OSATransfer.vbs

Both *.vbs files are called from ComputeClass.asa

Register.bat includes the following line (among many others):

C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\regasm "D:\InetPub\WWWROOT\Server.net\Dagger\hub.dll" /codebase

## OSA Issues

They are trying to reference hub.dll in their new web project, but hub.config is not getting copied to ASP.NET Temporary Files.

When a user first hits a website, IIS creates a shadow copy of the sites dependencies inside ASP.NET Temporary files. It copies the assemblies found in the bin folder as well as App_Code, etc. This is called 'Dynamic Compilation' and more can be found here: http://msdn.microsoft.com/en-us/library/ms366723.aspx

## Resources

* C# DLL Config File Issue: http://stackoverflow.com/questions/594298/c-sharp-dll-config-file/1009227#1009227
* Where's my DLL config: http://blogs.visoftinc.com/2013/10/21/dude-wheres-my-dll-config/
	* This one offers a solution that may work for us.
* This offers a similar solution, but using Path.GetDirectoryName: http://stackoverflow.com/questions/2427822/can-i-use-reflection-to-find-the-bin-configuration-folder-in-asp-net-instead-o






