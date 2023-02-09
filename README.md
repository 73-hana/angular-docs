angular-docs

```bash
$ docker run -itd --name angularnode -w /app -v "$(pwd)/app:/app" -p 3000:3000 node

$ docker exec -it angularnode /bin/bash