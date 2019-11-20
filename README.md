# Nginx
## Configure Nginx to redirect different hosts to different directories:
First, make a directory to hold your site:\
`sudo mkdir /var/www/<name>.gavenfinch.site`\
then, create the redirection rule:\
`cd /etc/nginx/sites-available`\
`sudo touch <name>.gavenfinch.site.conf`\
`sudo vim <name>.gavenfinch.site.conf`\
paste the following:
```
server {
  listen 80;
  server_name <name>.gavenfinch.site;
  root /var/www/<name>.gavenfinch.site;
  index index.html;
  default_type "text/html";
  location / {
    try_files $uri $uri/ =404;
  }
}
```
now enable it with a symbolic link:\
`sudo ln -s /etc/nginx/sites-available/<name>.gavenfinch.site.conf /etc/nginx/sites-enabled/`\
reload Nginx:\
`sudo service nginx reload`\

## Configure Nginx to redirect ports with node:
Remember to `npm install`!
Same as above, but paste the following into the .conf file instead:\
```
server {
  listen 80;
  server_name <name>.gavenfinch.site;
  # if your static files are in another directory, such as public,
  # make sure to specify that!
  root /var/www/<name>.gavenfinch.site;
  index index.html;
  default_type "text/html";
  location / {
    # Serve static files from the root location above.
    try_files $uri $uri/ /index.html;
  }
  location /api {
    # Serve api requests here. This will connect to your node
    # process that is running on port <port>.
    proxy_pass http://localhost:<port>;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}
```

## use pm2 to keep node services running:
pm2 start <server.js> --name <name>\
pm2 list
