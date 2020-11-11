#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Services/GrillFreshInc.Services.Identity.API/GrillFreshInc.Services.Identity.API.csproj", "Services/GrillFreshInc.Services.Identity.API/"]
RUN dotnet restore "Services/GrillFreshInc.Services.Identity.API/GrillFreshInc.Services.Identity.API.csproj"
COPY . .
WORKDIR "/src/Services/GrillFreshInc.Services.Identity.API"
RUN dotnet build "GrillFreshInc.Services.Identity.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GrillFreshInc.Services.Identity.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GrillFreshInc.Services.Identity.API.dll"]