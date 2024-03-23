FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["AWSCodePipelineDemo/AWSCodePipelineDemo.csproj", "AWSCodePipelineDemo/"]
RUN dotnet restore "AWSCodePipelineDemo/AWSCodePipelineDemo.csproj"
COPY . .
WORKDIR "/src/AWSCodePipelineDemo"
RUN dotnet build "AWSCodePipelineDemo.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "AWSCodePipelineDemo.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AWSCodePipelineDemo.dll"]
