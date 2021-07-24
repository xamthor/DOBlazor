# Blazor WASM on Digital Ocean App Platform

**Prerequisites**

Install [Digital Ocean CLI](https://docs.digitalocean.com/reference/doctl/how-to/install/)

**Setup** 

Create gitignore file for dotnet app

```
dotnet new gitignore
```

Create Docker file in root of the project  “ Dockerfile.build “

```
FROM mcr.microsoft.com/dotnet/sdk:5.0
WORKDIR /app
COPY appName.csproj appName.csproj.csproj
RUN dotnet restore appName.csproj.csproj
COPY . .
RUN dotnet publish -c Release -o /output —no-restore —nologo
```

Create DO yaml file  “ .do/app.yaml “

```
name: appName
static_sites:
- name: componentName
  github:
    repo: gitUser/gitRepo
    branch: master
    deploy_on_push: true
  dockerfile_path: Dockerfile.build 
  output_dir: /output/wwwroot
  index_document: index.html
  catchall_document: index.html
```

**Deploy** 

In the root folder run the following command

```
doctl app create --spec .do/app.yaml
```

Now you can manage app in the DO app platform online interface 


