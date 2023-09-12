#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Kiwi.Web/Kiwi.Web.csproj", "Kiwi.Web/"]
RUN dotnet restore "Kiwi.Web/Kiwi.Web.csproj"
COPY . .
WORKDIR "/src/Kiwi.Web"
RUN dotnet build "Kiwi.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Kiwi.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Kiwi.Web.dll"]