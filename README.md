# Wordpress SSL docker environment

## Last update 16/09/2025

## Introduction
We Are installing wordpress with ssl on our computer
What we will need to make our app works:
2. Before proceding you need to install
- powershell for windows [here](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5)
- mkcert using [chocolately](https://chocolatey.org/) 
- Docker Desktop [here](https://docs.docker.com/desktop/setup/install/windows-install/)
- An IDE of your choice, recommand [Visual Studio Code](https://code.visualstudio.com/download) (VScode)

## Install
1. Open VScode
2. Into VScode open the terminal via the menu Terminal => New Terminal
3. clone the repository from the folder up to the one you want to clone
For example 
the folder we are in up_to_my_project_folder = /myapps 
the folder to clone to my_project_folder_name = /myapps/wordpress
```
cd {up_to_my_project_folder}
git clone https://github.com/astro567/wordpress-ssl.git {my_project_folder_name}
```

## Create SSL certificates
1. Open powershell
2. Get ssl certifications file with **your custom local domain {eg: my_domain.local}** 
Make sure the name are as follow
**my_domain.local.pem**  
**my_domain.local-key.pem**  
replacing my_domain.local by your custom local domain 
3. Copy **your certs (my_domain.local.pem & my_domain.local-key.pem)** to ./server/conf/certs

## Set your custom domain
1. Open Vscode
2. Open /server/Dockerfile
Replace line 2 my_domain.local by your custom domain
```
ENV DOMAIN_NAME my_domain.local => ENV DOMAIN_NAME my_custom.domain
```
3. Rename **.env-example** to **.env** and fill **.env** file
```
mv .env-example .env
code .env
```
Replace in .env DOMAIN_NAME=my_domain.local by your custom domain
4. add **{my_domain.local}** to your hosts file
Navigate to the C:\Windows\System32\drivers\etc folder in File Explorer. 
Right-click on the hosts file and open it as administrator with a text editor like VScode. 
You may need to run VScode as an administrator to make changes.
Add the line below at the end of the file
```
127.0.0.1   my_domain.local
```

## Activate the app
1. Run in VScode terminal:
my_wordpress_repository is the repository where you clone the app (/myapps/wordpress/)
```
cd my_wordpress_repository
docker-compose build --no-cache
docker-compose up -d
```

## Customize your wordpress (optional)
1. **Copy your personal theme and plugins** into wp-content/plugins/ and wp-content/themes/  
For exmample botiga theme from [here](https://athemes.com/theme/botiga/)  
3. **Update .gitignore file** with your plugin and themes
To keep track of your plugin and themes into your github account
> remove  
    /wp-content/plugins/  
    /wp-content/themes/  
> add  
    /wp-content/plugins/*  
    !wp-content/plugins/{my plugin}/  
    /wp-content/themes/*  
    !/wp-content/themes/{my theme}/
4. Browse to phpMyAdmin: [http://localhost:9000]  
**Import your database** 
For example database for botiga: ([click here](https://www.dropbox.com/scl/fi/tru6inl3di26q3a939t0p/smart.sql?rlkey=5m49go8lu89m5wsr4qd1hezp7&dl=0) to get the sql file)


## Run your app
11. Browse to your website [https://{my_domain.local}]   
12. Thats it!

[https://{my_domain.local}]: https://my_domain.local
[http://localhost:9000]: http://localhost:9000
