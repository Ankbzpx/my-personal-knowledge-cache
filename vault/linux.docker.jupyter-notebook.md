---
id: l4gHGuoGRxCiM7tTmr79E
title: Jupyter Notebook
desc: ''
updated: 1643096584122
created: 1643027322589
---
Run docker container with port forwarding and start jupyter server
```
docker run -it --gpus all --env HTTP_PROXY="http://127.0.0.1:7890" --network host image:version
jupyter notebook --ip 0.0.0.0 --no-browser --allow-root
```
## Web browser
Open link shown in terminal in web browser

## VSCode
> Jupyer notebook needs to be in the same folder as in VSCode remote container

1. Copy link, open vscode
2. `Ctrl+Shift+P` choose `Remote-Container:Attach to runningcontainer`, select `image:version`
3. `Ctrl+Shift+P` choose `Jupyter: Specify local or remote Jupyter server for connections`
4. choose `Existing` then paste link
5. open the notebook