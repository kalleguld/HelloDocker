FROM node AS clientbuild
WORKDIR /usr/src/app
COPY HelloDocker/package*.json ./
RUN npm install
WORKDIR /usr/src/app/spa
COPY HelloDocker/spa/package*.json ./
RUN npm install
COPY HelloDocker/spa ./
RUN npm run build


FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY HelloDocker/HelloDocker.csproj .
RUN dotnet restore HelloDocker.csproj
COPY HelloDocker/ .
RUN dotnet build ./HelloDocker.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish HelloDocker.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
WORKDIR /
COPY --from=clientbuild /usr/src/app/spa/dist /spa/dist
ENTRYPOINT ["dotnet", "app/HelloDocker.dll"]