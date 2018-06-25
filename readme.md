# Create project base folder
mkdir failed-spa-build
cd .\failed-spa-build\

# Get the latest SPA templates
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::*

# Make sure proper older angular CLI version is available globally
npm install -g @angular/cli@1.7.0

# Build a new Angular SPA app from template
dotnet new angular -f netcoreapp2.0 -o failed-spa-build

# Build it
cd .\failed-spa-build\
dotnet build

# Verify it can run in development mode
setx ASPNETCORE_ENVIRONMENT "Development"
dotnet run

# Verify it can publish
dotnet publish -c Release -o ../pub

cd ..

# Create the test Dockerfile build files here...
# These files can be found verbatim in this repository at:
# Dockerfile.2.1-sdk
# Dockerfile.aspnetcore

# build the docker image from the microsoft/dotnet:2.1-sdk image
docker build -t failed-spa-build-sdk -f Dockerfile.2.1-sdk .

#build the docker image from the microsoft/aspnetcore-build:2.0.8-2.1.200-nanoserver-sac2016 image
docker build -t failed-spa-build-aspnetcore -f Dockerfile.aspnetcore .