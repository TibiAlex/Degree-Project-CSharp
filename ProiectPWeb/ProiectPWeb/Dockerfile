#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.


# docker build -f ProiectPWeb\ProiectPWeb\Dockerfile -t csharp  ProiectPWeb/
# docker run -dt -p 5223:80  --name csharp tibialex2000/csharp:latest


FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

ENV DOTNET_URLS=http://+:80
ENV ASPNETCORE_URLS=http://+:80
ENV ASPNETCORE_ENVIRONMENT=Development
ENV ASPNETCORE_LOGGING__CONSOLE__DISABLECOLORS=true
ENV DOTNET_USE_POLLING_FILE_WATCHER=1
ENV NUGET_PACKAGES=/root/.nuget/fallbackpackages
ENV NUGET_FALLBACK_PACKAGES=/root/.nuget/fallbackpackages

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["ProiectPWeb/ProiectPWeb.csproj", "ProiectPWeb/"]
RUN dotnet restore "ProiectPWeb/ProiectPWeb.csproj"
COPY . .
WORKDIR "/src/ProiectPWeb"
RUN dotnet build "ProiectPWeb.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ProiectPWeb.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ProiectPWeb.dll"]