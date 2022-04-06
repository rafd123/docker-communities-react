# Getting Started

Something can go here!

```yaml
---
filename: docker-compose.yml
---
services:
  app:
    image: nginx:alpine
    ports:
      - "80:80"
```

Now, let's actually run it!

```bash-exec
docker compose -f ${FILE_DROP_HOST_LOCATION}/docker-compose.yml up -d
```

Then open to http://localhost!


```bash-exec
docker compose -f ${FILE_DROP_HOST_LOCATION}/docker-compose.yml down
```
