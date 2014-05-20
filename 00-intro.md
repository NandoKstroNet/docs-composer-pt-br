# Introdução

Composer é uma ferramenta para gerenciamento de dependências no PHP. Ele permite que você declare as
dependências que o seu projetos necessita, baixa e instala as mesmas para você. 

## Gerenciando dependências

Composer não é gerenciador de pacotes. Sim, ele trabalha com pacotes e dependências, porém 
ele irá gerenciar uma base por projeto, instalá-los em um diretório (ex.: `vendor`)
dentro de seu projeto. Por padão o composer nunca instalará nada de forma global. Sendo assim,
ele é um gerenciador de dependências.

Esta ideia não é nova e o Composer é fortemente inspirado no [npm](http://npmjs.org/)
do node.js e no [bundler](http://gembundler.com/) do ruby. Mas aqui nós temos uma ferramenta
para o PHP.

Os problemas que o composer vêm a resolver, são: 

a) Você têm um projeto que depende de algumas bibliotecas.

b) Algumas dessas bibliotecas dependem de outras bibliotecas.

c) Você declara suas dependências.

d) Composer irá encontrar as versões que seus pacotes precisam e irá instalá-los.(em outras palavras, ele baixa os pacotes em seu projeto).

## Declarando dependências

Digamos que você estar criando um projeto e necessita de uma ferramenta para geração de logs.
Você opta por usar o [monolog](https://github.com/Seldaek/monolog). Para adicionar o Monolog ao seu
projeto, tudo que você precisa fazer é criar um arquivo chamado `composer.json` e descrever as dependências
do projeto, veja abaixo:

    {
        "require": {
            "monolog/monolog": "1.2.*"
        }
    }

Nós estamos simplismente dizendo que nosso projeto requer algum pacote `monolog\monolog`, que 
inicie com a versão `1.2`.

## Requerimentos de sistema

Composer requer PHP5.3.2+ para rodar. Algumas configurações sensíveis do php e compilar o mesmo
com algumas flags serão necessárias, mas o instalador irá avisá-lo sobre qualquer imcompatibilidade. 

Para instalar o pacote via sources ao invéz de simples arquivos zip, você irá precisar de git, svn ou
hg dependendo de como o pacote estar versionado.

Composer é multi-plataforma e nós nos esforçamos para fazê-lo rodar igualmente bem no Windows,
Linux e OSX.

## Instalação - *nix

### Baixando o executável do composer

#### Localmente

Para obter o composer, você precisa fazer apenas duas coisas. A primeira é instalar o 
Composer (lembre-se, isto irá baixar o composer em seu projeto):

    $ curl -sS https://getcomposer.org/installer | php

O comando acima irá verificar algumas configurações do seu php e baixar o composer na
pasta do seu projeto. Este arquivo é o binário do Composer. Ele é um PHAR (arquivo PHP),
que é um formato de arquivo PHP que, dentre outras coisas, pode ser executado via linha de 
comando.

Você pode instalar o Composer para um diretório especifico usando a opção `--install-dir`
e fornecer um diretório de destino(que pode ser um caminho absoluto ou relativo):

    $ curl -sS https://getcomposer.org/installer | php -- --install-dir=bin

#### Globalmente

Você pode colocar este arquivo(.phar) em qualquer lugar que você desejar. Se você colocá-lo no `PATH` do seu sistema, você poderá acessá-lo globalmente. Em sistemas unix você pode torná-lo executável e chamá-lo sem o `php`.

Você pode rodar o comando abaixo para acesso fácil ao `composer` de qualquer local do seu sistema:

    $ curl -sS https://getcomposer.org/installer | php
    $ mv composer.phar /usr/local/bin/composer

> **Nota:** Se houver falhas no comando acima, execute o comando `mv`
> novamente como sudo.

Então, basta executar `composer` em seguida para utilizá-lo ao invés de `php composer.phar`.

#### Globalmente (no OSX via homebrew)

Composer faz parte do projeto homebrew-php.

1. Adicione o repositório homebrew-php em sua instalação do brew se você ainda 
   não o fez: `brew tap josegonzalez/homebrew-php`.
2. Execute `brew install josegonzalez/php/composer`.
3. Use o Composer com o comando `composer`.

> **Nota:** Se você receber um erro falando que PHP53 ou maior é necessário, use o seguinte comando para instalar o php 
> `brew install php53-intl`

## Instalação - Windows

### Usando o instalador

Este é o caminho mais fácil para obter o Composer em sua máquina

Baixe e execute [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe),
isto irá instalar a versão mais recente do Composer e configurar seu PATH, o que você precisará fazer
em seguida é apenas chamar o comando `composer` em seu diretório via linha de comando.

### Instalação manual

Mude o diretório em seu `PATH` e execute o seguinte snippet instalador para baixar o 
composer.phar:

    C:\Users\username>cd C:\bin
    C:\bin>php -r "eval('?>'.file_get_contents('https://getcomposer.org/installer'));"

> **Nota:** Se houver falha no comando acima no file_get_contents, use a url `http` ou habilite o php_openssl.dll no php.ini

Crie um arquivo `composer.bat` ao lado de `composer.phar`:

    C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat

Feche seu terminal atual. Faça o teste usando um novo execução do seu terminal:

    C:\Users\username>composer -V
    Composer version 27d8904

    C:\Users\username>

## Usando o composer

Nós iremos usar o Composer para instalar as dependências do projeto. Se você não têm um
`composer.json` no seu diretório atual por favor pule para o capítulo
[Uso Básico](01-basic-usage.md).

Para resolver e baixar as dependências, execute o seguinte comando:

    $ php composer.phar install

Se você fez a instalação global e não têm o .phar no diretório execute o seguinte
comando:

    $ composer install

Seguindo o [exemplo acima](#declaring-dependencies), iremos baixar o monolog dentro de `vendor/monolog/monolog`.

## Autoloading

Além de baixar as bibliotecas, o Composer também prepara um arquivo de autoload que é capaz
de fazer o carregamento automático de todas as classes de qualquer biblioteca baixada pelo mesmo.
Para usá-lo, apenas adicione a seguinte linha no seu arquivo bootstrap:

    require 'vendor/autoload.php';

Woah! Agora comece a usar o monolog! Para continuar aprendendo mais sobre o Composer, leia o capítulo "Uso Básico".

[Uso Básico](01-basic-usage.md) &rarr;
