# STEP:1 Ubuntu base image
FROM ubuntu:latest

# STEP: 2 Install Nginx
RUN apt-get update && apt-get install -y -q nginx

# STEP:3 Copy file
COPY index.html /usr/share/nginx/html/

# STEMP:4 Run ngnix
CMD ["nginx", "-g", "daemon off;"]
