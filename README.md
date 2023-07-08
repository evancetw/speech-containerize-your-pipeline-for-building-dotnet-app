# speech-containerize-your-pipeline-for-building-dotnet-app



實際案例：monorepo 下，如何加速建置?


## Commands

```powershell
cd src\Evance.Speech.Demo230708

docker build -t demo.webapi -f .\Evance.Speech.Demo230708.WebAPI\Dockerfile .

docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"


```

