﻿FROM mcr.microsoft.com/dotnet/aspnet:9.0-preview AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0-preview AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BlazorApp2/BlazorApp2.csproj", "BlazorApp2/"]
COPY ["BlazorApp2.Client/BlazorApp2.Client.csproj", "BlazorApp2.Client/"]
RUN dotnet restore "BlazorApp2/BlazorApp2.csproj"
COPY . .
WORKDIR "/src/BlazorApp2"
RUN dotnet build "BlazorApp2.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "BlazorApp2.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorApp2.dll"]
