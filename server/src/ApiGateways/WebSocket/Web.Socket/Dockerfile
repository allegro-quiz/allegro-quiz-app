#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["ApiGateways/WebSocket/Web.Socket/Web.Socket.csproj", "ApiGateways/WebSocket/Web.Socket/"]
COPY ["Services/Quizer/Quiz.Domain/Quizzer.Domain.csproj", "Services/Quizer/Quiz.Domain/"]
RUN dotnet restore "ApiGateways/WebSocket/Web.Socket/Web.Socket.csproj"
COPY . .
WORKDIR "/src/ApiGateways/WebSocket/Web.Socket"
RUN dotnet build "Web.Socket.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Web.Socket.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Web.Socket.dll"]