﻿FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["TataneMaps.Utils/TataneMaps.Utils.csproj", "TataneMaps.Utils/"]
COPY ["TataneMaps.Application/TataneMaps.Application.csproj", "TataneMaps.Application/"]
COPY ["TataneMaps.Domain/TataneMaps.Domain.csproj", "TataneMaps.Domain/"]
COPY ["TataneMaps.Persistence/TataneMaps.Persistence.csproj", "TataneMaps.Persistence/"]
COPY ["TataneMaps.Infrastructure/TataneMaps.Infrastructure.csproj", "TataneMaps.Infrastructure/"]
COPY ["TataneMaps.Controllers/TataneMaps.Controllers.csproj", "TataneMaps.Controllers/"]
RUN dotnet restore "TataneMaps.Controllers.csproj"
COPY . .
WORKDIR "/src/TataneMaps.Controllers"
RUN dotnet build "TataneMaps.Controllers.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "TataneMaps.Controllers.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TataneMaps.Controllers.dll"]