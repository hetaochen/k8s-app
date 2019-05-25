FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["k8s-app/k8s-app.csproj", "k8s-app/"]
RUN dotnet restore "k8s-app/k8s-app.csproj"
COPY . .
WORKDIR "/src/k8s-app"
RUN dotnet build "k8s-app.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "k8s-app.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "k8s-app.dll"]