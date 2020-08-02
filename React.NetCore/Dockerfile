FROM mcr.microsoft.com/dotnet/core/aspnet:2.1.20-stretch-slim AS base

#RUN apt-get update -yq \
#    && apt-get install curl gnupg -yq \
#    && curl -sL https://deb.nodesource.com/setup_10.x | bash \
#    && apt-get install nodejs -yq

WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1 AS  build

RUN apt-get update -yq \
    && apt-get install curl gnupg -yq \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash \
    && apt-get install nodejs -yq


WORKDIR /src
COPY ["App/App.csproj", "App/"]
RUN dotnet restore "App/App.csproj"
COPY . .
WORKDIR "/src/App"
RUN dotnet build "App.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "App.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

#ENV ASPNETCORE_URLS http://*:7000
ENTRYPOINT ["dotnet", "App.dll"]

