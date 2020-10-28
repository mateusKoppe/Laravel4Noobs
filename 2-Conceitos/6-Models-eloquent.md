## Entendendo Eloquent

## Introdução

Chegamos agora na seção do Eloquent no Laravel, o nome pode parecer meio estranho, mas se você já está habituado com o padrão MVC de desenvolvimento, você deve saber que, o Eloquent no Laravel, nada mais é do que o responsável pela criação e utilização de Models dentro de sua aplicação. Caso você nunca tenha trabalhado com MVC antes e esteja aprendendo agora com o Laravel, não se preocupe, nessa seção será explicado com precisão e eficiência tudo que você precisa saber sobre o uso de Models dentro do Laravel.

Antes de mais nada é importante que você saiba que o Laravel convencionalmente trabalha com servidores MySql, porém, é possível sim trabalhar com servidores NoSql no Laravel, mas o nosso foco aqui será o MySQL, já que ele é o mais usado e o mais tranquilo de se configurar dentro de sua aplicação.

Cada tabela no banco de dados possui uma "Model" correspondente dentro da aplicação, que é usada para interagir com essa tabela. As Models permitem consultar dados em suas tabelas, bem como inserir novos registros na tabela. Antes de começarmos a brincar com o Laravel, é de extrema importância que você se certifique-se de configurar o seu Banco de dados em:

```
config/database.php
```

Ou

```
.
+-- app
+-- bootstrap
+-- config
|	+-- ...
|	+-- database.php
|	+-- ...
+-- ...
```

Neste arquivo você irá encontrar um array chamado "connections", dentro desse array terá diversos outros arrays de configurações, inclusive, o array do MySql, que é o que usaremos aqui, nesse array, teremos diversos índices, você deve configurar os índices 'host', 'port', 'database', 'username' e o índice 'password', nos índices 'host' e 'port' você deve especificar o servidor no qual o seu banco de dados está localizado e a porta respectivamente, no índice 'database' você de especificar o nome do banco de dados que você deseja trabalhar, nos índices 'username' e 'password' é importante que você coloque os dados necessários para que o Eloquent do Laravel consiga efetuar o login em seu sistema e trabalhar com os bancos de dados corretamente, antes de mais nada, agora que já configuramos nosso arquivo de database, devemos também configurar .env (que pode ser encontrado dentro da pasta raiz do projeto), no parágrado de "DB_CONNECTION" você deve configurar da mesma maneira que configurou no arquivo config.php, assim o Laravel será capaz de interpretar perfeitamente sua conexão com o banco de dados. Agora que temos tudo pronto, que tal colocarmos a mão na massa?


## Criando Models

Você deve se lembrar que o Laravel é um framework que trabalha com o sistema MVC, e como todo bom sistema MVC, temos nossos arquivos de Models, aqui no Laravel não é diferente, os arquivos de Models se encontram na pasta `app\Models`, podemos criar nosso arquivo dentro dela, vamos começar com um exemplo simples para criação de Models, você pode simplesmente entrar na pasta de Models e criar o arquivo, porém, é importante que você siga boas práticas, então, lembre-se de sempre usar a primeira letra maiúscula e caso tenha duas palavras no nome de seu arquivo, elas devem estar juntas e cada uma deve começar com a primeira letra maíscula, por exemplo:

```
Products.php
```

Ou

```
ProductsModel.php
```

Agora que você já criou o seu arquivo, é importante que você adicione um namespace à ele e que você herde a classe "Models" do Laravel antes de começar a usar ela na sua aplicação, pois é ela quem faz a conexão com o banco de dados e te permite usar os recursos do MySql dentro do Eloquent, para fazer isso é bem simples, você pode começar colocando o namespace do seu Model e após isso usar a classe do Laravel, por exemplo:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class SuaClasse extends Model
{
	//
}
```
> Note que é importante que você extenda a classe Model do Laravel dentro da classe que você criou para que você possa usar o seu banco de dados corretamente dentro de suas funções.

Caso você queira ficar mais rápido na criação de suas Models, você pode usar o terminal dentro da pasta raiz de seu projeto Laravel, o artisan oferece uma ferramenta para criar arquivos dentro do Laravel, uma delas é a criação de Models, para testar este método, você pode utilizar em seu terminal:

```bash
php artisan make:model NomeDaModel
```


Agora é só pressionar enter e está pronto, após entrar na pasta 'Models' de sua aplicação, você nota que seu arquivo foi criado com sucesso, e após abrir o arquivo você pode ver também que o própio artisan já criou o arquivo perfeitamente do modo que você precisa.

Exemplo de como o código será apresentado para você:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class NomeDaModel extends Model
{
	//
}
```


## Configurando tabelas

Note que em nenhum momento específicamos para o Laravel qual tabela queremos utilizar nessa nossa Model. porém, isso é bem simples, dentro de sua classe principal, adicione o seguinte código:

```php

//sua classe principal abaixo
class SuaClasse extends Model
{
	protected $table = 'Nome da Tabela';
}

```

Bem simples, não é verdade? bem, este procedimento segue de diversos paramêtros necessários para especificarmos para o Laravel informações extras de nossa tabela, segue abaixo mais exemplos:


Chaves Primárias (Primary Keys)


```php

//sua classe principal abaixo
class SuaClasse extends Model
{
	protected $primaryKey = 'Nome da coluna da chave primária, normalmente é o ID';
}

```


Caso sua chave primária não for um número inteiro, é importante que você específique para o Laravel qual é o formato de sua chave primária, por exemplo:


```php

//sua classe principal abaixo
class SuaClasse extends Model
{
	protected $keyType = 'string';
}

```

Caso você não queira que a chave primária seja auto incrimentada:

```php

//sua classe principal abaixo
class SuaClasse extends Model
{
	public $incrementing = false;
}

```

Caso você não tenha nenhuma coluna de TimeStamps em sua tabela, especifique 'false' para o atributo TimeStamps, assim você não terá problemas com datas desnecessárias ou erros em suas requisições.

```php

//sua classe principal abaixo
class SuaClasse extends Model
{

	public $timestamps = false;
}

```

Você pode também pode alterar o Formato de data na sua tabela.

```php

//sua classe principal abaixo
class SuaClasse extends Model
{
	protected $dateFormat = 'Formato da data';
}

```

Dentre outros, saiba mais em [Laravel Eloquent](https://laravel.com/docs/7.x/eloquent)


Você também pode criar a sua Model junto de sua Migration, assim você automatiza e diminuí o tempo de estruturação de seu banco de dados, para fazer isso, basta inserir o "-m" após o código de criação da Model no terminal de sua aplicação, por exemplo:

```bash
php artisan make:model Models\\NomeDaModel -m 
```

E assim você já estará criando sua Model e sua Migration juntas, para te ajudar a encontrar melhor sua Migration, você deve se lembrar que elas ficam dentro de:

```
database/migrations
```

Ou

```
.
+-- app
+-- bootstrap
+-- config
+-- database
|	+-- ...
|	+-- migrations
|	+-- ...
+-- ...
```
>Obs: nota-se que a Migration que o Artisan cria tem o mesmo nome de sua Model, isso te facilita na hora de encontrar sua Migration dentro de sua aplicação.


## Recuperando dados de tabelas

Agora que você já criou sua Model corretamente e sem muitos problemas, você já pode começar a trabalhar com suas tabelas do banco de dados. É importante que você imagine toda Model do Eloquent como um recurso poderoso capaz de recuperar dados rapidamente e sem muitos problemas, por exemplo:

```php

$Products = App\Models\Products::all();

foreach ($Products as $Product) {
	echo $Product->product_name;
}

```

Com o código exemplificado acima, você pode notar que a aplicação traz uma função estática da classe Produtos presente e logo em seguida já consegue fazer o foreach e retornar o nome de cada produto, este exemplo é bem simples e com ele você pode ver o quão eficiente e prático é retornar dados de uma tabela utilizando o Eloquent.  
Agora, segue abaixo, exemplos de consultas que você pode executar usando Models no Laravel:
>obs: Os exemplos abaixo foram criados com base em uma tabela chamada 'Products' ou seja, sua classe também se chama Products, sendo assim, veja que cada exemplo segue uma instância da classe Products

```php

Products::all();
// Retorna todos os registros da tabela Products (Seria o equivalente a
// SELECT * FROM products, para o MySQL)

Products::find($id);
// Retorna todos os dados do registro da tabela Products com o $id
// especificado na busca

Products::where('product_line_id',1)->get();
// Executa uma query com parâmetros de restrição (WHERE)

Products::where('product_line_id',1)
		->orderBy('product_line_id','description')
		->take(10)
		->get();
// Executa uma query com parâmetros de restrição (WHERE e LIMIT)
// organizando pelas colunas especificadas

```

Você também pode executar consultas mais específicas e mais elaboradas:

```php

Products::where('product_line_id', 1)->count();
// Executa uma query que conta o número de registro dentro dos
// parâmetros de restrição (WHERE)

Products::where('product_line_id', 1)->sum('price');
// Executa uma query retornando a soma de todos os preços do resultado da busca

Products::where('product_line_id', 1)->avg('price');
// Executa uma query que retorna a média de preços do resultado da busca

Products::where('product_line_id', 1)->max('price');
// Executa uma query que retorna o maior preço encontrado na busca

Products::where('product_line_id', 1)->count();
// Executa uma query que retorna o menor preço encontrado na busca
// parâmetros de restrição (WHERE)

```

Note que em nenhum momento nós mostramos alguma maneira de inserir dados dentro da tabela do Banco de Dados, pois para inserirmos dados dentro de uma tabela usando Models é um processo um pouco diferente dos citados acima. Para inserir dados utilizando o Eloquent existem duas formas. Na primeira você chama a Model instânciando ela dentro de uma variavel, que esta dentro do Controller ou você também pode usar a forma 'Mass Assignment' que consiste em chamar uma função estática que cria o registro dentro da tabela passando os atributos dentro de array, segue abaixo os exemplos:


Chamando Model no Controller
```php

$products= new Products;

$products->product_line_id = $request->product_line_id ;
$products->description = $request->description;
$products->expiration_time= $request->expiration_time;
$products->price = $request->price;

$products->save();
```

Antes de passar os parâmetros via array, devemos especificar dentro da classe na qual pertece a tabela quais serão os parâmetros que vamos passar, por exemplo:

```php

class Products extends Model{


	protected $fillable = [
	    'description',
	    'expiration_time',
	    'price',
	    'product_line_id'
	];
}

```

Agora passando parâmetros via array dentro de uma função estática
```php


$data = [
'product_line_id' => request('product_line_id'),
'description' => request('description'), 
'expiration_time' => request('expiration_time'),
'price' => request('price')
];

Products::create($data);
```

O Eloquent é extremamante poderoso e eficiente quando se trata de banco de dados no Laravel, hoje você aprendeu como se cria uma Model no Laravel e como ela é importante para estruturação de seu banco de dados, neste artigo não se tem ainda um exemplo perfeito da aplicação de Models no Laravel, você pode pesquisar mais sobre o Eloquent do Laravel clicando [aqui](https://laravel.com/docs/8.x/eloquent), nessa documentação você irá aprender um pouco mais sobre o Eloquent e como ele pode te tornar um desenvolvedor Laravel mais eficiente.

E agora que você concluiu mais um capitulo do tutorial já pode dar sequencia no próxima capitulo: [Requests e Response](./7-Request-response.md)
