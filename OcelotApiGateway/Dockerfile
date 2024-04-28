FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

COPY ["OcelotApiGateway/OcelotApiGateway.csproj", "OcelotApiGateway/"]
RUN dotnet restore "OcelotApiGateway/OcelotApiGateway.csproj"
COPY . .
WORKDIR "/src/OcelotApiGateway"
RUN dotnet build "OcelotApiGateway.csproj" -c Release -o /app/build


FROM build AS publish
RUN dotnet publish "OcelotApiGateway.csproj" -c Release -o /app/publish /p:UseAppHost=false


FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OcelotApiGateway.dll"]
