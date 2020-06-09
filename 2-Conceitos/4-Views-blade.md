## 3.4 View com Blade

Agora chegou a hora de criamos telas para nossa aplicação, para isso o Laravel possuí uma ótima template engine instalada por padrão chamada Blade. Blade é ótimo para criar e estender layouts, criar componentes, criar estruturas como loops, ifs e etc, tudo isso de forma simples e sem deixar o HTML estranho.

### Criando uma view
As views do seu projeto Laravel ficam por padrão na pasta `resource/views` e seguem o padrão de nomenclatura `[nome-da-view].blade.php`, então para criar uma view basta você criar uma view como `home.blade.php` e colocar o HTML nela, depois disso é só chamar no controller.

Para chamar uma view em alguma rota ou controller basta usar a função `view` como no exemplo abaixo:

```php
return view('home');
```

Ou seja, se você tiver um controller de única ação como vimos no cápitulo de [controllers](./3-Controllers.md) o código será semelhante a esse:

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

### Estruturas de controle
Por fim você vai precisar controlar a sua view utilizando condições, loops, etc.
A seguir você pode conferir exemples extremamente intuitivos de como utilizar cada uma:

#### If
Quando você precisar de um `if`, `elseif` ou `else` é com exatamente essas diretivas que você vai compor o seu template, `@if`, `@elseif` e `@else` respectivamente, e após isso para finalizar o if utilize a diretiva `@endif`:

```
@if (count($records) === 1)
    <p> Eu tenho um registro! </p>
@elseif (count($records) > 1)
    <p> Eu tenho múltiplos registros! </p>
@else
    <p> Eu não tenho nenhum registro </p>
@endif
```

Você pode utilizar a diretiva `@unless` para motivos semânticos, que significa "a menos que", na prática é um if que vai executar quando a condição for falsa, o mesmo que `if (!<condicao>)`

```html
@unless (Auth::check())
    <p>Você não está logado</p>
@endunless
```

Em adição também existem as diretivas `@isset` e `@empty` que podem ser muito convenientes e funcionam da mesmo forma que as suas respetivas funções no PHP.

```html
@isset($records)
    <p> O registro é definido e não é nulo </p>
@endisset

@empty($records)
    <p> O registro é vazio </p>
@endempty
```

#### Switch
Você pode criar um switch utilizando as diretivas `@switch`, `@case`, `@break`, `@default` e `@endswitch`

```html
@switch($i)
    @case(1)
        <p> Primeiro caso </p>
        @break

    @case(2)
        <p> Segundo caso </p>
        @break

    @default
        <p> Caso padrão </p>
@endswitch
```

#### Loops
Como já esperado existe uma boa variedade de loops que podem ser utilizados sendo eles `@for`, `@foreach`. `@forelse`, e `@while`, confira nos exemplos abaixo:

```html
@for ($i = 0; $i < 10; $i++)
    <p> O valor atual é {{ $i }} </p>
@endfor

@foreach ($users as $user)
    <p>Esse é o usuário: {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>Nenhum usuários...</p>
@endforelse

@while (true)
    <p>Estou em loop infinito.</p>
@endwhile
```

Caso você precise cancelar o pular uma etapa do loop use `@continue` ou `@break`:
```html
@foreach ($users as $user)
    @if ($user->type == 1)
        @continue
    @endif

    <li>{{ $user->name }}</li>

    @if ($user->number == 5)
        @break
    @endif
@endforeach
```

Você também pode utilizar a seguinte sintaxe:
```html
@foreach ($users as $user)
    @continue($user->type == 1)

    <li>{{ $user->name }}</li>

    @break($user->number == 5)
@endforeach
```

Dentro dos seus laços de repetição é possível utilizar a variável `$loop`, essa variável contém informações a respeito do loop:

```html
@foreach ($users as $user)
    @if ($loop->first)
        This is the first iteration.
    @endif

    @if ($loop->last)
        This is the last iteration.
    @endif

    <p>This is user {{ $user->id }}</p>
@endforeach
```

Você pode conferir mais sobre a variável `$loop` na descrição abaixo:

| Property         |	Description                                            |
| -                |-                                                          |
| $loop->index     | 	O índice do loop (começa em 0).                        |
| $loop->iteration |  	O número de voltas dado (começa em 1).                 |
| $loop->remaining |  	O número restante de iterações no loop                 |
| $loop->count     |  	O número total de items sendo iterados no loop.        |
| $loop->first     |  	Quando é a primeira iteração do loop.                  |
| $loop->last      |  	Quando é a ultima iteração do loop.                    |
| $loop->even      |  	Quando é o número de iteração é par.                   |
| $loop->odd       |  	Quando é o número de iteração é ímpar.                 |
| $loop->depth     |  	O nível the profundidade do loop.                      |
| $loop->parent    |  	O `$loop` do loop pái quando esse loop é um filho      |