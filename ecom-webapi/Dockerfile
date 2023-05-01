#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["ecom-webapi/ecom-webapi.csproj", "ecom-webapi/"]
RUN dotnet restore "ecom-webapi/ecom-webapi.csproj"
COPY . .
WORKDIR "/src/ecom-webapi"
RUN dotnet build "ecom-webapi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ecom-webapi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ecom-webapi.dll"]