# aws-deploy
This repository contains step-by-step guide &amp; resources for deploying a full-stack application from scratch in AWS

## Deploying Application

### Deploying Django on AWS EC2
- Resources: [Deploy Django in AWS Ubuntu 20.04 using Gunicorn and Nginx.](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-20-04)

- Youtube: [Deploy Django on AWS | Django Deployment NGINX GUNICORN | Django Deployment by Code Keen](https://youtu.be/dBTIAfi7y5U?si=DRAWEJNhZqsH-Uic)

### Deploying React App on AWS EC2
- Resources: [How To Deploy a React Application with Nginx on Ubuntu 20.04 Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-react-application-with-nginx-on-ubuntu-20-04) or [Deploy Your React App with AWS EC2](https://aws.plainenglish.io/deploy-your-react-app-with-aws-ec2-f355f5700aae)

- Youtube: [Deploying a React Application with Nginx on Ubuntu](https://youtu.be/WKfmhgYQlCM?si=po5HjoOEKE8cQIpo)

Deploying a React application to an AWS EC2 instance involves several steps. Here's a high-level overview of the process:

1. **Set Up an AWS EC2 Instance:**
   - Sign in to your AWS Management Console.
   - Navigate to the EC2 service and launch an EC2 instance. Make sure to choose an appropriate instance type and configure security groups to allow incoming traffic on the required ports (e.g., port 80 for HTTP).

2. **SSH Access:**
   - Once your EC2 instance is up and running, you'll need SSH access to it. Use the private key associated with your instance to connect.

   ```bash
   ssh -i your-private-key.pem ec2-user@your-instance-ip
   ```

3. **Update the System:**
   - After connecting to your instance, update the system packages:

   ```bash
   sudo yum update -y
   ```

4. **Install Node.js and NPM:**
   - Install Node.js and npm to run your React application:

   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
   source ~/.nvm/nvm.sh
   nvm install node
   ```

5. **Deploy Your React Application:**
   - There are various ways to deploy a React app, but one common method is to use a version control system like Git. Clone your React app repository to your EC2 instance:

   ```bash
   git clone your-repo-url
   ```

   - Navigate to your React app directory and install the dependencies:

   ```bash
   cd your-app-directory
   npm install
   ```

   - Build your React app for production:

   ```bash
   npm run build
   ```

6. **Set Up a Web Server:**
   - You can use a web server like Nginx or Apache to serve your React app. Install and configure Nginx:

   ```bash
   sudo yum install nginx -y
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

   - Create an Nginx server block (virtual host) configuration for your React app. This usually involves editing the `/etc/nginx/nginx.conf` file or creating a new configuration file in `/etc/nginx/conf.d/`.

   Example Nginx config:

   ```nginx
   server {
       listen 80;
       server_name your-domain.com;
       root /path/to/your/react/app/build;
       index index.html;

       location / {
           try_files $uri /index.html;
       }
   }
   ```

   - Test the Nginx configuration and restart Nginx:

   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

7. **Configure DNS:**
   - If you have a custom domain, configure the DNS settings to point to your EC2 instance's public IP address.

8. **Secure Your Application:**
   - Consider setting up SSL/TLS using a service like AWS Certificate Manager or Let's Encrypt for secure HTTPS access to your application.

9. **Monitoring and Maintenance:**
   - Set up monitoring, backups, and automated deployment processes as needed for your production environment.

10. **Access Your React App:**
   - Visit your domain or public IP address in a web browser to access your deployed React application.

Remember that this is a simplified overview, and the exact steps may vary depending on your specific requirements and the AWS region you are using. Always follow best practices for security and performance when deploying to production environments.

## Deploying Node.js Express Application on AWS EC2

- Youtube: [How to Deploy NodeJS App on AWS EC2 instance | Run App as Background Service Using PM2 | Full Demo](https://youtu.be/KVmKM6MxvcA?si=JJxCJomtsFdVcsG5) or []()
Deploying an Express.js Node.js application to a production AWS EC2 instance involves several steps. Here's a general guide on how to do it:

1. **Set Up an AWS EC2 Instance:**
   - Launch an EC2 instance on AWS with the desired specifications (e.g., choose an appropriate instance type and configure security groups).
   - Ensure that the instance has an appropriate IAM role or permissions to access other AWS services if your application requires it.

2. **SSH Access:**
   - Use SSH to connect to your EC2 instance. You will need the private key associated with your instance:

   ```bash
   ssh -i your-private-key.pem ec2-user@your-instance-ip
   ```

3. **Update the System:**
   - After connecting to your instance, update the system packages:

   ```bash
   sudo yum update -y
   ```

4. **Install Node.js and NPM:**
   - Install Node.js and npm to run your Express.js application:

   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
   source ~/.nvm/nvm.sh
   nvm install node
   ```

5. **Deploy Your Express.js Application:**
   - Transfer your Express.js application code to the EC2 instance. You can use tools like `scp`, `rsync`, or Git to do this.

   ```bash
   scp -i your-private-key.pem -r /path/to/your/app ec2-user@your-instance-ip:/path/to/destination
   ```

6. **Install Application Dependencies:**
   - Navigate to your application directory on the EC2 instance and install the dependencies:

   ```bash
   cd /path/to/your/app
   npm install --production
   ```

7. **Set Up a Process Manager:**
   - Use a process manager like `pm2` to keep your Node.js application running in the background and automatically restart it if it crashes:

   ```bash
   npm install pm2 -g
   pm2 start app.js
   ```

   - You can configure `pm2` to start your application at system boot by running:

   ```bash
   pm2 startup
   ```

   - Save the current `pm2` processes:

   ```bash
   pm2 save
   ```

8. **Set Up Nginx as a Reverse Proxy (Optional):**
   - To route HTTP requests to your Express.js application, you can use Nginx as a reverse proxy. Install Nginx (if not already installed):

   ```bash
   sudo yum install nginx -y
   ```

   - Create an Nginx server block configuration to proxy requests to your Node.js application. For example:

   ```nginx
   server {
       listen 80;
       server_name your-domain.com;

       location / {
           proxy_pass http://localhost:3000; # Assuming your Express app runs on port 3000
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

   - Test the Nginx configuration and restart Nginx:

   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

9. **Configure DNS:**
   - If you have a custom domain, configure the DNS settings to point to your EC2 instance's public IP address.

10. **Security and SSL/TLS (Optional):**
    - Consider securing your application by setting up SSL/TLS using a service like AWS Certificate Manager or Let's Encrypt for HTTPS access.

11. **Monitoring and Maintenance:**
    - Set up monitoring, backups, and automated deployment processes as needed for your production environment.

12. **Access Your Express.js Application:**
    - Visit your domain or public IP address in a web browser to access your deployed Express.js application.

This is a general guide, and specific configurations may vary based on your application's requirements and AWS region. Ensure you follow best practices for security, performance, and scalability when deploying to a production environment.

## Deploying Django Application on Elastic Beanstalk

- Youtube: [Deploy Django App Using Elastic Beanstalk by Mirza](https://youtu.be/ypnEf7W8db0?si=UzDMn8ev1IYi_kw1)

## Configuring Nginx as Reverse Proxy

- Youtube: [How to configure Nginx Reverse Proxy Servers Tutorial](https://youtu.be/7jNhZrtckhA?si=5K6Skp5FXEEqML8p)


