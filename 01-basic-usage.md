1 - Instalação
2 - composer.json: Setup do projeto
	1 - A Key "require"
	2 - Nomes de Pacotes
	3 - Versões de pacotes
	4 - Próximo release significativo (o operador ~)
	5 - Estabilidade
3 - Instalação de dependências
4 - composer.lock - O arquivo de "lock"
5 -Packagist
6 - Autoloading



Uso básico

Instalação

Para instalar o composer, você apenas precisa baixar o executável    composer.phar    

    curl -sS https://getcomposer.org/installer | php    

Para maiores detalhes, consulte o capítulo de introdução.

Para checar se o Composer está OK, execute o PHAR através do php: 
    php composer phar    

E será exibida a lista de comandos disponíveis.


Nota: Você também pode executar as verificações apenas sem baixar Composer utilizando o - verificar opção. Para mais informações, é só usar - help.
Nota: Você não precisa baixar o composer para fazer essa verificação, basta usar o parâmetro    --check    . Para maiores informações, use o parâmetro     --help     .

    curl -sS https://getcomposer.org/installer | php -- --help    


composer.json: Setup do Projeto

Para começar a utilizar o Composer em seu projeto, tudo do que você precisa é o arquivo    composer.json     .Este arquivo descreve as dependências de seus projetos, bem como a descrição de outros metadados.

O formato JSON é bem fácil de ser escrito e permite que você defina estruturas aninhadas.

A Chave    require    

A primeira (e geralmente única) coisa que você insere no     composer.json    é a chave    require    . Você simplesmente está mostrando ao Composer quais pacotes seu projeto depende.

{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}


Como você pode ver,    require    requer um objeto que mapeie nomes de pacote (ex.:    1.0.*    ).

Nomes de pacote

Um nome de pacote consiste no nome do fornecedor e o nome do projeto. Frequentemente eles serão identicos - o nome do fornecedor apenas existe para evitar conflitos de nomenclaturas. Isto permite que duas pessoas diferentes criem uma biblioteca chamada    json    , que então terão os nomes    igorw/json    e    seldaek/json    .
Aqui nós temos requerendo    monolog/monolog    , então o nome do fornecedor e do projeto são os mesmos. Para projetos com nomes únicos, isto é altamente recomendado e permite adicionar mais projetos relacionados dentro do mesmo namespace, mais adiante. Se você está mantendo uma lib, essa medida irá tornar mais fácil a divisão da mesma em pacotes menores/desacoplados.

Versões de pacotes

No exemplo anterior nos estamos requerendo a versão    1.0.8    do monolog. Isto abrange qualquer versão de desenvolvimento que esteja no branch    1.0    . Serão contempladas as versões    1.0.0    ,    1.0.2    ou    1.0.20    .

Restrições de versão podem ser definidas de algumas maneiras diferentes: 


| Nome | Exemplo   |  Descricao   |
|---|---|---|---|---|
|Versão exata   | 1.0.2  |  Você pode especificar a versão exata de um pacote |   |   |
| Range  | >=1.0 >=1,0,<2.0 `>=1.0,<1.1  | >1.2`  |  Usando os operadores de comparação, você pode especificar intervalos de versões válidas. Operadores válidos são:     >, >=, <,<=,!=   .Você pode definir múltiplos intervalos, separados por vírgula,que serão tratados como o **operador lógico AND**  | O operador    `    será tratado como o **operador lógico OR**. Lembrando que o Operador AND possui precedência com relação ao OR.  |
| Curinga (wildcard)  |    1.0.*      | Você pode definir um padrão com o     *    .    1.0.*    equivale a     >=1.0,<1.1       |
| Operador til (~)  |     ~1.2      |   Muito útil para projetos que seguem o versionamento semântico.    ~1.2    é equivalente a    >=1.2,<2.0    . Para maiores detalhes, leia a próxima seção.   |


Próximo release significativo (operador ~).

O operador til (    ~    ) é melhor explicado através de um exemplo:     ~1.2    é equivalente a    >=1.2,<2.0    , enquanto    ~1.2.3   é equivalente a     >=1.2.3,<1.3    .Como você pode ver, é mais útil para projetos que respeitam o versionamento semântico. Um uso comum seria para marcar a menor versão secundária depentente, como    ~1.2    (que permite qualquer outra versão exceto a 2.0). Uma vez que, teoricamente, não deve haver problemas de compatibilidade em versões anteriores a 2.0, o pacote irá funcionar corretamente. Uma outra outra explicação é que o     ~    especifica uma versão mínima, mas permite que o último digito especificado seja incrementado.

Nota: Embora    2.0-beta.1   é estritamente anterior a    .20, uma restrição de versão igual a     ~1.2    não será instalada. Como dito anteriormente,    ~1.2   significa que o dígito    .2 pode mudar, mas o dígito    1.    é fixo.


Escalabilidade

Por padrão apenas releases estáveis são levados em conta. Se você gostaria de trazer versões RC, beta, aplha ou dev de suas dependências, você pode fazer isto usando flags de escalabilidade. Para mudar isto para todos os pacotes ao invés de fazer isto por dpendência, você pode também utilizar a configuração minimum-stability.

Instalando dependências

Para buscar as dependências definidas para o seu projeto local, execute o comando    installl  do    composer.phar    .

    php composer.phar install


Isto irá buscar pela última versão do     monolog/monolog    que coincide com a restrição de versão fornecida e será baixada para o diretório    vendor    . É uma convenção colocar códigos de terceiro num diretório chamado    vendor    . No caso do monolog, ele serã baixado para o     vendor/monolog/monolog    .

Dica: Se você está usando o git em seu projeto, você vai querer adicionar o     vendor    dentro de seu    .gitignore.    Você realmente não vai querer adicionar todo esse código no seu repositório.


Outra coisa é que o comando    install   faz é adicionar o arquivo    composer.lock    dentro da raíz do seu projeto.


@TODO:


composer.lock - The Lock File#

After installing the dependencies, Composer writes the list of the exact versions it installed into a composer.lock file. This locks the project to those specific versions.

Commit your application's composer.lock (along with composer.json) into version control.

This is important because the install command checks if a lock file is present, and if it is, it downloads the versions specified there (regardless of what composer.json says).

This means that anyone who sets up the project will download the exact same version of the dependencies. Your CI server, production machines, other developers in your team, everything and everyone runs on the same dependencies, which mitigates the potential for bugs affecting only some parts of the deployments. Even if you develop alone, in six months when reinstalling the project you can feel confident the dependencies installed are still working even if your dependencies released many new versions since then.

If no composer.lock file exists, Composer will read the dependencies and versions from composer.json and create the lock file.

This means that if any of the dependencies get a new version, you won't get the updates automatically. To update to the new version, use update command. This will fetch the latest matching versions (according to your composer.json file) and also update the lock file with the new version.

php composer.phar update
If you only want to install or update one dependency, you can whitelist them:

php composer.phar update monolog/monolog [...]
Note: For libraries it is not necessarily recommended to commit the lock file, see also: Libraries - Lock file.
Packagist#

Packagist is the main Composer repository. A Composer repository is basically a package source: a place where you can get packages from. Packagist aims to be the central repository that everybody uses. This means that you can automatically require any package that is available there.

If you go to the packagist website (packagist.org), you can browse and search for packages.

Any open source project using Composer should publish their packages on packagist. A library doesn't need to be on packagist to be used by Composer, but it makes life quite a bit simpler.

Autoloading#

For libraries that specify autoload information, Composer generates a vendor/autoload.php file. You can simply include this file and you will get autoloading for free.

require 'vendor/autoload.php';
This makes it really easy to use third party code. For example: If your project depends on monolog, you can just start using classes from it, and they will be autoloaded.

$log = new Monolog\Logger('name');
$log->pushHandler(new Monolog\Handler\StreamHandler('app.log', Monolog\Logger::WARNING));

$log->addWarning('Foo');
You can even add your own code to the autoloader by adding an autoload field to composer.json.

{
    "autoload": {
        "psr-4": {"Acme\\": "src/"}
    }
}
Composer will register a PSR-4 autoloader for the Acme namespace.

You define a mapping from namespaces to directories. The src directory would be in your project root, on the same level as vendor directory is. An example filename would be src/Foo.php containing an Acme\Foo class.

After adding the autoload field, you have to re-run install to re-generate the vendor/autoload.php file.

Including that file will also return the autoloader instance, so you can store the return value of the include call in a variable and add more namespaces. This can be useful for autoloading classes in a test suite, for example.

$loader = require 'vendor/autoload.php';
$loader->add('Acme\\Test\\', __DIR__);
In addition to PSR-4 autoloading, classmap is also supported. This allows classes to be autoloaded even if they do not conform to PSR-4. See the autoload reference for more details.

Note: Composer provides its own autoloader. If you don't want to use that one, you can just include vendor/composer/autoload_*.php files, which return associative arrays allowing you to configure your own autoloader.




