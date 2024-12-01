# Change bin and obj folder locations

To change the bin and obj folder locations in an ASP.NET Core project using Visual Studio Code (VSCode), you'll need to modify your project's .csproj file. Specifically, you can set custom paths for the bin and obj directories by adding or modifying the OutputPath and BaseIntermediateOutputPath properties.

Open your project’s .csproj file, which is located at the root of your project directory. Then, add or modify the following properties within the `<PropertyGroup>` tag:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <!-- Custom output path for compiled binaries -->
    <OutputPath>..\..\build\bin\</OutputPath>    
    <!-- Custom intermediate output path for obj files -->
    <BaseIntermediateOutputPath>..\..\build\obj\</BaseIntermediateOutputPath>
  </PropertyGroup>

</Project>
```

You can also set the output paths for specific build configurations (like Debug or Release) if necessary, using a more granular `<PropertyGroup>` section with a Condition attribute.

```xml
<PropertyGroup Condition="'$(Configuration)'=='Debug'">
  <OutputPath>..\..\build\debug\bin\</OutputPath>
  <BaseIntermediateOutputPath>..\..\build\debug\obj\</BaseIntermediateOutputPath>
</PropertyGroup>

<PropertyGroup Condition="'$(Configuration)'=='Release'">
  <OutputPath>..\..\build\release\bin\</OutputPath>
  <BaseIntermediateOutputPath>..\..\build\release\obj\</BaseIntermediateOutputPath>
</PropertyGroup>
```


Explanation:

* `OutputPath` This controls where the final compiled assemblies (DLLs) are placed.
* `BaseIntermediateOutputPath` This controls where the intermediate files like .obj files, which are created during the build process, are placed.

After making these changes, it's a good idea to clean and rebuild the project to ensure the new folder structure takes effect.

```zsh
% dotnet clean
% dotnet build
```

After the build, check the specified locations for the bin and obj directories. They should now be placed in the custom paths you set.