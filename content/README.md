# content-image


## content-image

Simple docker image to holds the content

```shell
docker build -t content-image .
```

```shell
docker run -it --rm -v /tmp/test/:/dest/ content-image rsync -avh /content/ /dest/
```
