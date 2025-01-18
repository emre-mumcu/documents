Check for Outdated Packages

You can install the dotnet-outdated tool to find outdated packages:
dotnet tool install --global dotnet-outdated-tool

Then, run:
dotnet outdated

This will display a list of outdated packages along with their current, latest, and compatible versions.

Update All Packages

While there isn’t a built-in command in the dotnet CLI to update all packages at once, you can use dotnet-outdated with the --update option:

dotnet outdated --update

This will update all outdated packages in your project.

Restore After Updating
After updating packages, make sure to restore the dependencies:

dotnet restore

