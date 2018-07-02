### This project now no longer fails to build, it is just a POC test project now... Thanks Microsoft! : )


### Create project base folder
mkdir failed-spa-build

cd .\failed-spa-build\

### Get the latest SPA templates
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::*

### Make sure proper older angular CLI version is available globally
npm install -g @angular/cli@1.7.0

### Build a new Angular SPA app from template
dotnet new angular -f netcoreapp2.0 -o failed-spa-build

### Build it
cd .\failed-spa-build\

dotnet build

### Verify it can run in development mode
setx ASPNETCORE_ENVIRONMENT "Development"

dotnet run

### Verify it can publish
dotnet publish -c Release -o ../pub

cd ..

### Create the test Dockerfile build files here...
### These files can be found verbatim in this repository at:
[Dockerfile.2.1-sdk](https://github.com/temporafugiunt/AngularSPATemplateFailure/blob/master/Dockerfile.2.1-sdk)

[Dockerfile.aspnetcore](https://github.com/temporafugiunt/AngularSPATemplateFailure/blob/master/Dockerfile.aspnetcore)

### build the docker image from the microsoft/dotnet:2.1-sdk image
docker build -t failed-spa-build-sdk -f Dockerfile.2.1-sdk .

### build the docker image from the microsoft/aspnetcore-build:2.0.8-2.1.200-nanoserver-sac2016 image
docker build -t failed-spa-build-aspnetcore -f Dockerfile.aspnetcore .

Running either of these commands results in:
```
  > failed_spa_build@0.0.0 build C:\app\failed-spa-build-src\ClientApp
  > ng build --extract-css "--prod"

  module.js:549
      throw err;
      ^

EXEC : error : Cannot find module 'C:\app\failed-spa-build-src\ClientApp\node_modules\@angular\cli\bin\ng' [C:\app\failed-spa-build-src\failed-spa-build.csproj]
      at Function.Module._resolveFilename (module.js:547:15)
      at Function.Module._load (module.js:474:25)
      at Function.Module.runMain (module.js:693:10)
      at startup (bootstrap_node.js:191:16)
      at bootstrap_node.js:612:3
  npm ERR! code ELIFECYCLE
  npm ERR! errno 1
  npm ERR! failed_spa_build@0.0.0 build: `ng build --extract-css "--prod"`
  npm ERR! Exit status 1
  npm ERR!
  npm ERR! Failed at the failed_spa_build@0.0.0 build script.
  npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

  npm ERR! A complete log of this run can be found in:
  npm ERR!     C:\Users\ContainerAdministrator\AppData\Roaming\npm-cache\_logs\2018-06-25T20_23_36_785Z-debug.log
C:\app\failed-spa-build-src\failed-spa-build.csproj(43,5): error MSB3073: The command "npm run build -- --prod" exited with code 1.
The command 'powershell -Command $ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue'; dotnet publish -c Release -o /app/failed-spa-build-pub' return
ed a non-zero code: 1
```
