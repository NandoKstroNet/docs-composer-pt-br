1. Instalação
2. composer.json: Setup do projeto
	1. A Key "require"
	2. Nomes de Pacotes
	3. Versões de pacotes
	4. Próximo release significativo (o operador ~)
	5. Estabilidade
3. Instalação de dependências
4. composer.lock - O arquivo de "lock"
5. Packagist
6. Autoloading.





#Uso básico

##Instalação

Para instalar o composer, você apenas precisa baixar o executável `composer.phar`: 

    curl -sS https://getcomposer.org/installer | php    

Para maiores detalhes, [consulte o capítulo de introdução].

Para checar se o Composer está OK, execute o PHAR através do `php`: 

    php composer phar    

E será exibida a lista de comandos disponíveis.


Nota: Você não precisa baixar o composer para fazer essa verificação, basta usar o parâmetro `--check`. Para maiores informações, use o parâmetro `--help`.

    curl -sS https://getcomposer.org/installer | php -- --help    



##`composer.json`: Setup do Projeto

Para começar a utilizar o Composer em seu projeto, tudo do que você precisa é do arquivo `composer.json`. Este arquivo descreve as dependências de seus projetos, bem como a descrição de outros metadados.

O formato JSON é bem fácil de ser escrito e permite que você defina estruturas aninhadas.

###A key `require`    

A primeira (e geralmente única) informação que você insere no `composer.json` é a chave `require`. Você simplesmente está mostrando ao Composer quais pacotes seu projeto depende.

    {
    "require": {
        "monolog/monolog": "1.0.*"
    }
    }


No exemplo acima,`require` pede um objeto que faça a referência as versões do pacote (ex.: `1.0.*`).


###Nomes de pacote


Um nome de pacote consiste no nome do fornecedor (*vendor*) e no nome do projeto. Frequentemente eles serão identicos - o nome do *vendor* apenas existe para evitar conflitos com nomes. Isto permite que duas pessoas diferentes criem uma biblioteca chamada `json`, que então terão os nomes `igorw/json` e `seldaek/json`, por exemplo.
Aqui vamos requerer o  `monolog/monolog`, então o *vendor* name e o nome do projeto são os mesmos. Para projetos com nomes únicos, isto é altamente recomendado: e permite mais adiante, adicionar mais projetos relacionados dentro do mesmo *namespace*. Se você está mantendo uma lib, essa medida irá tornar mais fácil a divisão da mesma em pacotes menores/desacoplados.

###Versões de pacotes

No exemplo anterior nós 'exigimos' a versão `1.0.*` do `monolog`. Isto abrange qualquer versão de desenvolvimento que esteja no branch `1.0`. Serão contempladas as versões `1.0.0`,`1.0.2` ou `1.0.20`.

Restrições de versão podem ser definidas de algumas maneiras diferentes: 

| Nome | Exemplo   |  Descricao   |
|---|---|---|---|---|
|Versão exata   | `1.0.2`  |  Você pode especificar a versão exata de um pacote |   |   |
| Range  | `>=1.0` `>=1,0,<2.0` `>=1.0,<1.1"  | >1.2`  |  Usando os operadores de comparação, você pode especificar intervalos de versões válidas. Operadores válidos são:     `>`, `>=`, `<`,`<=`,`!=`   .Você pode definir múltiplos intervalos, separados por vírgula,que serão tratados como o **operador lógico AND**  | O operador    `    será tratado como o **operador lógico OR**. Lembrando que o Operador AND possui precedência com relação ao OR.  |
| Curinga (wildcard)  |    `1.0.*`      | Você pode definir um padrão com o `*`. `1.0.*` equivale a `>=1.0,<1.1` |
| Operador til (~)  |     `~1.2`      |   Muito útil para projetos que seguem o versionamento semântico. `~1.2` é equivalente a    `>=1.2,<2.0`    . Para maiores detalhes, leia a próxima seção.   |


###Próximo release significativo (operador ~).

O operador til (`~`) é melhor explicado através de um exemplo: `~1.2` é equivalente a `>=1.2,<2.0`, enquanto `~1.2.3` é equivalente a `>=1.2.3,<1.3`. Como você pode ver, é mais útil para projetos que respeitam o versionamento semântico. Um uso comum seria para marcar a menor versão secundária depentente, como `~1.2` (que permite qualquer outra versão exceto a 2.0). Uma vez que, teoricamente, não deve haver problemas de compatibilidade em versões anteriores a 2.0, o pacote irá funcionar corretamente. Uma outra outra explicação é que o `~` especifica uma versão mínima, mas permite que o último digito especificado seja incrementado.

Nota: 
Embora `2.0-beta.1` é estritamente anterior a `.2.0`, uma restrição de versão igual a `~1.2` não será instalada. Como dito anteriormente, `~1.2` significa que o dígito `x.2` pode mudar, mas o dígito `1.x` é fixo.


###Estabilidade

Por padrão apenas releases estáveis são levados em conta. Se você gostaria de trazer versões RC, *beta*, *alpha* ou *dev* de suas dependências, você pode fazer isto usando flags de escalabilidade. Para mudar isto para todos os pacotes ao invés de fazer isto por dpendência, você pode também utilizar a configuração minimum-stability.

##Instalando dependências

Para buscar as dependências definidas para o seu projeto local, execute o comando `install` do `composer.phar`.

    php composer.phar install


Isto irá buscar pela última versão do `monolog/monolog` que coincide com a restrição de versão fornecida e será baixada para o diretório `vendor`. É uma convenção colocar códigos de terceiro num diretório chamado `vendor`. No caso do monolog, ele serã baixado para o `vendor/monolog/monolog`.

Dica: Se você está usando o git em seu projeto, você vai querer adicionar o `vendor` dentro de seu `.gitignore`.Você realmente não vai querer adicionar todos os pacotes baixados no seu repositório.


Outra coisa é que o comando `install` faz é adicionar o arquivo `composer.lock` dentro da raíz do seu projeto.




##composer.lock - O arquivo de  *Lock*


Após a instalação das dependências, o Composer irá criar a lista das versões exatas instaladas dentro do arquivo composer.lock, o que fará com que o projeto seja "fechado" nessas versões específicas.

No seu controle de versão, dê um "commit" no arquivo composer.lock. Essa medida é importante pois o comando install checa se um arquivo de "lock" está presente e caso esteja, o composer irá baixar apenas as versões das dependências ali especificadas (independente do que está no arquivo composer.json).

Isto significa dizer que, qualquer um que configurar o projeto irá fazer o donwload das versões **exatas** das dependências. Sua máquina local de desenvolvimento, os servidores do produção, os outros desenvolvedores do seu time, tudo e todos rodarão o aplicativo sob as mesmas dependências, o que mitiga o potencial de bugs que afetam apenas algumas partes do desenvolvimento. E mesmo que você trabalhe sozinho, em seis meses quando precisar resintalar o projeto você pode se sentir confiante de que as dependências instaladas ainda funcionam mesmo que as dependências atuais estejam em versões maiores.

Se não houver nenhum arquivo composer.lock, o Composer irá trazer as referências de dependências (e suas versões)  direto do composer.json e irá criar o arquivo de lock.

Isto significa que se alguma das dependências estiverem numa nova versão, você não vai obter os updates automaticamente. Para atualizar para uma nova versão, use o comando upadate. Ele irá buscar a última versão de acordo com o arquivo composer.json e também irá atualizar o arquivo de lock para apontar para a nova versão.

    php composer.phar update

Se você deseja apenas instalar ou atualizar uma dependência apenas, você pode colocá-la na whitelist:

    php composer.phar update monolog/monolog [...]


Nota: para bibliotecas, não é necessáriamente recomendado dar um "commit" no arquivo de lock. Veja também: Bibliotecas - arquivo de lock


##Packagist


Packagist é o principal repositório do Composer. Um repositório do composer é basicamente uma fonte de pacotes: um lugar de onde você pode obter os pacotes. O Packagist visa ser o repositório central, aquele que todo mundo usa. Isto significa que você pode automaticamente requerer qualquer pacote disponível nele.

Se você acessar o site do Packagist (packagist.org), você pode visualizar e buscar todos os pacotes.

Qualquer projeto opensource usando o Composer deve publicar seus pacotes no packagist. Uma biblioteca não precisa estar no Packagist para ser usada pelo Composer, mas faz a vida muito mais simples.


##Autoloading


Para bibliotecas que especificam informações de autoload, o Composer gera um arquivo chamado vendor/autolload.php. Simplesmente dê um include neste arquivo e ele será carregado automaticamente.

    require 'vendor/autoload.php';

Isto faz com que um código de terceiro seja facilmente utilizado. Por exemplo: Se seu projeto depende do monolog, você pode começar usando suas classes, e elas serão carregadas automaticamente.

    $log = new Monolog\Logger('name');
    $log->pushHandler(new Monolog\Handler\StreamHandler('app.log', Monolog\Logger::WARNING));
    $log->addWarning('Foo');

Você pode inclusive adicionar seu próprio código no autoloader, adicionando um campo de autoload no composer.json: 

    {
    "autoload": {
        "psr-4": {"Acme\\": "src/"}
    }
    }

No exemplo acima, o *composer* irá registrar um autoloader PSR-4 ao namespace Acme.

Você pode definir um mapeamento (mapping) desde namespaces até diretórios. O diretório `src` estará na raiz do seu projeto, no mesmo nível que o diretório `vendor` está. Um exemplo seria o arquivo `src/Foo.php` contendo uma classe chamada Acme\Foo.

Após adicionar o campo de autoload, você deve rodar o comando de *install* novamente para que o arquivo vendor/autoload.php seja gerado de novo.

Além deste arquivo, será retornado também uma instância do *autoloader*, então você pode armazenar o valor de retorno da chamada do *include* numa variável e adicionar mais namespaces. Isto pode ser útil para fazer o autoload em várias classes, num ambiente de testes. Por exemplo:

    $loader = require 'vendor/autoload.php';
    $loader->add('Acme\\Test\\', __DIR__);

Além do autoloading ao PSR-4, o *classmap* também é suportado. Isso significa dizer que as classes podem ser carregadas automaticamente mesmo se elas não estão conforme o PSR-4. Para maiores detalhes, consulte a referência do autoload.

Nota: O Composer fornece seu próprio autoloader. Se você não deseja usá-lo, basta apenas incluir seus arquivos .php na pasta `vendor/composer/autoload_*.php` , e serão retornados arrays associativos, permitindo que você configure seu próprio autoloader.



