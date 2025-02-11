# Setting up for a multi-project workspace (monorepo) 

https://angular.dev/reference/configs/file-structure 

Use the following command to create a new workspace with all of the workspace-wide configuration files, but no root-level application. 

```zsh
% ng new my-workspace --no-create-application 
```

NOTE: If you get error `npm error code ETARGET` when running the above command, clean npm cache then re-run it. 

```zsh
% npm cache clean --force 
% ng new my-workspace --no-create-application 
```

Then, you can generate applications and libraries with names that are unique within the workspace. 

```zsh
% cd my-workspace 
% ng generate application my-app 
% ng generate library my-lib 
```

# Sample 

```zsh
% npm cache clean --force 
% ng new workspace --no-create-application --skip-git --skip-tests 
# Edit the default projects folder newProjectRoot in angular.json of workspace if you like 
% cd workspace 
% ng generate application my-app --skip-tests 
% ng generate library my-lib 
```

# Remove an application from an Angular workspace 

To remove an application from an Angular workspace, you need to perform several steps manually because there is no single command to remove an application using the Angular CLI. 

1. Delete the Application Directory: rm -rf projects/my-application 
2. Update angular.json: Remove the application's configuration from the angular.json file. Open angular.json. Find the entry for the application under the projects section. Remove the entire entry for the application and update the defaultProject if necessary. 
3. Update tsconfig.json and Other TypeScript Configuration Files. 
4. Update package.json (if necessary): If the application had specific dependencies listed in the package.json of the root workspace, you might want to remove those dependencies if they are not used by other applications. 
5. Update Workspace Configuration Files: There might be other configuration files that reference the application, such as CI/CD configuration files, karma.conf.js, protractor.conf.js, etc. Review these files and remove any references to the removed application. 
6. Run npm install: If you removed dependencies from package.json, run npm install to update the node_modules directory. 

```zsh
% npm install 
% ng generate application vs ng new 
% ng generate application and ng new are two Angular CLI commands that serve different purposes and are used in different scenarios. 
% ng new
```

The ng new command is used to create a new Angular workspace and a new initial Angular application. This command is typically used when you are starting a new Angular project from scratch. 

```zsh
% ng new my-app 
% ng new my-angular-app 
```

After running this command, you will have a new directory my-angular-app with a complete Angular project setup, including the initial application, package.json, angular.json, src directory, etc.  What It Does: 

1. Creates a new directory named my-app. 
2. Sets up a new Angular workspace in that directory. 
3. Generates an initial Angular application within the workspace. 
4. Installs the necessary dependencies. 
5. Creates a new angular.json file to manage the workspace configuration. 
6. Initializes a Git repository (optional). 

```zsh
ng generate application 
```

The ng generate application (or ng g application) command is used to add a new application to an existing Angular workspace. This is useful when you want to manage multiple Angular applications within a single workspace (monorepo). 

```zsh
% ng generate application my-new-app 
```

What It Does: 

1. Adds a new application to the existing workspace. 
2. Updates the angular.json file to include the new application configuration. 
3. Creates a new directory for the application within the projects directory. 
4. Does not reinstall dependencies (since the workspace already has them). 

```zsh
% cd my-angular-workspace 
% ng generate application my-new-app 
```

After running this command, you will have a new application my-new-app under the projects directory within the existing workspace. The angular.json file will be updated to include the new application's configuration. 

# Prerendering (SSG) 

Prerendering, commonly referred to as Static Site Generation (SSG), represents the method by which pages are rendered to static HTML files during the build process. 

Prerendering maintains the same performance benefits of server-side rendering (SSR). But achieves a reduced Time to First Byte (TTFB), ultimately enhancing user experience. The key distinction lies in its approach that pages are served as static content, and there is no request-based rendering. 

When the data necessary for server-side rendering remains consistent across all users, the strategy of prerendering emerges as a valuable alternative. Rather than dynamically rendering pages for each user request, prerendering takes a proactive approach by rendering them in advance. 

To prerender a static page, add SSR capabilities to your application with the following Angular CLI command: 

```zsh
% ng add @angular/ssr 
```

Once SSR is added, you can generate the static pages by running the build command: 

```zsh 
% ng build
```

# Run the application 

```zsh
% cd my-app 
% ng serve --open 
```

The ng serve command launches the server, watches your files, and rebuilds the app as you make changes to those files. The --open (or just -o) option automatically opens your browser to http://localhost:4200/.

# angular.json and package.json 

angular.json and package.json serve different purposes in an Angular project. 

## package.json 
package.json is a standard file in all Node.js projects, including Angular projects. It contains metadata about the project, as well as scripts, dependencies, and other configurations. It includes: 

1) Metadata: Basic information about the project, such as name, version, description, author, and license. 
2) Scripts: Custom commands that you can run using npm run <script-name>. 
3) Dependencies: Lists of dependencies required for the project to run (dependencies) and for development purposes (devDependencies). 
4) Engines: Specifies the versions of Node.js and npm that are required. 

## angular.json 

angular.json is specific to Angular projects and is generated by the Angular CLI. It contains configuration options for your Angular applications and libraries. It includes: 

1. Projects: Defines the configuration for each Angular project within the workspace, including applications and libraries. 
2. Default Project: Specifies the default project to use when no project name is provided to the Angular CLI commands. 
3. Architect: Defines various build targets and configurations for building, serving, testing, and linting the project. 

# Find out which node_modules folder an Angular project is using 

By default, the node_modules folder is located in the root directory of your Angular project, alongside the package.json file. This is where all the dependencies listed in package.json are installed. 

# Run ng version in your terminal within the project directory. 

Run npm list to see all the installed packages and their versions. 

Check Environment Variables: Sometimes, the NODE_PATH environment variable might be set to specify a different location for node_modules. 



# NOTES:
/.angular/cache should be added to .gitignore 

By default paths are relative to src folder in Angular CLI (see the root value of the .angular-cli.json file to confirm). To include the test.md file to the list of files that angular compiler will keep in the package once compiled. 

```json
 "assets": [ 
  "assets", 
  "favicon.ico", 
+ "app/test.md", 
], 
// or 
{ "glob": "**/*.md", "input": "src/", "output": "/" } 

// Use:
// <markdown [src]="'app/test.md'"></markdown> 
// ng serve --configuration=dev 
```

# Sample

```zsh
# Create Service: 
% ng g service _services/auth
# Create Guard: 
% ng g guard _guards/auth 
# Create Application in the Smae Directory: 
% ng new AppName --directory ./
```

# References

https://github.com/PrismJS/prism-themes 
https://cdnjs.com/libraries/prism-themes 
https://angular.dev/overview
https://angular.dev/tools/cli
