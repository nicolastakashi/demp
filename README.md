# DEMP: Docker + Nginx + MySQL + PHP

DEMP é um projeto base simples, para desenvolvedores possão executar e depurar seus projetos PHP em sua IDE favorita.

Quando iniciei meu primeiro projeto utilizando PHP senti bastante dificuldade para criar um ambiente de desenvolvimento utilizando Docker e tive mais dificuldade ainda para conseguir realizar o debug de uma aplicação que está sendo executada dentro de um container Docker.

Com isso resolvi criar esse repositório para facilitar a vida dos desenvolvedores que queiram fazer isso.

Basicamente o que fiz foi reunir o conhecimento que ganhei durante a leitura de diversos artigos de blog em um unico lugar. 

### Estrutura do projeto
Por se tratar de um projeto báse, não fiz ele pensando em nenhum framework é apenas PHP rodando dentro do Docker, então deixo você a vontade para colocar sua estrutura de projeto dentro disso.
```sh
├── docker
│   └── php
│       ├── Dockerfile
│       └── docker-php-ext-xdebug.ini
├──  db-data
├── docker-compose.yml
├── LICENSE
├── nginx
│   └── app.conf
├── public
│   ├── index.html
│   └── info.php
└── README.mdc
```
Abaixo deixo uma breve discrição sobre cada diretório.

- **docker** - Diretório para Dockerfile customizados
- **db-data** - Diretório para armazenamento do volume do container do MySQL
- **nginx** - Diretório para os arquivos de configuração do Nginx
- **public** - Diretório com os seus arquivos PHP.

### Docker-Compose.yml
No docker-compose.yml do projeto não tem nada de novo ou mágico, estou utilizando a maioria das imagens direto do Docker Hub, exceto a imagem do PHP que precisei criar um Dockerfile.

### PHP Docker File
Como disse acima não utilizei a imagem do PHP direto do Docker Hub, porque por padrão não vem com a extensão do XDebug instalada.

Na própria página da imagem do PHP no Docker Hub indica que você crie um Dockerfile caso você precice instalar alguma extensão que não esteja habilitada por padrão.

O resultado final foi esse:

```
FROM php:7.1.0-fpm
RUN pecl install redis-3.1.0 \
    && pecl install xdebug-2.5.0 \
    && docker-php-ext-enable redis xdebug

COPY docker-php-ext-xdebug.ini  /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```

Após a instalar do XDebug eu adicionei um comando para poder copiar o XDebug.ini com a configuração que você desejar para dentro do container Docker.

### XDebug.ini

Esse arquivo não tem nada demais, no geral basta você adicionar o seu IP da sua máquina dentro dele para conseguir depurar sua aplicação.

```
zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20160303/xdebug.so
xdebug.remote_enable=1
xdebug.remote_handler=dbgp
xdebug.remote_port=9000
xdebug.remote_autostart=1
xdebug.remote_connect_back=0
xdebug.idekey=docker
xdebug.remote_host={{SEU IP LOCAL}}
```
No geral as configurações do para seu ambiente Docker são essas, agora o que precisa ser feito para depurar suas aplicações que estão rodando dentro do container e realizar a configuração.

### Configurando sua IDE
- [Visual Studio Code]
- [PHPStorm]


[Visual Studio Code]: <https://github.com/nicolastakashi/demp/wiki/Visual-Studio-Code>
[PHPStorm]: <https://github.com/nicolastakashi/demp/wiki/PHPStorm>
