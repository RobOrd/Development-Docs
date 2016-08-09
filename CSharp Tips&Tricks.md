
### Compilar una solución sin VS ###
Para compilar una solución completa y generar los binarios correspondientes solo disponiendo del código fuente y en NET Framework y sin necesidad de Visual Studio.

- Entrar al directorio del NET Framework
- Ejecutar MSBuild.exe con los parametros correpondientes y la ruta de la solución.

	    cd windows
    	cd microsoft.net
    	cd framework
    	cd v4.0.30319
    	
    	msbuild.exe "c:\temp\VeiderSoft.AgileTools-master\VeiderSoft.AgileTools.Main.sln" /t:Rebuild /p:Configuration=debug /p:Platform="x86"
	

En caso de que la solución tenga dependencias de Nuget.

- Download the [NuGet.exe](http://nuget.codeplex.com/releases/view/58939) Command Line boostrapper and, for example, place it inside the solution directory.
- Open a command prompt and change into the solution directory.
  
    	cd "C:\Users\Oliver\Documents\My Project"

- Invoke NuGet.exe to update the packages required for this solution:
	
		NuGet.exe install "My Project/packages.config" -o packages/




>Implementar esto como un feature de AgileTools