# speech-containerize-your-pipeline-for-building-dotnet-app

實際案例：monorepo 下，如何加速建置?

## 介紹 WebAPI Dockerfile

```powershell
cd src\Evance.Speech.Demo230708

docker build -t demo.webapi -f .\Evance.Speech.Demo230708.WebAPI\Dockerfile .

docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

### 從 Docker Image 取得二進位檔

```powershell
docker build --output=bin_webapi --target=binaries -f .\Evance.Speech.Demo230708.WebAPI\Dockerfile .
```

## 介紹 Worker Dockerfile

```powershell
cd src\Evance.Speech.Demo230708

docker build -t demo.worker -f .\Evance.Speech.Demo230708.WorkerService\Dockerfile .

docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

### 從 Docker Image 取得二進位檔

```powershell
docker build --output=bin_worker --target=binaries -f .\Evance.Speech.Demo230708.WorkerService\Dockerfile .
```

## Mono

```powershell
cd src\Evance.Speech.Demo230708

docker build --tag=demo.mono.webapi --target=webapi_final .
docker build --tag=demo.mono.worker --target=worker_final .

docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

