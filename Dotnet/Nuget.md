# Creating a Nuget Project

Before you create a NuGet package, you need to set some metadata in your project file (MyLibrary.csproj) to define how the package will be published. Open MyLibrary.csproj in VS Code and add the following elements under the `<Project>` node to provide the metadata:

```xml
<Project Sdk="Microsoft.NET.Sdk">

	<PropertyGroup>
        <!-- PackageType Template is for template projects only -->
		<PackageType>Template</PackageType>
		<PackageId>mmc.MyLibrary</PackageId>
		<PackageVersion>1.0.0</PackageVersion>
		<TargetFramework>net9.0</TargetFramework>
		<IsPackable>true</IsPackable>
		<Title>Package Title</Title>
		<Authors>Emre Mumcu</Authors>
		<Company>mumcu.net</Company>
		<Description>Package Description</Description>
		<RepositoryUrl>https://github.com/*.git</RepositoryUrl>
		<PackageLicenseExpression>MIT</PackageLicenseExpression>
		<PackageTags>package; tags;</PackageTags>
		<Copyright>Copyright © 2024</Copyright>
		<PackageProjectUrl>https://emre-mumcu.github.io</PackageProjectUrl>
		<PackageReadmeFile>readme.md</PackageReadmeFile>
		<PackageIcon>mmc-icon.png</PackageIcon>
		<IncludeContentInPack>true</IncludeContentInPack>
		<IncludeBuildOutput>false</IncludeBuildOutput>
		<EnableDefaultContentItems>false</EnableDefaultContentItems>
		<NoDefaultExcludes>true</NoDefaultExcludes>
	</PropertyGroup>

	<ItemGroup>
		<Content Include="**\**" Exclude="**\bin\**;**\obj\**;**\.vs\**;**\.git\**" />
		<Content Remove=".gitignore; .gitattributes; editorconfig;" />
		<None Include="mmc-icon.png" Pack="true" PackagePath="\" />
		<None Include="readme.md" Pack="true" PackagePath="\" />
	</ItemGroup>

	<ItemGroup>
		<FrameworkReference Include="Microsoft.AspNetCore.App" />
	</ItemGroup>    

</Project>
```

Once the .csproj file is configured, build your project to make sure everything is working properly.

dotnet build

To create a NuGet package, run the following command in the terminal:

dotnet pack

This will generate a .nupkg file in the bin\Debug (or bin\Release if you build in release mode) directory.

# Publish the Package (Optional)

To share your NuGet package with others or use it in other projects, you can publish it to a NuGet repository. 

First, you need to register for a NuGet.org account and get your API key. Once you have your API key, run:

dotnet nuget push bin/Debug/MyLibrary.1.0.0.nupkg --api-key YOUR_API_KEY --source https://api.nuget.org/v3/index.json

If you want to publish the package to a private NuGet server, replace the URL with the URL of your private repository.

Once your package is published, you can verify it by searching for it on NuGet.org (if published there) or by installing it in a different project:

dotnet add package MyLibrary --version 1.0.0

# Automating with VS Code Tasks (Optional)

You can create custom tasks in VS Code to automate some of these steps (build, pack, push). Create a .vscode/tasks.json file in your project:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build Project",
      "type": "shell",
      "command": "dotnet build",
      "group": "build",
      "problemMatcher": "$msCompile"
    },
    {
      "label": "Pack Project",
      "type": "shell",
      "command": "dotnet pack",
      "group": "build",
      "problemMatcher": "$msCompile"
    },
    {
      "label": "Publish to NuGet",
      "type": "shell",
      "command": "dotnet nuget push bin/Debug/MyLibrary.1.0.0.nupkg --api-key YOUR_API_KEY --source https://api.nuget.org/v3/index.json",
      "group": "build",
      "problemMatcher": "$msCompile"
    }
  ]
}
```

This allows you to run these tasks directly from VS Code's "Terminal" > "Run Task" menu.