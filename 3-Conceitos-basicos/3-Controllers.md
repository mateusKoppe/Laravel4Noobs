## 3.2 Controllers

Com o conceito de rotas dominado, chegou a hora de entendermos como funcionam os Controllers.

Usamos controllers para não precisar usar funções anônimas em todas as rotas, com eles podemos organizar melhor o comportamento de nossas rotas em classes.

Antes tinhamos essa rota:
```php
Route::get('/products', function () {
    return ['Produto A', 'Produto B', 'Produto C'];
});
```

Agora usando controllers poderemos resumir essa rota a:

```php
Route::get('/products', 'ProductController@index');
```

e atribuir toda a lógica disso no controller, assim conseguimos manter nosso código mais organizado.

### Criando controllers
Por padrão os controllers de aplicações Laravel ficam na pasta `App/Http/Controllers`, geralmente tem o nome em singular e tem o nome finalizado em `Controller` como por exemplo `UserController` ou `ProductController`.

Para você criar um controller basta apenas você criar uma classe seguindo as convenções acima e extender a classe `App/Http/Controllers/Controller`, com apenas isso você já tem um controller. Confira o exemplo no código abaixo:

```php
// App/Http/Controllers/ProductController.php

<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;

class ProductController extends Controller
{
    public function index()
    {
        return ['Produto A', 'Produto B', 'Produto C'];
    }
}
```

Caso você queira ser mais produtivo e prefira usar as facilidades do cli Artisan usando a função `make:controller`, basta executar o comando abaixo, com isso você irá criar um controller tal qual o exibido anteriormente:

```bash
php artisan make:controller <nome-do-controller>

#ex:

php artisan make:controller ProductController
```

E para mepear as rodas com os métodos do nosso conntroller é só usar a seguinte configuração:

```php
Route::get('/products', 'ProductController@index');
```

> Repare que estamos passando somente o nome do controller, não precisamos especificar todo o seu namespace, apenas o que vem depois de `App/Http/Controllers`, e após isso usados `@` para indicar o método que queremos mapear, com isso toda a configuração foi feita.

Agora toda vez que alguma requisição combinar com o path `/products` o método `index` do controller `ProductController` será executado.

### Controller de única ação
Caso você tenha apenas um método para um controller, como por exemplo uma página home, sobre ou algo semelhante você pode usar um controller de única ação, para isso basta usar o método `__invoke`:

```php
// App/Http/Controllers/HomeController

<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;

class HomeController extends Controller
{
    public function __invoke($id)
    {
        return view('home');
    }
}
```

E na rota é só chamar o controller:

```php
Route::get('/home', 'HomeController');
```

Você também pode usar o Artisan para facilitar sua vida com isso, para isso basta passar o parametro `--invokable`:

```bash
php artisan make:controller <nome-do-controller> --invokable

# ex:
php artisan make:controller HomeController --invokable
```

### Controller resource
Como quase todos os controllers trabalham no padrão CRUD (Create, Read, Update, Delete) o Laravel implementa um padrão de controller chamado resource que padroniza essas operações.

Com esse padrão trabalhamos com 7 métodos, sendo eles:
- Index:  
É nesse método que serão listados os items do CRUD. O seu URI é `/<path>` e o verbo HTTP é `GET`;

- Create:  
É nesse método que será exido o formuário para a criação de um novo item. O seu URI é `/<path>/create` e o verbo HTTP é `GET`;

- Store:  
É nesse método que o formuário para de criação será salvo. O seu URI é `/<path>` e o verbo HTTP é `POST`;

- View:  
É nesse método que será exibido mais informações sobre um item individualmente. O seu URI é `/<path>/<id>` e o verbo HTTP é `GET`;

- Edit:  
É nesse método que será exido o formuário para a edição de item. O seu URI é `/<path>/<id>/edit` e o verbo HTTP é `GET`;

- Update:  
É nesse método que o formuário para de atualização será salvo. O seu URI é `/<path>/<id>` e o verbo HTTP é `PUT`;

- Destroy:  
É nesse método que o item será excluido. O seu URI é `/<path>/<id>` e o verbo HTTP é `DELETE`;

E os nomes das rotas será por padrão `<nome-do-resource>.<metodo>`.

Confira a tabela abaixo pra um exemplo de um resource para Produtos:

|Verbo     | URI                 | Ação    | Nome da rota    |
|----------|---------------------|---------|-----------------|
|GET       | /products           | index   | products.index  |
|GET       | /products/create    | create  | products.create |
|POST      | /products           | store   | products.store  |
|GET       | /products/{id}      | show    | products.show   |
|GET       | /products/{id}/edit | edit    | products.edit   |
|PUT/PATCH | /products/{id}      | update  | products.update |
|DELETE    | /products/{id}      | destroy | products.destroy|

A forma mais fácil de criad um controller resource é utilizar a função `make:controller` com `--resource` do Artisan, siga o exemplo abaixo:

```bash
php artisan make:controller <nome-do-controller> --resource

# Ex:
php artisan make:controller ProductController --resource
```

E agora a melhor parte, para mapear com as rotas basta usar o método `resource` da classe `Route`:

```php
Route::resource('<path>', '<controller>');

#ex
Route::resource('products', 'ProductController');
```

E com isso já está tudo configurado.

### Controller resource parcial
### Resource aninhados
### Traduzindo URL de rota resource
