## 3.3 Controllers

Com o conceito de rotas dominado, chegou a hora de entendermos como funcionam os Controllers.

Controller são usados para organizar melhor o código do projeto, com controllers é possível criar classes, métodos e isolar muito melhor o código em seu escopo, além de nos dar acesso a várias facilidades que estão listadas no decorrer desse capítulo,

Uma rota usando uma função anônima:
```php
Route::get('/products', function () {
    return ['Produto A', 'Produto B', 'Produto C'];
});
```

Aqui não parece ser um problema, é apenas uma linha, mas imagine se esse método precisar de várias linhas de lógica, o código pode ficar bem bagunçado dessa forma no meio de todas as outras rotas, por isso é uma melhor ideia usar as rotas apenas para mapear URIs e ações de controllers:

Com isso em mente, mapearemos a rota passando um array contendo o `classname` do Controller e o metódo desejado:
```php
use App\Http\Controllers\ProductController;

Route::get('/products', [ProductController::class, 'index']);
```
E com isso basta desenvolver a ação `index` dentro do controller `ProductController`.

### Criando controllers
Por padrão os controllers de aplicações Laravel ficam na pasta `App/Http/Controllers`, eles geralmente tem o nome em singular e finalizado em `Controller` como por exemplo `UserController` ou `ProductController`.

A forma mais produtivo de criar um controller é utilizando o cli Artisan e a função `make:controller`, configura o exemplo abaixo:

```bash
php artisan make:controller ProductController
```

Com isso o Laravel irá automáticamente gerar o arquivo `App/Http/Controllers/ProductController.php` com o seguinte código:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ProductController extends Controller
{
    public function index()
    {
        //...
    }
}
```

Agora toda vez que alguma requisição combinar com o path `/products` o método `index` do controller `ProductController` será executado.

### Controller de única ação
Caso você tenha apenas um método para um controller, como por exemplo uma página home, sobre ou algo semelhante você pode usar um controller de única ação, para isso basta usar o método `__invoke`:

```php
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

Neste caso se deve passar apenas o `classname` do Controller, sem necessidade de um array.

```php
use App\Htpp\Controllers\HomeController;

Route::get('/home', HomeController::class);
```

Você também pode usar o Artisan para facilitar sua vida com isso, para isso basta passar o parametro `--invokable` como no exemplo abaixo:

```bash
php artisan make:controller HomeController --invokable
```

### Controller resource
Como quase todos os controllers trabalham no padrão CRUD (Create, Read, Update, Delete) o Laravel implementa um padrão de controller chamado resource que padroniza essas operações.

Com esse padrão trabalhamos com 7 ações, sendo elas:
- Index:  
É nessa ação que serão listados os items do CRUD. O seu URI é `/<path>` e o verbo HTTP é `GET`;

- Create:  
É nessa ação que será exido o formuário para a criação de um novo item. O seu URI é `/<path>/create` e o verbo HTTP é `GET`;

- Store:  
É nessa ação que o formuário para de criação será salvo. O seu URI é `/<path>` e o verbo HTTP é `POST`;

- View:  
É nessa ação que será exibido mais informações sobre um item individualmente. O seu URI é `/<path>/<id>` e o verbo HTTP é `GET`;

- Edit:  
É nessa ação que será exido o formuário para a edição de item. O seu URI é `/<path>/<id>/edit` e o verbo HTTP é `GET`;

- Update:  
É nessa ação que o formuário para de atualização será salvo. O seu URI é `/<path>/<id>` e o verbo HTTP é `PUT`;

- Destroy:  
É nessa ação que o item será excluido. O seu URI é `/<path>/<id>` e o verbo HTTP é `DELETE`;

A forma mais fácil de criar um controller resource é utilizar a função `make:controller` com o parâmetro `--resource` do Artisan, siga o exemplo abaixo:

```bash
php artisan make:controller ProductController --resource
```

E a melhor parte, para mapear com as rotas basta usar o método `resource` da classe `Route` e todas as 7 ações serão mapeadas automáticamente:

```php
Route::resource('products', ProductController::class);
```

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

#### Controller Resource parcial
Caso você não precise de todos os métodos do resource você pode utilizara o método `only` ou `except`.

Use o método `only` para deixar explicito apenas quais métodos você quer usar:
```php
Route::resource('photos', PhotoController::class)->only([
    'index', 'show'
]);
```

O método `except` faz exatamento ao contrário, use ele para excluir os método que você não pretende usar:

```php
Route::resource('photos', PhotoController::class)->except([
    'create', 'store', 'update', 'destroy'
]);
```

#### API Resource
Está desenvolvendo uma API? Se for o caso você não vai precisar dos métodos `create` e `edit`, mas você não precisa excluir eles manualmente, quando for crir o controller com Artisan basta usar o parametro `--api`:

```bash
php artisan make:controller API/UserController --api
```

Com isso o controller criado irá implementar apenas as ações necessárias para desenvolver uma API e na rota basta usar o método `apiResource`:

```php
use App\Htpp\Controllers\API\UserController;

Route::apiResource('users', UserController::class);
```

#### Resource relacionados
Eventualmente você pode precisar definir uma rota que é filha de outra, como por exemplo comentários que pertecem a postagens, em casos como esses você vai precisar usar resource aninhados, para isso basta colocar um ponto `.` entre o nome do resource pai e o resource filho e com isso o Laravel já configurará as rotas automáticamente:

```php
Route::resource('posts.comments', PostCommentController::class);
```

Com isso as rotas geradas seguirão o seguinte padrão:

`/posts/{post}/comments/{comment}`

E agora que você concluiu mais um capitulo do tutorial já pode dar sequencia no próxima capitulo:  [Views com blade](./4-Views-blade.md)