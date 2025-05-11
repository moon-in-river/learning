# 常用命令

- 基于一个镜像，打新的镜像。

```bash
docker tag image_id repository:tag
```

- 基于容器，制作新镜像。

```bash
docker commit container_id/container_name image_name:tag
```
