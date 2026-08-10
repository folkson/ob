version: '3'

services:
  bitwarden:
    image: vaultwarden/server:alpine
    restart: always
    volumes:
      - ${WEBAPP_STORAGE_HOME}/site/wwwroot/bitwarden:/home
    command: ["/home/bitwarden.sh"]
    environment:                                  
      - I_REALLY_WANT_VOLATILE_STORAGE=true        
      - SIGNUPS_ALLOWED=false                       
      - WEB_VAULT_ENABLED=false                     
      - ADMIN_TOKEN=
      - DOMAIN=https://vaults.azurewebsites.net                 
      - PUSH_ENABLED=true                          
      - PUSH_INSTALLATION_ID=aa3499ce-1977-4ae9-b5ec-b2180030c2e9               
      - PUSH_INSTALLATION_KEY=1jkmVn40kFdQQZu9RQ9t            
      - REALTIME_BAK_CYCLE=30                      
      - REALTIME_BAK_COUNTS=10                     
      - DAILY_BAK_COUNTS=5                         
      - WEBDAV_URL=https://dav.jianguoyun.com/dav/wardenbit/  
      - WEBDAV_USER=xtcfe20@163.com           
      - WEBDAV_PASS=ahsrcx7e6fdzvm37           

  nginx:
    image: nginx:alpine
    volumes:
      - ${WEBAPP_STORAGE_HOME}/site/wwwroot/bitwarden:/home
    ports:
      - "80:80"
    restart: always
    entrypoint: /home/bitwarden.sh
    command: ["nginx","-g","daemon off;"]
    depends_on:
      - bitwarden