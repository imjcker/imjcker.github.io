---
layout: post
title: MongoDB
category: NoDB
tags: mongoDB
---

# 2020-01-01-mongodb

MongoDB 搭建使用记录。

```text
docker run -d --name mongo-server -p 27017:27017 \
-v /Users/alanturing/mongo/configdb:/data/configdb -v /Users/alanturing/mongo/db:/data/db \
-e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=mongo \
mongo:latest
```

进入mongo容器

```text
docker exec -it mongo-server bash
```

