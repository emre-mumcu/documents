# npm config 

```zsh
npm config set proxy http://proxy.****.intra:80 
npm config set strict-ssl false -g 
npm config set registry https://nexus.sgk.intra/repository/npm-group/
```

# NODE & NPM Update 

```zsh
# Check Version 
% node --version 
# First update npm 
% npm install -g npm stable 
# Then update node 
% npm install -g node 
# or 
% npm install -g n 
# Check after version installation 
% node --version 
# or 
% node -v 
```

# UPDATE NODE (Alternative) 
```zsh
# Clear the NPM cache 
% npm install -g n 
# Install a new version of Node 
% n latest 
# or 
% n stable 
# or specific version 
% n 0.8.20 
# Remove previously installed versions 
% n prune 
```

# UPDATE NPM (Alternative) 

```zsh
% npm version 
# Clear NPM's cache: 
% npm cache clean -f 
# Just as you use NPM to update packages, you can use NPM to update itself. 
% npm install -g npm@latest 
% npm install -g node 
```

# List outdated Packages: 

```zsh
% npm outdated 
```

# Update Packages: 

```zsh
% npm update 
```

# Update Angular 

```zsh
% ng version 
% npm uninstall -g @angular/cli 
% npm install -g @angular/cli@latest 
```

# Create Project Structure 

```zsh
# dotnet 
C:\Users\emumcu2\Desktop>mkdir NetAng 
C:\Users\emumcu2\Desktop>cd NetAng 
C:\Users\emumcu2\Desktop\NetAng>dotnet new webapi -o Backend --use-controllers 
C:\Users\emumcu2\Desktop\NetAng>dotnet new sln 
C:\Users\emumcu2\Desktop\NetAng>dotnet sln add Backend/. 
# angular 
C:\Users\emumcu2\Desktop\NetAng>ng new help 
C:\Users\emumcu2\Desktop\NetAng>ng new Frontend --skip-tests --skip-install --skip-git --style=css --ssr=false 
C:\Users\emumcu2\Desktop\NetAng>cd Frontend 
C:\Users\emumcu2\Desktop\NetAng\Frontend>npm install 
# open project 
C:\Users\emumcu2\Desktop\NetAng\Frontend>cd .. 
C:\Users\emumcu2\Desktop\NetAng>code . 
```

# .http Files 

https://learn.microsoft.com/en-us/aspnet/core/test/http-files?view=aspnetcore-8.0 

The .http file format and editor was inspired by the Visual Studio Code REST Client extension (https://marketplace.visualstudio.com/items?itemName=humao.rest-client). 
The Visual Studio 2022 .http editor recognizes .rest as an alternative file extension for the same file format. 

https://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html 
https://www.rfc-editor.org/rfc/rfc9110.html 

Create a file ***.http in VSCode and add requests. Then run the project:

```zsh
dotnet run 
```

Then run the requests using the Send Request header just above the request line. 