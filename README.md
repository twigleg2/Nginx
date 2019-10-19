# Nginx
## Configure Nginx to redirect different hosts to different directories.
First, make a directory to hold your site:
`sudo mkdir /var/www/<name>.gavenfinch.site`
then, create the redirection rule:
`cd /etc/nginx/sites-available`
`sudo touch <name>.gavenfinch.site.conf`
`sudo vim <name>.gavenfinch.site.conf`
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
now enable it with a symbolic link:
`sudo ln -s /etc/nginx/sites-available/<name>.gavenfinch.site.conf /etc/nginx/sites-enabled/`
reload Nginx:
`sudo service nginx reload`
