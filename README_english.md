# Siteproxy 2.0
Siteproxy 2.0 uses service workers, making the proxy more stable and able to proxy more websites. It also uses Hono instead of Express, which quadruples the speed. It supports deployment on Cloudflare Workers. This reverse proxy allows access to YouTube/Google without needing to bypass internet censorship and supports login for GitHub and Telegram web.

It's a purely web-based online proxy, requiring no configuration on the client side; it reverse proxies to the internet.

```
                                                 +----> google/youtube
                             +----------------+  |
                             |                |  |
user browser +-------------->+ siteproxy      +-------> wikipedia
                             |                |  |
                             +----------------+  |
                                                 +----> chinese forums
```

Please do not use this project for illegal purposes; you are responsible for the consequences.

## Table of Contents
- [Features](#features)
- [Usage Tips](#usage-tips)
- [Deploy to Cloudflare Worker](#deploy-to-cloudflare-worker)
- [Deploy to VPS or Cloud Server](#deploy-to-vps-or-cloud-server)
- [Contact Information](#contact-information)

### Features
- Uses Hono instead of Express, speeding up the service by four times.
- Supports deployment on Cloudflare Workers.
- Supports password-controlled access to the proxy; only those with the password can access the proxy.
- No client-side configuration needed; accessing the proxy URL grants access to the worldwide web.
- Supports login for GitHub and Telegram web.
- Accessing the worldwide web and hiding your IP is as simple as entering the URL of the deployed Siteproxy.
- No software installation or browser configuration required on the client side.

### Usage Tips
1. You can use the deployed Siteproxy for git clone operations like this:
```
git clone https://your-proxy-domain.name/user-your-password/https/github.com/the-repo-to-clone
```

### Deploy to Cloudflare Worker

```
1. Assuming your domain is already managed by Cloudflare, set your proxy website's domain DNS to any IP, such as 192.0.2.2, and ensure it is proxied, so the proxy status is: Proxied.
2. Git clone this project and open build/worker.js with a text editor (or just download this file directly).
3. Search for the string http://localhost:5006 and replace it with your proxy website domain, like https://your-proxy-domain.name. Also, search for user22334455 and change it to a password of your choosing.
4. Create a worker, edit the worker, and copy and paste the edited worker.js into it. Save and deploy.
5. Add a worker route, directing your-proxy-domain.name/* to the worker you just saved.
6. Now you can directly access https://your-proxy-domain.name/user-your-password/, with the domain and password replaced by your own.
```

### Deploy to VPS or Cloud Server
```
1. Create an SSL website (using certbot and nginx, look up usage) and configure nginx. /etc/nginx/sites-enabled/default should include the following content::
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
2. Execute: sudo systecmctl restart nginx
3. Under user environment, run the following commands to install node environment, if you don't already have node, skip this step
   (1)curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
   (2)source ~/.bashrc
   (3)nvm install v18
4. Execute:git clone https://github.com/netptop/siteproxy.git;
5. Execute:cd siteproxy;
6. Open and modify the config.json file then save:
   {
      "proxy_url": "https://your-proxy.domain.name", // This is your proxy server domain
      "token_prefix": "/user-SetYourPasswordHere/", // This acts as your site password to prevent unauthorized access. Keep the slashes at both ends.
      "description": "Note: token_prefix acts as your site password, set it carefully. proxy_url and token_prefix together form the access URL."
   }
7. Execute: nohup node bundle.js &
8. Now, you can access your domain in a browser; the URL is the proxy_url followed by the token_prefix.
9. For Cloudflare acceleration, refer to Cloudflare documentation.
```
### Contact Information
Telegram group: @siteproxy
<br />
email: netptop@gmail.com
