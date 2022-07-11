#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["SS14/nuget.config", "SS14/"]
COPY ["SS14.Admin/SS14.Admin.csproj", "SS14.Admin/"]
COPY ["SS14/Content.Server.Database/Content.Server.Database.csproj", "SS14/Content.Server.Database/"]
COPY ["SS14/Content.Shared.Database/Content.Shared.Database.csproj", "SS14/Content.Shared.Database/"]
RUN dotnet restore "SS14.Admin/SS14.Admin.csproj"
COPY . .
WORKDIR "/src/SS14.Admin"
RUN dotnet build "SS14.Admin.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SS14.Admin.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SS14.Admin.dll"]