# explorer-setup
// The explorer is forked from source code of Pingpub. Tks them for sharing

## 1. Setup and install neccessary packages
```
git clone https://github.com/ping-pub/explorer.git

# Remove old yarn and nodejs (if they are so old and incompatible)
sudo apt remove yarn
sudo apt remove cmdtest


# Install nodejs 16
# sudo apt-get purge --auto-remove nodejs
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install new yarn
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn

# Install new nginx for webserve
sudo apt update && sudo apt -y upgrade
sudo apt -y install sudo vim zip unzip git
sudo apt -y install nginx-full
sudo systemctl status nginx

# if dont wanna use root account for security, you can create new one with sudo permission
adduser cosmos-explorer
adduser cosmos-explorer sudo
```

## 2. Run webseve in develop mode and edit web content
### 2.1 Edit web content
- Open the file `/root/explorer/ping.conf`
- Add below content into the file
```
server {
    server_name  cosmos_explorer;
    listen 443;
    location / {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Max-Age 3600;
        add_header Access-Control-Expose-Headers Content-Length;
    }
}
```
[![image](https://user-images.githubusercontent.com/91453629/190846058-844afa0f-32aa-4362-b991-da6372d939c8.png)](https://github.com/hoangduevu/explorer-setup/issues/1#issue-1379104078)

- Open `/root/explorer/src/views/Home.vue` and edit Main Daskboard

- Open `root/explorer/themeConfig.js` to edit info
[![image](https://user-images.githubusercontent.com/91453629/190849173-8c15f665-71b5-44f4-98bb-f08401fc08a6.png)](https://github.com/hoangduevu/explorer-setup/issues/1#issuecomment-1252090754)

- Open `/root/explorer/src/@core/layouts/components/AppFooter.vue` to edit footer
[![image](https://user-images.githubusercontent.com/91453629/190849926-713fc134-c569-4c0d-8097-95e47bb399b8.png)](https://github.com/hoangduevu/explorer-setup/issues/1#issuecomment-1252093365)

- Create your logo filenames `logo.svg` and `logo.svg`, then upload to path `/root/explorer/public/`

- Upload your logo to [imgur](https://imgur.com/), then create your online image link, example `https://i.imgur.com/pBRmmiE.jpg`
- Open `/root/explorer/src/@core/layouts/components/Logo.vue` to fill your online image link, then Logo of Main Dashboard will be changed
![image](https://user-images.githubusercontent.com/91453629/190850453-7794ca44-9830-4c1f-902c-8e33c2cdee2f.png)
![image](https://user-images.githubusercontent.com/91453629/190850419-859780c3-dd7b-403e-ade0-27ab7cf30111.png)

- Edit json file of your chain in `/root/explorer/src/chains`, fill in your own RPC and API if need (Step 2.2)

### 2.2 Edit config data of your chain
* Below is example for HAQQ chain
- Open `/root/.haqqd/config/config.toml`, expose your RPC, enable CORS.
![image](https://user-images.githubusercontent.com/113001655/191224989-0a9090cc-107a-479d-8a3d-2199dd759ab2.png)

- Open `/root/.haqqd/config/app.toml`, enable API and CORS
![image](https://user-images.githubusercontent.com/113001655/191225435-5417525b-4b41-48fc-8dd9-3288ec88c65e.png)

- Restart HAQQ

### 2.3 Run webserver in dev mode
Open new terminal and run below command
```
yarn && yarn serve
```
### 2.4 Login web server in dev mode
- Open your browser, fill in: http://YOUR_IP:8080
- Check content is OK or not, then adjust directly on your data and refresh your browser to recheck until all is OK

### 2.5 Build webserver
- Open new terminal and run below command
```
yarn && yarn build
```
- Copy your web data to path of Root webserver
```
mkdir /var/www/YOUR_NAME
cp /root/explorer/dist/* /var/www/YOUR_NAME
```
- Check and edit the file `/etc/nginx/sites-available/default`
![image](https://user-images.githubusercontent.com/113001655/191225796-3a8246a0-9720-43a0-9dda-7c5050b0983c.png)

- Restart your webserver
```
systemctl restart nginx
```
# check
```
systemctl status nginx.service
journalctl -xe
```
- Login your browser: http://YOUR_IP
# 3.link Domain
#3.1 go to namecheap https://www.namecheap.com/myaccount/login/ buy domain
#3.2 link domain from namecheap to Cloudflare https://www.youtube.com/watch?v=i0IY77IUHx4
in https://dash.cloudflare.com/ go to setting DNS 

