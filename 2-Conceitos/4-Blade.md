## 3.4 View com Blade

Agora chegou a hora de criamos telas para nossa aplicação, para isso o Laravel possuí uma ótima template engine instalada por padrão chamada Blade. Blade é ótimo para criar e estender layouts, criar componentes, criar estruturas como loops, ifs e etc, tudo isso de forma simples e sem deixar o HTML estranho.

### Criando uma view
As views do seu projeto Laravel ficam por padrão na pasta `resource/views` e seguem o padrão de nomenclatura `[nome-da-view].blade.php`, então para criar uma view basta você criar uma view como `home.blade.php` e colocar o HTML nela, depois disso é só chamar no controller.

Para chamar uma view em alguma rota ou controller basta usar a função `view` como no exemplo abaixo:

```php
return view('home');
```

Ou seja, se você tiver um controller de única ação como vimos no cápitulo de [controllers](./3-Controllers.md) o código será semelhante com isso:

```php
class HomeController extends Controller
{
    public function __invoke()
    {
        return view('home'); // resource/views/home.blade.php
    }
}
```

### Exibindo valores
Agora que você já sabe como chamar o a sua view é só começar a exibir valores dinâmicos.

O primeiro passo é passar valores para a view, para isso basta passar como segundo argumento na função `view` um array no formato de dicionário, como no exemplo abaixo:
```php
return view('home', [
    'name' => 'John'
]);
```

Para imprimir valores o Blade trabalha com um operador que você já pode estar familiarizado que é o `{{ }}`. Então você pode imprimir a variável `name` passada para a view da seguinte forma:

```html
<p> Olá {{ $name }}. </p>
```

Isso irá imprimir:

```html
<p> Olá John. </p>
```

### Templates
Um dos principais benefícios de usar Blade é a possíbilidade de criar templates para herança. Com isso você pode montar um template principal e estender ele em outros templates trocando apenas o conteúdo da página e evitando repetição de código, confira o exemplo abaixo:

```html
<!-- resources/views/layouts/app.blade.php -->

<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```

Observe no template o uso de `@yield`, marcadores como esse com inicias em `@` são chamados directivas.

A directiva `@yield` é usada para determinar onde uma sessão será exibida, você vai usar a directiva `@section` para isso inserir o conteúdo nessa sessão.

Para estender um template utilize a directiva `@extends`:

```html
<!-- resources/views/child.blade.php -->

@extends('layouts.app')

@section('title', 'Título da página')

@section('content')
    <p>Esse é o conteúdo da minha página.</p>
@endsection
```

