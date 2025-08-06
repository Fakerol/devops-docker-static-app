# Moso Interior Website Deployment

This guide outlines the steps to deploy a static website using Docker with a sample template.

## Prerequisites
- Docker installed on your system
- Basic knowledge of Linux commands
- Internet connection to download required files

## Deployment Steps

1. **Set Up Environment**
   ```bash
   sudo -i
   sudo apt install unzip -y
   mkdir images
   cd images
   mkdir moso
   ```

2. **Download and Prepare Files**
   ```bash
   wget https://www.example.com/sample-template.zip
   unzip sample-template.zip
   cd sample-template
   tar czvf moso.tar.gz *
   mv moso.tar.gz ../
   cd ..
   rm -rf sample-template sample-template.zip
   mv moso.tar.gz moso/
   cd moso
   ```

3. **Create Dockerfile**
   Create a file named `Dockerfile` with the following content:
   ```dockerfile
   FROM ubuntu:latest
   LABEL Author="John Doe"
   LABEL Project="moso"
   RUN apt update && apt install git -y
   RUN apt install apache2 -y
   CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
   EXPOSE 80
   WORKDIR /var/www/html
   VOLUME /var/log/apache2
   ADD moso.tar.gz /var/www/html
   ```

4. **Build and Run Docker Container**
   ```bash
   docker build -t mosoimg .
   docker images
   docker run -d --name mosowebsite -p 9080:80 mosoimg
   docker ps -a
   ```

5. **Access the Website**
   Open a browser and navigate to `http://example.com:9080`

## Notes
- Ensure port 9080 is open on your firewall.
- Replace `example.com` with your server's actual IP address or domain.
- The template URL and file names have been replaced with dummy data for security.
