# NGINX-git sync

## 1. Instalação

### 1.1 Caso tenha arquivos de configurações importantes faça backup.

```bash
zip -r  /etc/nginx /path/to/nginx.bkp
```
### 1.2 Remover o diretório do nginx para evitar conflito.
```bash
sudo rm -rf /etc/nginx
```
### 1.3 Em seguida, crie um novo diretório vazio para o nginx e ajuste as permissões
```bash
sudo mkdir -p /etc/nginx && sudo chown nginx:nginx /etc/nginx
```
### 1.4 Como usuário nginx faça o clone do repositório git

```bash
git clone https://token:<token>@... /etc/nginx
```

## 2.0 configuração
2.1 Como usuário root, agora vamos criar o arquivo nginx-git-sync.service.

```bash
vim /lib/systemd/system/nginx-git-sync.service
```
Deve conter as seguintes informações: 

```config
[Unit] 
Description=Sync NGINX config using git 
After=nginx.target 

[Service] 
User=nginx 
Group=nginx 
Type=oneshot 
WorkingDirectory=/etc/nginx 
ExecStart=/usr/local/sbin/nginx-git-sync 

[Install] 
WantedBy=multi-user.target 
```
2.2 Em seguida vamos criar o arquivo de timer no systemd
```bash
vim /lib/systemd/system/nginx-git-sync.timer
```
Deve conter as seguintes informações
```config
[Unit]
Description=Run nginx-git-sync

[Timer]
OnCalendar=*:0/15
AccuracySec=1us
Unit=nginx-git-sync.service

[Install]
WantedBy=timers.target
```
## Testar se tudo está funcionando corretamente.


## Screenshots
```bash
sudo systemctl status nginx-git-sync.service
```
![](https://i.imgur.com/PSaMS3N.png)
```bash
sudo systemctl status nginx-git-sync.timer
```
![](https://i.imgur.com/ugSXsH4.png)



