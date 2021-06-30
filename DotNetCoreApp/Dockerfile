#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/runtime:2.1-alpine AS base
WORKDIR /app

EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-alpine AS build
WORKDIR /src

COPY ["DotNetCoreApp.csproj", "DotNetCoreApp/"]
RUN dotnet restore "DotNetCoreApp/DotNetCoreApp.csproj"
COPY . .
WORKDIR "/src/DotNetCoreApp"
RUN dotnet build "DotNetCoreApp/DotNetCoreApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DotNetCoreApp/DotNetCoreApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DotNetCoreApp.dll"]
