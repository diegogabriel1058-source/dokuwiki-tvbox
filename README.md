# 🧠 DokuWiki-TVBox  
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
