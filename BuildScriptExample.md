
## Azure pipeline tasks

[Azure pipeline tasks](https://docs.microsoft.com/nb-no/learn/modules/create-a-build-pipeline/4-plan-build-tasks)
````
#!/bin/bash

# Install Node.js modules as defined in package.json.
npm install --quiet

# Compile Sass (.scss) files to standard CSS (.css).
node-sass Tailspin.SpaceGame.Web/wwwroot

# Minify JavaScript and CSS files.
gulp

# Print the date to wwwroot/buildinfo.txt.
echo `date` > Tailspin.SpaceGame.Web/wwwroot/buildinfo.txt

# Install the latest .NET packages the app depends on.
dotnet restore

# Build the app under the Debug configuration.
dotnet build --configuration Debug

# Publish the build to the /tmp directory.
dotnet publish --no-build --configuration Debug --output /tmp/Debug

# Build the app under the Release configuration.
dotnet build --configuration Release

# Publish the build to the /tmp directory.
dotnet publish --no-build --configuration Release --output /tmp/Release
````
What are Azure Pipelines tasks?
In Azure Pipelines, a task is a packaged script or procedure that's been abstracted with a set of inputs.
An Azure Pipelines task abstracts away the underlying details. This abstraction makes it easier to run common build functions, like downloading build tools or packages your application depends on or running Visual Studio or Xcode to build your project.
Here's an example that uses the DotNetCoreCLI@2 task to build a C# project that targets .NET Core:
yml
````

task: DotNetCoreCLI@2
  displayName: 'Build the project'
  inputs:
    command: 'build'
    arguments: '--no-restore --configuration Release'
    projects: '**/*.csproj'
````
The pipeline might translate this task to this command:
bash

Kopier

dotnet build MyProject.csproj --no-restore --configuration Release

Let's break this task down a bit more:
The DotNetCoreCLI@2 task maps to the dotnet command.
displayName defines the task name that's shown in the user interface. You'll see this in action soon.
inputs defines arguments that are passed to the command. 
command specifies to run the dotnet build subcommand.
arguments specifies additional arguments to pass to the command.
projects specifies which projects to build. This example uses the wildcard pattern **/*.csproj. Both ** and *.csproj are examples of what are called glob patterns. The ** part specifies to search the current directory and all child directories. The *.csproj part specifies any .csproj file. Wildcards let you act on multiple files without specifying each one. If you need to act on a specific file only, you can specify that file instead of using wildcards.
The "@" in the task name, for example DotNetCoreCLI@2, refers to the task's version. As new task versions become available, you can gradually migrate to the latest version to take advantage of new features.

https://azuredevopsdemogenerator.azurewebsites.net/environment/createproject