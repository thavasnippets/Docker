# Docker
Docker is an open source platform for building, deploying, and managing containerized applications.

## How To Dockerize a .NET Core?

### Prerequisites
  - .NET Core SDK
  - Docker Desktop
  - VSCode
    - C# for Visual Studio Code (powered by OmniSharp)
    - ms-azuretools.vscode-docker

### Example
- Create new web api Project and the below command the whole project structure is scaffolded
        ``dotnet new webapi ``
- Lets build and test the app.
- Add Dockerfile in the project level with the snippets     
    ![DockerVSCode](https://user-images.githubusercontent.com/15138302/107313130-ec3e5700-6ab7-11eb-90be-1a6c30e0f35e.JPG)
    
  ```docker
  # Start from the base image
  FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build-env
  WORKDIR /app

  # Copy csproj and restore as distinct layers
  COPY *.csproj ./
  RUN dotnet restore

  # Copy everything else and build
  COPY . ./
  RUN dotnet publish -c Release -o out

  # Build runtime image
  FROM mcr.microsoft.com/dotnet/sdk:5.0
  WORKDIR /app
  COPY --from=build-env /app/out .
  ENTRYPOINT ["dotnet", "WebApiDocker.dll"]
  ```
- Build the app with the Docker Image
``` docker build -t thavaselvanpoc/dockerwebapi .```
- Once the Docker image is built. You can run the image with the following command
```docker run -p 8080:80 thavaselvanpoc/dockerwebapi ```
- List running containers
```docker ps```
- Push the Docker image to the docker hub
```docker push thavaselvanpoc/dockerwebapi ```

### Deployment in Linux box
- Connect to  linux box and make sure the docker desktop has installed.
- Pull the image from the docker hub using below command
```docker pull thavaselvanpoc/dockerwebapi ```
    ![DockerUbuntu](https://user-images.githubusercontent.com/15138302/107312931-8ce04700-6ab7-11eb-8356-155b971adf2a.JPG)
- You can run the image with the following command
```docker run -p 8080:80 thavaselvanpoc/dockerwebapi ```
    ![DockerLinuxResult](https://user-images.githubusercontent.com/15138302/107313044-c618b700-6ab7-11eb-9589-386d24e2899e.JPG)
