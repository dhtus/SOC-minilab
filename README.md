Cau hinh cai dat OpenVAS tren Ubuntu SOC lab: 

OpenVAS cai tren docker: 
sudo docker run -d -p 9392:9392 --name OpenVAS imauss/OpenVAS 

Cai them nginx de redirect 9392 sang 443. Dung chuan https:
1. tao cert (luu y: days = 1096 ~3 nam): 
 sudo openssl req -x509 -nodes -newkey rsa:2048 -keyout /etc/ssl/private/openvas.local.key -out /etc/ssl/certs/openvas.local.crt -days 1096 -subj "/CN=openvas.local"

2. Tao Nginx site: 
sudo tee /etc/nginx/sites-available/openvas.local.conf >/dev/null <<'EOF'
server {
  listen 80;
  server_name openvas.local;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl;
  server_name openvas.local;

  ssl_certificate     /etc/ssl/certs/openvas.local.crt;
  ssl_certificate_key /etc/ssl/private/openvas.local.key;

  # OpenVAS (GSAD) is HTTP-only on localhost:9392
  location / {
    proxy_pass http://127.0.0.1:9392;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Proto https;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
EOF

3. Enable site:
sudo ln -sf /etc/nginx/sites-available/openvas.local.conf /etc/nginx/sites-enabled/openvas.local.conf

4. Test & reload nginx:
sudo nginx -t
sudo systemctl reload nginx

5. Truy cap: 
https://openvas.local
