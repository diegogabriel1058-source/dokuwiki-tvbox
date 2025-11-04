# 🧠 DokuWiki-TVBox  S905x
Servidor DokuWiki rodando em uma TV Box com Armbian  

## 🗂️ Projeto  
**Servidor DokuWiki em TV Box com Armbian e PHP 5.3**

Este projeto demonstra como configurar um **servidor web completo** com **DokuWiki (versão Weatherwax – 2014-09-29)** em uma **TV Box** executando **Armbian (Debian 11 Bullseye)**, utilizando **Apache 2.4.65** e **PHP 5.3.29** (compilado manualmente com PHP-FPM).  

O objetivo é criar um **ambiente local de documentação colaborativa**, com suporte a **compartilhamento de arquivos via Samba** para acesso direto por dispositivos Windows — ideal para testes, estudos e prototipagem.  

O projeto também inclui **passos de backup e migração** para um servidor corporativo.

---

## ⚙️ Destaques

- **Hardware:** executado em uma TV Box **Cortex-A53 (aarch64)** com ~431 MB de RAM — mostrando a viabilidade de dispositivos de baixo custo como servidores domésticos.  
- **Prática:** TV Boxes são ideais para servidores de teste devido à **portabilidade**, **baixo consumo de energia** e **custo acessível**, possibilitando experimentos com Armbian e outros sistemas.  
- **Desafios técnicos:**  
  - Compilação do **PHP 5.3.29** em arquitetura *aarch64*, superando dependências obsoletas e limitações de memória.  
  - Integração do **PHP-FPM** com **Apache** via *Unix socket*.  
  - Configuração de **Samba** com acesso anônimo seguro para facilitar backups em rede local.  

---

## ✨ Funcionalidades

- DokuWiki configurado com **namespaces** e controle de acesso.  
- **Acesso via Samba** aos diretórios de páginas e mídia do Wiki.  
- **Backup automatizado** e preparação para migração para ambiente corporativo.  

---

## 🧰 Tecnologias Utilizadas

| Componente | Versão / Descrição |
|-------------|--------------------|
| **Sistema Operacional** | Armbian (Debian 11 Bullseye, aarch64) |
| **Servidor Web** | Apache 2.4.65 com mod_fcgid |
| **PHP** | 5.3.29 (compilado manualmente com PHP-FPM) |
| **Wiki** | DokuWiki Weatherwax (2014-09-29) |
| **Compartilhamento** | Samba (SMB/CIFS) com acesso *guest* |
| **Ferramentas** | gcc, make, wget, nano, systemd |

---

## 🏗️ Estrutura do Projeto

### 1. Instalação do Ambiente
- Atualização do Armbian e instalação das dependências (`libxml2-dev`, `libjpeg-dev`, etc).  
- Configuração do Apache com **proxy_fcgi** para integração ao PHP-FPM.  

### 2. Compilação do PHP 5.3
- Configuração sem suporte IMAP para evitar erros (`utf8_mime2text`).  
- Ajustes para arquitetura *aarch64* e otimização para hardware limitado.  

### 3. Configuração do DokuWiki
- Instalação via interface web, criação de superusuário e definição de ACLs.  
- Organização das páginas com namespaces (exemplo: `projetos:inicio`).  

### 4. Configuração do Samba
- Compartilhamento do diretório `/var/www/html/wiki` com acesso anônimo.  
- Permissões ajustadas para o usuário **www-data (Apache)**.  

### 5. Backup e Migração
- Geração de backup via `tar` e transferência para servidor remoto via `scp`.

---

## 🚀 Como Executar

### Pré-requisitos
- TV Box com **Armbian (Debian 11)** e **mínimo de 400 MB de RAM**.  
- Conexão na **rede local** (IP fixo recomendado, ex.: `192.168.1.21`).  

### Passos
1. Clone este repositório ou siga o tutorial completo.  
2. Execute os scripts de configuração do Apache, PHP-FPM, DokuWiki e Samba.  
3. Acesse:  
   - Wiki: `http://<IP>:80/wiki`  
   - Compartilhamento: `\\<IP>\dokuwiki`  

---

## 💡 Por que usar uma TV Box?

TV Boxes são uma excelente base para **servidores domésticos e experimentais**, pois oferecem:  
- Processadores eficientes (como o Cortex-A53)  
- Baixo consumo de energia  
- Facilidade de uso com Armbian  
- Custo extremamente baixo  

Este projeto é um **exemplo prático de reaproveitamento de hardware** para aprendizado e experimentação com servidores Linux.

---

## ⚠️ Notas Importantes

- **Segurança:** o PHP 5.3 está obsoleto e deve ser usado **apenas em ambientes isolados**.  
  Para produção, recomenda-se atualizar para PHP 8.x.  
- **Limitações:** a RAM limitada (~431 MB) exige ajustes no `pm.max_children` do PHP-FPM.  
- **Próximos passos:** adicionar plugins ao DokuWiki (ex.: *gallery*) e configurar **backups automáticos via Samba**.  

---

🗓️ **Versão:** 1.0  
📍 **Licença:** MIT 

---
## 🚀 Como Executar

### Pré-requisitos
- TV Box com **Armbian (Debian 11)** e **mínimo de 400 MB de RAM**.  
- Conexão na **rede local** (IP fixo recomendado, ex.: `192.168.1.21`).  

### 🧱 Instalação do Armbian na TV Box (Resumido)

Como uso TV Boxes (ex.: Cortex-A53, aarch64) como servidores domésticos, o **Armbian** é ideal por sua leveza e suporte a ARM.  
Baixe do site oficial, flashe no SD e insira na TV Box.

#### 🔽 Download
1. Acesse [https://www.armbian.com](https://www.armbian.com).  
2. Selecione o modelo da sua TV Box (ex.: **Amlogic S905X3** ou **Rockchip RK3328**) usando a ferramenta de seleção automática.  
3. Baixe a imagem **Debian 11 (Bullseye)** para **aarch64**, ex.:  
Armbian_23.02.0-trunk_Bullseye_current_5.15.93.img.xz

💾 Preparar o Cartão SD
Use um cartão SD de 8 GB+, classe 10.

No Windows:

Instale o Balena-Etcher.

⚙️ Boot na TV Box
Insira o SD na TV Box.

Conecte HDMI, teclado/mouse ou use SSH via Ethernet.

O boot leva 1–2 min e expande o filesystem automaticamente.

Login padrão:

Usuário: root
Senha: 1234
(mude imediatamente com passwd).

🧩 Instalação do DokuWiki, PHP 5.3 e Samba (Passo a Passo)
Após o Armbian iniciar, execute os comandos como root.
Monitore memória com free -h (ajuste pm.max_children se < 400 MB).

⚙️ Passo 1: Atualizar o Sistema e Instalar Dependências

Atualize o sistema:

sudo apt update
sudo apt upgrade -y


Instale dependências:

sudo apt install -y build-essential apache2 libapache2-mod-fcgid libxml2-dev libjpeg-dev libpng-dev \
libfreetype6-dev libmcrypt-dev libxslt1-dev libkrb5-dev libltdl-dev default-libmysqlclient-dev wget \
tar nano pkg-config libssl-dev libreadline-dev zlib1g-dev libzip-dev libicu-dev libonig-dev \
libsqlite3-dev libbz2-dev libcurl4-openssl-dev libgmp-dev libldap2-dev libsodium-dev libargon2-dev


Limpe pacotes desnecessários:

sudo apt autoremove -y

🌐 Passo 2: Instalar e Configurar o Apache

Habilite módulos necessários:

sudo a2enmod proxy_fcgi setenvif


Edite o VirtualHost:

sudo nano /etc/apache2/sites-available/000-default.conf


Substitua o conteúdo por:

<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php5-fpm.sock|fcgi://localhost/"
    </FilesMatch>
</VirtualHost>


Teste e reinicie o Apache:

sudo apache2ctl configtest
sudo systemctl restart apache2
sudo systemctl status apache2

🧱 Passo 3: Compilar e Instalar o PHP 5.3.29 com PHP-FPM

Baixe e extraia:

cd ~
wget https://www.php.net/distributions/php-5.3.29.tar.gz
tar xzf php-5.3.29.tar.gz
cd php-5.3.29


Configure (sem IMAP):

./configure --prefix=/opt/php5 --with-config-file-path=/opt/php5/etc --enable-fpm \
--with-fpm-user=www-data --with-fpm-group=www-data --enable-bcmath --enable-opcache \
--enable-ftp --enable-gd-native-ttf --enable-libxml --enable-mbstring --enable-soap \
--enable-sockets --enable-zip --with-curl --with-freetype-dir=/usr --with-gd \
--with-gettext --with-mcrypt --enable-mysqlnd --with-mysql=mysqlnd --with-pdo-mysql=mysqlnd \
--with-mysqli=mysqlnd --with-openssl --with-zlib --with-xsl --enable-calendar \
--with-jpeg-dir=/usr --with-png-dir=/usr --host=aarch64-linux-gnu


Compile e instale:

make
sudo make install


Teste o binário:

/opt/php5/sbin/php-fpm --test

🔧 Passo 4: Configurar o PHP-FPM

Crie diretórios:

sudo mkdir -p /opt/php5/etc/pool.d /opt/php5/var/run /run/php
sudo chown www-data:www-data /run/php


Arquivo /opt/php5/etc/php-fpm.conf:

[global]
error_log = /var/log/php-fpm.log
include=/opt/php5/etc/pool.d/*.conf


Arquivo /opt/php5/etc/pool.d/www.conf:

[www]
user = www-data
group = www-data
listen = /run/php/php5-fpm.sock
listen.owner = www-data
listen.group = www-data
pm = dynamic
pm.max_children = 3
pm.start_servers = 1
pm.min_spare_servers = 1
pm.max_spare_servers = 2


Serviço Systemd /lib/systemd/system/php5-fpm.service:

[Unit]
Description=The PHP 5.3 FastCGI Process Manager
After=network.target

[Service]
Type=simple
PIDFile=/opt/php5/var/run/php-fpm.pid
ExecStart=/opt/php5/sbin/php-fpm --nodaemonize --fpm-config /opt/php5/etc/php-fpm.conf
ExecReload=/bin/kill -USR2 $MAINPID

[Install]
WantedBy=multi-user.target


Inicie o PHP-FPM:

sudo systemctl daemon-reload
sudo systemctl enable --now php5-fpm

📘 Passo 5: Instalar e Configurar o DokuWiki

Baixe e extraia:

cd ~
wget https://download.dokuwiki.org/src/dokuwiki/dokuwiki-2014-09-29.tgz
sudo tar xzf dokuwiki-2014-09-29.tgz -C /var/www/html/
sudo mv /var/www/html/dokuwiki-2014-09-29 /var/www/html/wiki


Permissões:

sudo chown -R www-data:www-data /var/www/html/wiki
sudo chmod -R 755 /var/www/html/wiki


Instalação via navegador:

Acesse: http://192.168.1.21/wiki/install.php

Defina o nome do wiki e crie o superusuário

Escolha uma política de ACL (ex.: Open Wiki para testes)

Depois, remova o instalador:

sudo rm /var/www/html/wiki/install.php

💾 Passo 6: Alimentar o DokuWiki

Crie páginas e namespaces pela interface web (ex.: docs:projeto1)

Os dados ficam em:

/var/www/html/wiki/data/pages

🚀 FEITO!
