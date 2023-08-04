#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["CICD/CICD.csproj", "CICD/"]
RUN dotnet restore "CICD/CICD.csproj"
COPY . .
WORKDIR "/src/CICD"
RUN dotnet build "CICD.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CICD.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CICD.dll"]