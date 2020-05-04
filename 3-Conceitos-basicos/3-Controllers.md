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

Para você criar um controller basta apenas você criar uma classe seguindo as convenções acima e extender a classe `App/Http/Controllers/Controller`, com apenas isso você já tem um controller. Confira um exemplo abaixo:

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

E para acessar esse controller poderiamos usar a rota:

```php
Route::get('/products', 'ProductController@index');
```

Agora toda vez que alguma requisição combinar com o path `/products` o método `index` do controller `ProductController` será executado.

### Controller de única ação
### Controller resource
### Controller resource parcial
### Resource aninhados
### Traduzindo URL de rota resource
