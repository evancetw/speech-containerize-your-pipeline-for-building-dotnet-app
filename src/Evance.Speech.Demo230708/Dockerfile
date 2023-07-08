
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS common_build
WORKDIR /src
COPY . .
RUN dotnet restore "Evance.Speech.Demo230708.sln"

###

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS webapi_base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM common_build AS webapi_publish
RUN dotnet publish "Evance.Speech.Demo230708.WebAPI/Evance.Speech.Demo230708.WebAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM webapi_base AS webapi_final
WORKDIR /app
COPY --from=webapi_publish /app/publish .
ENTRYPOINT ["dotnet", "Evance.Speech.Demo230708.WorkerService.dll"]

FROM scratch AS webapi_binaries
COPY --from=webapi_base /app/publish .

###

FROM mcr.microsoft.com/dotnet/runtime:7.0 AS worker_base
WORKDIR /app

FROM common_build AS worker_publish
RUN dotnet publish "Evance.Speech.Demo230708.WorkerService/Evance.Speech.Demo230708.WorkerService.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM worker_base AS worker_final
WORKDIR /app
COPY --from=worker_publish /app/publish .
ENTRYPOINT ["dotnet", "Evance.Speech.Demo230708.WorkerService.dll"]

FROM scratch AS worker_binaries
COPY --from=worker_publish /app/publish .
