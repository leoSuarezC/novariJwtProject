#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/aspnetcore:2.0-nanoserver-sac2016 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0-nanoserver-sac2016 AS build
WORKDIR /src
COPY ["src/OpenIdConnectClient/OpenIdConnectClient.csproj", "src/OpenIdConnectClient/"]
RUN dotnet restore "src/OpenIdConnectClient/OpenIdConnectClient.csproj"
COPY . .
WORKDIR "/src/src/OpenIdConnectClient"
RUN dotnet build "OpenIdConnectClient.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "OpenIdConnectClient.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "OpenIdConnectClient.dll"]