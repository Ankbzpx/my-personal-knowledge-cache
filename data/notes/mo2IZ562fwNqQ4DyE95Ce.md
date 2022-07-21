
## Build

Build over localHost proxy
```
docker build -t img_name:tag --network=host --build-arg http_proxy=http://127.0.0.1:7890 .
```

## Run
```
docker run --env HTTP_PROXY="http://127.0.0.1:7890" --gpus all nvidia/cuda:10.2-base nvidia-smi
```
