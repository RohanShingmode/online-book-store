## Deploy .NET Application on Ubuntu Server using Nginx
For Deploy the .NET application create the application .
Update Your Ubuntu Server
```
sudo apt-get update
```
### Install classic version of .NET  sdk
```
sudo snap install dotnet-sdk --classic
```
### Check .NET Version
```
dotnet --version
```
### Install Nginx
```
sudo apt-get install nginx
```
### Enable and Start Nginx
```
systemctl enable nginx
systemctl start nginx
```
### Check status of Nginx must be Active
```
systemctl status nginx
```
### Open Default file and change the location 80 to 5000 port
```
sudo nano /etc/nginx/sites-available/default
```
### put this content on location
```
   location / {
       proxy_pass http://localhost:5000;
       proxy_http_version 1.1;
       proxy_set_header Upgrade $http_upgrade;
       proxy_set_header Connection keep-alive;
       proxy_set_header Host $host;
       proxy_cache_bypass $http_upgrade;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;
   }
```
### Change the directory to default location
```
cd /var/
```
### Open powershell and type SCP command to copy your .NET Application to Virtual machine
```
scp -o  -r C:\Users\QPL0038\source\repos\WebApplication1\HelloWorldApp.web your_username@your_vm_public_ip:~
```
### move your app to /var/ folder
```
mv HelloWorldApp.web /var/
```
### Run Your Application Locally on  http://yourip:5000
```
cd /var/
sudo dotnet HelloWorldApp.web.dll
```
### Stop the Application Ctrl+c create .service file 
```
nano /etc/systemd/system/[YourAppName].service
```
### If any access denied error Arises use sudo of the start of command
###  Copy this Lines and change according to your application name and Folder
```
[Unit]
Description=my Web Application

[Service]
WorkingDirectory=/var/HelloWorldApp.web
ExecStart=/usr/bin/dotnet /var/HelloWorldApp.web/HelloWorldApp.web.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production


[Install]
WantedBy=multi-user.target

```
### Save this file and enable the service 
```
sudo systemctl enable [YourAppName].service
```
### Start the service
```
sudo systemctl start [YourAppName].service
```
### Reload the server
```
sudo nginx -s reload
```
### Check status of your Service must be Running
```
systemctl status [YourAppName].service
```
### Check your application on browser only type your IP Address on browser

<img width="960" alt="nwfile" src="https://github.com/RohanShingmode/Jenkins-installation-ubuntu/assets/104613881/841e80d2-8f9e-4a9f-94d1-c6037c0ce4b3">

Reference:

[.NET app install on ubuntu using nginx](https://youtu.be/cpkX9mScZEU?si=idOKRBkxWDgc6cBB)

[deploy .NET Application on Nginx](https://youtu.be/AheinMduSXM?si=3SJ-VQ5hP-01CplL)





