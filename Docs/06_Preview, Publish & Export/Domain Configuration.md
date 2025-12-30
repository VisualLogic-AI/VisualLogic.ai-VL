# Domain Configuration

**Domain Configuration** is a key step in making your VisualLogic application accessible on the web. It involves linking your app to a **custom domain** or using the **default domain** provided by VisualLogic. In this guide, we will walk through everything you need to know about setting up and configuring your custom domain for publishing your app, ensuring it has a professional and personalized web address.

------

## **What is a Domain Name?**

A **domain name** is the unique address that people type into their web browser to visit a website. It represents the **identity** of your app on the internet. The domain name is what users will type into their browser to access your application.

- **Example**: `www.myapp.com` is a domain name that points to the homepage of your app.

When you publish your application, you need to set up a domain name to allow users to visit your app. You can use **VisualLogic's default domain** or configure your own **custom domain**.

### **What is a Path?**

A **path** is the part of a URL that comes after the domain name and directs users to a specific page or section of your website. The path determines where users will land when they visit a specific URL.

- **Example**: In the URL `www.myapp.com/products`, `products` is the **path** to the product listing page of your application.

Paths are essential for directing users to various parts of your application, such as the homepage, user profiles, or product pages. Paths are set up within the application itself.

------

## **Why Use a Custom Domain?**

Using a **custom domain** offers several advantages over using the **default public domain**:

1. **Professional Branding**: A custom domain makes your app appear more professional. For example, `www.myapp.com` looks far more branded and trustworthy than a subdomain like `myapp.visuallogic.com`.
2. **Complete Control**: A custom domain gives you **full control** over your website’s address, DNS settings, and SSL configuration. You can also move your app between hosting providers or platforms if needed without worrying about platform lock-in.
3. **SEO Benefits**: Custom domains are essential for **Search Engine Optimization (SEO)**. Search engines rank branded domains higher, helping your app get better visibility.
4. **SSL Encryption**: Custom domains ensure that your app uses **HTTPS** by enabling SSL certificates, which protect data integrity and security during transmission.

------

## **How to Configure a Custom Domain**

If you prefer to use your **own custom domain**, follow these steps to configure it for your VisualLogic application.

### **Steps to Configure Your Custom Domain**:

### 1. **Purchase a Domain Name**

You first need to **purchase a domain** from a domain registrar. Some popular registrars include:

- **GoDaddy**
- **Namecheap**
- **Google Domains**

Once you have purchased your domain, you are ready to begin the configuration process.

### 2. **Access the Domain Configuration Panel in VisualLogic**

1. In **VisualLogic**, navigate to the **Publish Panel**.
2. Select the **Domain Configuration** option to open the domain settings.

### 3. **Configure DNS Settings**

To link your custom domain to your VisualLogic app, you will need to update your **DNS records**. DNS settings define where to direct users when they visit your custom domain.

#### **Types of DNS Records to Configure**:

- **A Record**:
   The **A record** links your custom domain (e.g., `www.myapp.com`) directly to an IP address. This is typically used for root domains.
  - **Example**:
    - Host: `@`
    - Value: `123.456.78.90` (VisualLogic server IP address)
    - TTL: `3600` (1 hour)
- **CNAME Record**:
   The **CNAME record** is used to point a subdomain (e.g., `www.myapp.com`) to another domain or subdomain (e.g., `yourapp.visuallogic.com`). This is typically used for subdomains like `www` or `blog`.
  - **Example**:
    - Host: `www`
    - Value: `yourapp.visuallogic.com`
    - TTL: `3600` (1 hour)

#### **DNS Configuration Example**:

- **A Record**:
  - Host: `@`
  - Value: `123.456.78.90` (VisualLogic's IP)
  - TTL: `3600` (1 hour)
- **CNAME Record**:
  - Host: `www`
  - Value: `yourapp.visuallogic.com`
  - TTL: `3600` (1 hour)

### 4. **Verify Domain Configuration**

Once you’ve updated your DNS records, you need to verify that your domain is linked correctly. Open your web browser and enter your custom domain (e.g., `www.myapp.com`). If the application loads correctly, the configuration was successful.

### 5. **SSL Configuration (HTTPS)**

**SSL (Secure Sockets Layer)** ensures that the connection between your users and your app is encrypted, protecting sensitive data like passwords, credit card information, and personal data.

- VisualLogic automatically configures **SSL certificates** for your custom domain once it’s linked to the platform.
- Your app will be accessible via **HTTPS**, indicating a secure connection.

You don’t need to manually configure SSL; VisualLogic handles it for you.

------

## **Preview and Publish Using Custom Domain**

Once you configure your custom domain, you can use it in both **Preview Mode** and **Publish Mode**:

1. **Preview Mode**:

   - You can test your app with your **custom domain** before publishing it. This allows you to ensure that everything functions correctly under the final URL.

   When you enter **Preview Mode**, VisualLogic will use your custom domain (e.g., `www.myapp.com`) so you can see how the app performs with the real URL.

2. **Publish Mode**:

   - Once you are satisfied with your app in **Preview Mode**, you can publish the app to **production** and use your custom domain for the live version.

------

## **Post-Domain Configuration**

Once your custom domain is linked and published, there are a few steps to ensure everything is functioning properly:

1. **Test the Live Version**:
   - Access your app using the custom domain (e.g., `www.myapp.com`) and ensure everything is working as expected.
2. **Monitor DNS Propagation**:
   - DNS changes can take up to **48 hours** to propagate globally. If your custom domain isn’t resolving immediately, give it some time to update.
3. **Verify SSL Activation**:
   - Confirm that SSL is active for your custom domain, ensuring the app is served over **HTTPS**.

------

## **Summary**

Configuring a **custom domain** is an essential step to give your VisualLogic application a professional, personalized web address. With VisualLogic, you can:

1. **Purchase and configure** your custom domain.
2. **Update DNS records** to link your domain to VisualLogic.
3. **Activate SSL** for a secure connection to your app.

Once configured, your application will be accessible via your custom domain, both in **Preview Mode** and **Publish Mode**.

------

## **Next Steps**

- After successfully configuring your custom domain, proceed with **publishing** your application. You can find detailed instructions on **publishing** in the **Web Publishing** documentation.
- If you want to **self-host** your application, refer to the **Export and Deployment** documentation to export your app and host it on your own servers.