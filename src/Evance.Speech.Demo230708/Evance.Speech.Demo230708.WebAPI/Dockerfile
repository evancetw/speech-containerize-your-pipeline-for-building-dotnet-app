#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
# * 初始化新的 build stage，以 mcr.microsoft.com/dotnet/aspnet:7.0 為基底 image
#   * 這個 image 只有 .NET Runtime，可以用來執行編譯好的 .NET Application
# * 並且將這個 build stage 命名為 base
WORKDIR /app
# 在 container 內建立目錄 /app，並且在隨後的 container 設定工作目錄為 /app
# Q: 為什麼不 mkdir /app && cd /app，而是用 WORKDIR 指令？
EXPOSE 80
# 「宣告」container 啟動後，服務會開放 port 80
EXPOSE 443
# 「宣告」container 啟動後，服務會開放 port 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
# * 初始化新的 build stage，以 mcr.microsoft.com/dotnet/sdk:7.0 為基底 image
#   * 這個 image 有 .NET SDK，可以將你的 C# 原始碼編譯成 .NET Application
# * 並且將這個 build stage 命名為 build
WORKDIR /src
# 在 container 內建立目錄 /src，並且在隨後的 container 設定工作目錄為 /app
COPY ["Evance.Speech.Demo230708.WebAPI/Evance.Speech.Demo230708.WebAPI.csproj", "Evance.Speech.Demo230708.WebAPI/"]
# 從「build context」複製 .csproj 檔案到 container 內的 /src/Evance.Speech.Demo230708.WebAPI/ 目錄
RUN dotnet restore "Evance.Speech.Demo230708.WebAPI/Evance.Speech.Demo230708.WebAPI.csproj"
# 使用 dotnet restore 指令，還原 .csproj 檔案所宣告的套件
COPY . .
# 將「build context」的所有檔案複製到 container 內的 /src/ 目錄
WORKDIR "/src/Evance.Speech.Demo230708.WebAPI"
# 設定 container 的工作目錄為 /src/Evance.Speech.Demo230708.WebAPI/
RUN dotnet build "Evance.Speech.Demo230708.WebAPI.csproj" -c Release -o /app/build
# 使用 dotnet build 指令，編譯 .csproj，並且輸出到 /app/build 目錄

FROM build AS publish
# * 初始化新的 build stage，以 "build" 這個 build stage 為基底 image
# * 並且將這個 build stage 命名為 publish
RUN dotnet publish "Evance.Speech.Demo230708.WebAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false
# 使用 dotnet publish 指令，編譯 .csproj，並且輸出到 /app/publish 目錄

FROM base AS final
# * 初始化新的 build stage，以 "base" 這個 build stage 為基底 image
# * 並且將這個 build stage 命名為 final
WORKDIR /app
# 在 container 內建立目錄 /app，並且在隨後的 container 設定工作目錄為 /app
COPY --from=publish /app/publish .
# 從 "publish" 這個 build stage 複製 /app/publish 目錄到 container 內的 /app/publish 目錄
ENTRYPOINT ["dotnet", "Evance.Speech.Demo230708.WebAPI.dll"]
# * 設定 container 啟動後要執行的指令
# * 等同於在 terminal 輸入 dotnet Evance.Speech.Demo230708.WebAPI.dll


