# Middlewares

## O que são Middlewares?
De forma resumida, middleware é uma estrutura posicionada intermediária a outras estruturas, ou seja, liga essas estruturas, permanecendo entre elas. No caso do Laravel, entre requisições do protocolo HTTP.
O middleware se posicionará entre as chamadas de requisições, podendo interceptar a comunicação e chamando determinadas ações em sua aplicação.
```
Requisição -> Middleware -> Requisção/Resposta
```

## Exemplo de Utilização
Ok, agora que você entendeu o conceito por trás dos middlewares, deve estar se perguntando, qual a utilizade disso no Laravel?
Existem diversos casos de aplicação de middlewares, desde autenticação a validação de dados em sua aplicação, os middlewares serão muito úteis.
Como exemplo, vamos usar o middleware a seguir, encontrado na [Documentação](https://laravel.com/docs/8.x/middleware) do próprio Laravel:
```php
<?php

namespace App\Http\Middleware;

use Closure;

class CheckAge
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next) // Como parâmetros recebemos a request disparada pelo usuário, e a próxima requisição da rota
    {
        if ($request->age <= 200) { // captura o campo "age" recebido na requisição
            return redirect('home');
        }

        return $next($request); // retorna a próxima requisição definida na rota
    }
}
```

## Criando seu próprio Middleware
Para criar um middleware, utilize o comando em seu terminal:
``` properties
php artisan make:middleware MeuMiddleware
```

Seu middleware será criado no diretório:
```
app/Http/Middleware
```
```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class MeuMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle(Request $request, Closure $next)
    {
        return $next($request);
    }
}
```

## Registrando seu Middleware
Após criar o arquivo de middleware, precisamos registrá-lo em `app/Http/Kernel.php`.
Vamos adicionar nosso middleware na última linha do array `$routeMiddleware`, e dar um nome a ele.
```php
// Within App\Http\Kernel Class...

protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
    'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
    'can' => \Illuminate\Auth\Middleware\Authorize::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
    'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
    'meumiddleware' => \App\Http\Middleware\MeuMiddleware::class
];
```

## Utilizando middlewares nas rotas
Nosso middleware já está pronto, agora o incluiremos em nossas rotas:
Para saber mais sobre rotas no Laravel [clique aqui](../2-Conceitos/2-Rotas.md)

* Rota única
```php
Route::get('admin/profile', function () {
    //
})->middleware('meumiddleware');
```
```php
use App\Http\Middleware\MeuMiddleware;

Route::get('admin/profile', function () {
    //
})->middleware(MeuMiddleware::class); // chamando o middleware
```

* Grupos
```php
Route::group(['middleware' => ['apiJwt']], function () {
  // Rotas
  //...
}
```

## Ordenando middlewares
É possível específicar a ordem de execução de middlewares em `app/Http/Kernel.php` através do array `$middlewarePriority`:
```php
protected $middlewarePriority = [
    'meumiddleware' => \App\Http\Middleware\MeuMiddleware::class
    'outromiddleware' => \App\Http\Middleware\OutroMiddleware::class
];
```

Middlewares são extremamente úteis em aplicações para diversas implementações, lembrando que é possível colocar diversos middlewares em cadeia para uma mesma rota. Para mais detalhes sobre Middlewares no laravel acesse a [Documentação Oficial](https://laravel.com/docs/8.x/middleware).

