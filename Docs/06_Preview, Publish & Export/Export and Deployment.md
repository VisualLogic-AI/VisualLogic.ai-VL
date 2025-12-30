# Export and Deployment

VisualLogic offers a powerful feature for **exporting** your application and **deploying** it to your own servers. This gives you **full control** over your deployment environment, allowing you to host your app independently from the platform. In this guide, we’ll walk you through the **export process** and provide detailed steps on how to deploy your application using **single-server** or **containerized environments** like **Docker**. We will also cover the configuration of **reverse proxies**, **load balancing**, and **SSL termination**.

------

## **Why Export and Deploy?**

Exporting and deploying your VisualLogic application gives you the following benefits:

1. **Full Control Over Infrastructure**: Host your application on your own infrastructure or cloud servers.
2. **Avoid Platform Lock-in**: Self-hosting allows you to detach your app from VisualLogic's cloud environment.
3. **Scalability**: Easily scale your application using **elastic deployment**, containers, and load balancing.
4. **Cost Management**: After export, there are no further server or traffic fees from VisualLogic, though you will need to manage your infrastructure costs.

------

## **Types of Exported Application Packages**

VisualLogic allows you to export your app in the following formats:

### **1. Full-stack Application (Frontend + Backend)**

This package includes both the **frontend** and **backend** code, and is suitable for applications that require both client-side and server-side logic.

- **Core Program** (Base Program) for backend services.
- **Application Code Package**: Contains both frontend and backend code.

### **2. Frontend-Only Application (HTML Package)**

This package includes only the **frontend** code (HTML, CSS, JS) and is ideal for static websites or frontend-only applications.

- Contains **UI files** but no backend services.

------

## **Exporting Your Application for Deployment**

### **Steps to Export Your Application:**

1. **Open the Export Panel**:
   - Navigate to the **File** menu in VisualLogic and select **App Export** under the **Manual Deployment** section.
2. **Choose the Export Format**:
   - Select whether you want to export a **full-stack application** (frontend + backend) or just the **frontend HTML package**.
3. **Download the Export Package**:
   - VisualLogic will generate a **ZIP file** containing the exported code. Download the **ZIP file** to your local machine.
4. **Review the Exported Files**:
   - The exported files will contain either:
     - Full-stack code (backend and frontend).
     - Frontend code (HTML, CSS, JS only).
5. **Prepare the Files for Deployment**:
   - Extract the **ZIP** file and check the contents to ensure everything is ready for deployment.

------

## **Deploying to Your Own Server**

### **Single-Server Deployment**

For a basic setup, you can deploy your full-stack application on a **single server**. This is the simplest deployment method and is suitable for small-scale applications.

#### **Steps for Single-Server Deployment**:

1. **Set Up Your Server**:

   - Use a **Linux-based server** (CentOS is recommended, but Ubuntu and Debian also work).
   - Make sure you have **root or sudo access** for installing necessary software.

2. **Install Required Software**:

   - **MySQL**: Install MySQL (or MariaDB) for the database.
   - **Redis**: Install Redis for caching and session management.
   - **Web Server (Nginx/Apache)**: Install a web server (e.g., **Nginx** or **Apache**) to serve your frontend files.

   Example installation commands:

   ```
   sudo yum install mysql-server redis nginx
   ```

3. **Upload and Extract Exported Files**:

   - Use **FTP/SFTP** or **SSH** to upload the exported **ZIP file** to your server.
   - Extract the **ZIP file** into a directory (e.g., `/var/www/myapp/`).

4. **Set Up the Backend (if applicable)**:

   - For **Node.js**: Run `npm install` to install dependencies.
   - Start the backend server: `node server.js`.

5. **Configure the Web Server (Frontend)**:

   - Configure **Nginx** or **Apache** to serve the frontend files.
      Example **Nginx configuration**:

   ```
   server {
       listen 80;
       server_name www.myapp.com;
       root /var/www/myapp/frontend;
       index index.html;
   
       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```

6. **Start the Application**:

   - For **Node.js**: Run the backend server (e.g., `node server.js`).
   - For **PHP/Python**, ensure the appropriate backend service is running.

------

## **Reverse Proxy and Load Balancing**

In more advanced deployment scenarios, particularly when scaling to multiple servers or ensuring high availability, a **reverse proxy** is used to route requests to different servers or containers. **Nginx** and **Apache** are the most commonly used reverse proxies. Here's how you can configure a **reverse proxy** and **load balancing**.

### **Setting Up Reverse Proxy**:

A **reverse proxy** acts as an intermediary between users and your backend servers. It forwards user requests to the appropriate backend server, providing **load balancing**, **SSL termination**, and **caching**.

#### **Nginx Example**:

1. **Install Nginx**:

   - If you haven't installed Nginx yet, use the following command to install it:

     ```
     sudo yum install nginx
     ```

2. **Configure Nginx for Reverse Proxy**:

   - Edit the Nginx configuration to forward requests to your backend servers:

   ```
   upstream backend {
       server backend1.example.com;
       server backend2.example.com;
   }
   
   server {
       listen 80;
       server_name www.myapp.com;
   
       location / {
           proxy_pass http://backend;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       }
   }
   ```

3. **SSL Termination**:

   - You can configure **SSL termination** on Nginx so that the secure connection (HTTPS) is handled by the reverse proxy, offloading the encryption/decryption process from the backend servers:

   ```
   server {
       listen 443 ssl;
       server_name www.myapp.com;
   
       ssl_certificate /etc/nginx/ssl/myapp.crt;
       ssl_certificate_key /etc/nginx/ssl/myapp.key;
   
       location / {
           proxy_pass http://backend;
           # Other proxy settings...
       }
   }
   ```

4. **Enable Load Balancing**:

   - Nginx will distribute incoming traffic to the available backend servers in the **upstream** block. This is **round-robin** by default, but you can adjust it for other balancing methods.

------

## **Deploying with Docker (Containerized Deployment)**

Docker allows you to deploy VisualLogic applications in **containers**, which are isolated environments that are easy to deploy, scale, and manage.

1. **Install Docker** on your server (if not already installed).

   - For **Ubuntu**:

     ```
     sudo apt-get install docker.io
     ```

2. **Upload Exported Files**:

   - Upload the **base program** and **application code package** to your server.

3. **Build Docker Image**:

   - Create a **Dockerfile** for the backend services, specifying the environment, dependencies, and the application itself.

4. **Run Docker Containers**:

   - Start containers using the `docker run` command, and use a **load balancer** or **Nginx** to distribute requests to different container instances if needed.

------

## **Using a Custom Domain for Deployment**

After deployment, you can set up a **custom domain**:

1. **Update DNS Records**:
   - Configure your **A Records** or **CNAME Records** to point to the server’s IP address.
2. **SSL Configuration**:
   - Set up **SSL** using **Let’s Encrypt** or other providers to ensure **HTTPS** access to your app.
3. **Access Your Application**:
   - Test your app by visiting the custom domain (e.g., `www.myapp.com`).

------

## **Post-Deployment Tasks**

After deployment, you should:

1. **Monitor Application**:
   - Use tools like **Google Analytics**, **New Relic**, or **Datadog** to monitor performance and detect issues.
2. **Backups**:
   - Regularly back up your application files and databases.
3. **Security Updates**:
   - Apply security patches and ensure your server is secure.

------

## **Summary**

Exporting and deploying your VisualLogic application to your own server or containerized environment gives you **complete control** over your infrastructure, security, and scalability. The steps include:

1. **Exporting** your app from VisualLogic.
2. **Deploying** it to a server (single-server or containerized).
3. **Configuring reverse proxies** and **load balancing** for scaling.
4. **Setting up a custom domain** and **SSL** for secure access.

With this setup, you can fully manage your app’s lifecycle, from deployment to maintenance.