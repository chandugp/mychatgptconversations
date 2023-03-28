# Dockerize nginx application.

#### You:
Dockerize a nginx application step by step

#### ChatGPT:
Sure, here are the steps to dockerize an nginx application:

1. Create a Dockerfile

   - Start with a base image of nginx, for example `nginx:latest`.
   - Copy your nginx configuration files to the container using the `COPY` command.
   - Copy your static files to the container using the `COPY` command.
   - Expose the port that nginx listens on using the `EXPOSE` command.

   Here's an example Dockerfile:

   ```
   FROM nginx:latest
   COPY nginx.conf /etc/nginx/nginx.conf
   COPY static/ /usr/share/nginx/html/
   EXPOSE 80
   ```

2. Build the Docker image
   - Navigate to the directory where your Dockerfile is located.
   - Run the `docker build` command to build the image. Give it a name and tag it with a version number, like so: `docker build -t my-nginx-app:v1 .`
   - This command will build the Docker image based on the instructions in the Dockerfile.

3. Run the Docker container
   - Use the `docker run` command to run the container. Here's an example command: `docker run -d -p 8080:80 my-nginx-app:v1`
   - This command will start the container and map port 8080 on your host to port 80 in the container.
   - You can now access your nginx application by visiting <http://localhost:8080> in your web browser.

And that's it! You've successfully dockerized your nginx application.
