#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:7.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Evance.Speech.Demo230708.WorkerService/Evance.Speech.Demo230708.WorkerService.csproj", "Evance.Speech.Demo230708.WorkerService/"]
RUN dotnet restore "Evance.Speech.Demo230708.WorkerService/Evance.Speech.Demo230708.WorkerService.csproj"
COPY . .
WORKDIR "/src/Evance.Speech.Demo230708.WorkerService"
RUN dotnet build "Evance.Speech.Demo230708.WorkerService.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Evance.Speech.Demo230708.WorkerService.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Evance.Speech.Demo230708.WorkerService.dll"]