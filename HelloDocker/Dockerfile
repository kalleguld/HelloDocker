FROM node AS clientbuild
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
WORKDIR /usr/src/app/spa
COPY ./spa/package*.json ./
RUN npm install
COPY ./spa ./
RUN npm run build


FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["HelloDocker.csproj", "HelloDocker/"]
RUN dotnet restore "HelloDocker/HelloDocker.csproj"
COPY . .
RUN ls -al "./HelloDocker"
RUN dotnet build "./HelloDocker/HelloDocker.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "HelloDocker.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
WORKDIR /
COPY --from=clientbuild /usr/src/app/spa/dist /spa/dist
ENTRYPOINT ["dotnet", "HelloDocker.dll"]