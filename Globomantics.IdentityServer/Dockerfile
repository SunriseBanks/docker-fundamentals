#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
COPY nginx/id-local.crt /usr/local/share/ca-certificates/id-local.crt
RUN update-ca-certificates

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Globomantics.IdentityServer/Globomantics.IdentityServer.csproj", "Globomantics.IdentityServer/"]
RUN dotnet restore "Globomantics.IdentityServer/Globomantics.IdentityServer.csproj"
COPY . .
WORKDIR "/src/Globomantics.IdentityServer"
RUN dotnet build "Globomantics.IdentityServer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Globomantics.IdentityServer.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Globomantics.IdentityServer.dll"]
