# Siteproxy 2.0
<br />
Siteproxy 2.0 utilizes service workers, making the proxy more stable and capable of handling more websites. It replaces Express with Hono to achieve a fourfold speed increase and supports deployment on Cloudflare Workers. It provides a reverse proxy, allowing access to YouTube/Google without needing to bypass internet censorship, and supports GitHub and Telegram web logins. This online proxy operates entirely within web pages, requiring no client configuration, and provides reverse proxy access to the internet.
```
                                                 +----> google/youtube
                             +----------------+  |
                             |                |  |
user browser +-------------->+ siteproxy      +-------> wikipedia
                             |                |  |
                             +----------------+  |
                                                 +----> chinese forums
```
Please do not use this project for illegal purposes, or you will bear the consequences.

## Contents
- [Features](#features)
- [Usage Tips](#usage-tips)
- [Deploying to Cloudflare Worker](#deploying-to-cloudflare-worker)
- [Deploying to VPS or Cloud Server](#deploying-to-vps-or-cloud-server)
- [Contact Information](#contact-information)

### Features
- Replaces Express with Hono, increasing speed by four times.
- Supports deployment on Cloudflare Worker.
- Supports password-controlled access to the proxy; only those with the password can access it.
- Requires no configuration on the client side; accessing the proxy URL grants access to the whole world.
- Supports GitHub and Telegram web logins.
- Entering the URL of the deployed Siteproxy allows access to the entire world while hiding your IP.
- No software installation is required on the client side, nor does the client browser need any configuration.

### Usage Tips
1. You can use the deployed Siteproxy to perform a git clone, for example:
```
git clone https://your-proxy-domain.name/user-your-password/https/github.com/the-repo-to-clone
```

### Deploying to Cloudflare Worker
```
1. Assume your domain is already managed under Cloudflare;
2. Git clone this project and use a text editor to open build/worker.js (you can also download this file directly without cloning)
3. Search for the string http://localhost:5006 and replace it with your proxy website domain, such as https://your-proxy-domain.name. Also, search for user22334455 and change it to a password of your choosing.
4. Create a worker and edit it by copying and pasting the modified worker.js into the worker, then save and deploy.
5. On the Workers & Pages page, open the worker you just saved, click 'Triggers' at the top, then 'Add custom domain', setting it to your proxy domain.
6. Now you can directly access https://your-proxy-domain.name/user-your-password/, replacing the domain and password with your own.
```

### Deploying to VPS or Cloud Server
```
1. Set up an SSL website (using Certbot and Nginx, Google for instructions) and configure Nginx. Your /etc/nginx/sites-enabled/default should include the following:
   ...
   server {
      server_name your-proxy.domain.name
      location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass       http://localhost:5006;
      }
   }
2. Execute: sudo systemctl restart nginx
3. Install Node environment in user space, if you don't already have Node installed:
   (1) curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
   (2) source ~/.bashrc
   (3) nvm install v18
4. Execute: git clone https://github.com/netptop/siteproxy.git;
5. Execute: cd siteproxy;
6. Open and modify the config.json file, saving it:
   {
      "proxy_url": "https://your-proxy.domain.name", // This is your proxy server domain
      "token_prefix": "/user-SetYourPasswordHere/",  // This acts as your site password to prevent unauthorized access. Keep the slashes at the start and end.
      "description": "Note: The token_prefix acts as the site password. Please set it carefully. The proxy_url combined with the token_prefix forms the access URL."
   }
7. Execute: nohup node bundle.js &
8. You can now access your domain in the browser, using the earlier mentioned proxy_url combined with the token_prefix.
9. For Cloudflare acceleration, refer to Cloudflare documentation.
```
### Contact Information
Telegram group: @siteproxy
<br />
email: netptop@gmail.com
