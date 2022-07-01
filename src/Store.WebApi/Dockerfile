#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Store.WebApi/Store.WebApi.csproj", "src/Store.WebApi/"]
COPY ["src/Store.Infrastructure/Store.Infrastructure.csproj", "src/Store.Infrastructure/"]
COPY ["src/Store.ApplicationCore/Store.ApplicationCore.csproj", "src/Store.ApplicationCore/"]
RUN dotnet restore "src/Store.WebApi/Store.WebApi.csproj"
COPY . .
WORKDIR "/src/src/Store.WebApi"
RUN dotnet build "Store.WebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Store.WebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
#COPY ./publish/devops-dotnet /app
ENTRYPOINT ["dotnet", "Store.WebApi.dll"]