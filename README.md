# dokuwiki-tvbox
Servidor com DokuWIki rorando em uma TV BOX com Armbian
Projeto: Servidor DokuWiki em TV Box com Armbian e PHP 5.3
Descrição
Este projeto configura um servidor web completo com DokuWiki (versão Weatherwax, 2014-09-29) rodando em uma TV Box com Armbian (Debian 11 Bullseye), utilizando Apache 2.4.65 e PHP 5.3.29 (compilado com PHP-FPM). O objetivo foi criar um ambiente local para documentação colaborativa, com suporte a compartilhamento de arquivos via Samba para acesso direto a partir de dispositivos Windows, ideal para testes e prototipagem. O projeto também inclui passos para migração do DokuWiki para um servidor corporativo.
Destaques

Hardware: Executado em uma TV Box Cortex-A53 (aarch64) com ~431 MB de RAM, demonstrando a viabilidade de dispositivos de baixo custo como servidores domésticos.
Prática: TV Boxes são minha escolha recorrente para servidores domésticos de teste devido à sua portabilidade, baixo consumo de energia e custo acessível, permitindo experimentação com sistemas como Armbian.
Desafios técnicos:

Compilação do PHP 5.3.29 em aarch64, superando limitações de memória e dependências obsoletas (ex.: IMAP, OpenSSL).
Configuração de PHP-FPM com socket Unix para integração com Apache.
Compartilhamento seguro via Samba com acesso anônimo para facilitar backups via rede local.


Funcionalidades:

DokuWiki configurado para criação e organização de páginas com namespaces.
Acesso aos arquivos do wiki (páginas, mídia) via Samba no Windows.
Backup automatizado e preparação para migração a um servidor corporativo.



Tecnologias Utilizadas

Sistema Operacional: Armbian (Debian 11 Bullseye, aarch64)
Servidor Web: Apache 2.4.65 com mod_fcgid
PHP: 5.3.29 (compilado manualmente com PHP-FPM)
Wiki: DokuWiki Weatherwax (2014-09-29)
Compartilhamento: Samba (SMB/CIFS) com acesso guest
Ferramentas: gcc, make, wget, nano, systemd

Estrutura do Projeto

Instalação do Ambiente:

Atualização do Armbian e instalação de dependências (libxml2-dev, libjpeg-dev, etc.).
Configuração do Apache com proxy_fcgi para PHP-FPM.


Compilação do PHP 5.3:

Configuração sem suporte IMAP para evitar erros (utf8_mime2text).
Ajustes para arquitetura aarch64 e recursos limitados da TV Box.


Configuração do DokuWiki:

Instalação via interface web, definição de superusuário e ACLs.
Alimentação com páginas e namespaces (ex.: projetos:inicio).


Samba:

Compartilhamento do diretório /var/www/html/wiki com acesso anônimo.
Permissões ajustadas para compatibilidade com www-data (Apache).


Backup e Migração:

Geração de backup com tar e transferência via scp para servidor corporativo.



Como Executar

Pré-requisitos:

TV Box com Armbian (Debian 11) e ~400 MB de RAM.
Conexão à rede local (IP fixo recomendado, ex.: 192.168.1.21).


Passos:

Clone este repositório ou siga o tutorial completo.
Execute os scripts de configuração para Apache, PHP-FPM, DokuWiki e Samba.
Acesse o wiki em http://<IP>:80/wiki e o share em \\<IP>\dokuwiki.



Por que TV Box?
Utilizo TV Boxes como servidores domésticos para testes devido à sua versatilidade e baixo custo. Com um Cortex-A53 e Armbian, é possível rodar servidores web, wikis e serviços de rede com consumo mínimo de energia. Este projeto é um exemplo prático de como hardware acessível pode ser reaproveitado para experimentação e aprendizado.
Notas

Segurança: PHP 5.3 é obsoleto e deve ser usado apenas em ambientes isolados. Planeje upgrades para PHP 8.x em produção.
Limitações: A RAM limitada da TV Box (~431 MB) requer ajustes em pm.max_children do PHP-FPM.
Próximos passos: Adicionar plugins ao DokuWiki (ex.: gallery) e configurar backups automáticos via Samba.
