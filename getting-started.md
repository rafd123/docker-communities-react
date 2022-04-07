# Getting Started

## Deploying an Off-the-Shelf Image

For our first app, let's deploy a simple [Nginx](https://hub.docker.com/_/nginx) container.

1. We're going to create a simple Docker Compose file that will define a single container app. We'll map port 80 of the host to port 80 of the container.

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

1. Now, let's actually run it! We can use `docker compose up` to start the container!

    ```bash-exec
    docker compose -f ${FILE_DROP_HOST_LOCATION}/docker-compose.yml up -d
    ```

    To see it working, simply open http://localhost!

## Doing Something Custom

Now that we've run an off-the-shelf image, let's build our own custom image! We'll build on our nginx example and plug in our own website.

1. Let's start off by creating a simple `index.html` file. We'll put this in the container shortly.

    ```html
    ---
    filename: index.html
    ---
    <!DOCTYPE html>
    <html>
    <body>
      <div style="text-align: center; margin: 2em auto;">
        <h2>Hooray!</h2>
        <p><img src="https://media.giphy.com/media/11sBLVxNs7v6WA/giphy.gif" /></p>
      </div>
    </body>
    </html>
    ```

1. Now, let's define a `Dockerfile` to help us create our custom image! This Dockerfile defines:

    - `FROM nginx:alpine` - indicates we want to build on top of the `nginx:alpine` image
    - `COPY index.html /usr/share/nginx/html` - indicates we want to place the `index.html` file into the container image at the path `/usr/share/nginx/html`

    ```dockerfile
    ---
    filename: Dockerfile
    ---
    FROM nginx:alpine
    COPY index.html /usr/share/nginx/html
    ```

1. Now, let's tweak our `docker-compose.yml` file to build and use our custom image, rather than the off-the-shelf image. Adjust the compose file to look like the following:

    ```yaml
    ---
    filename: docker-compose.yml
    ---
    services:
      app:
        build: ./
        ports:
          - "80:80"
    ```

    The `build: ./` tells Compose to build an image using the `Dockerfile` in the current directory and use the built image for the service.

1. And let's redeploy our app! We'll use our same compose command from earlier, but add the `--build` flag. This will ensure we rebuild the containers.

    The neat thing is that Compose will figure out what changed since the last time we ran and reconcile the difference!

    ```bash-exec
    docker compose -f ${FILE_DROP_HOST_LOCATION}/docker-compose.yml up -d --build
    ```

1. Then open to http://localhost!

## Tearing it all down

When we're done, we can simply run `docker compose down` and it's as if nothing ever happened!

```bash-exec
docker compose -f ${FILE_DROP_HOST_LOCATION}/docker-compose.yml down
```

