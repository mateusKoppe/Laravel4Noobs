## 3.2 Rotas

Agora que configuramos o nosso projeto e entendemos como funciona a sua arquitetura chegou a hora de começarmos a entender as rotas.

As rotas são como caminhos que temos nas URLs da nossa aplicação, por exemplo, no endereço _localhost:8000_**/home** a nossa rota é `/home`, se tivermos um blog e quisermos acessar algum post possívelmente o seu endereço será algo como _localhost:8000_**/posts/1**, nesse exemplo a rota é `/posts/1`.

Nós precisamos de rotas para saber qual parte da nossa aplicação o usuário quer acessar e para isso configuramos uma ação para cada rota de nossa aplicação, podemos configurar a rota `/home` para exibir a página inicial do nosso site, a rota `/produtos` para uma página com a listagem de todos os produtos e assim por diante.

Agora que sabemos o que são rotas e para o que servem chegou a hora de criar as nossas próprias, para isso abra a pasta `routes` na raiz da nossa aplicação, é nessa pasta que ficam as rotas por padrão, dentro dessa pasta teremos 4 arquivo: `api.php`, `channels.php`, `console.php` e `web.php`, cada arquivo é refêrente a um tipo de rota, por enquanto iremos abrir apenas o arquivo `routes/web.php` e ignorar os outros.

Dentro do arquivo `routes/web.php` vamos criar uma rota simples, a rota será `/teste` e ela irá apenas exibir texto simples, iremos passar uma função anônima que irá retornar esse texto como callback, para isso vamos inserir o seguinte código ao fim do arquivo:

```php
Route::get('teste', function () {
    return 'Essa é a minha primeira rota';
});
```

Acima estamos utilizando o método `get` da classe `Route` e passando dois parâmetros, o `path` da rota como 'teste' e uma função anônima que retorna uma string.

Com isso quando o usuário acessar a rota `/teste` o Laravél irá buscar esse rota, executar a função anônima e exibir o seu retorno.

Para testar essa rota é só adicionar `/teste` ao fim da URL do seu projeto: [http://localhost:8000/teste](http://localhost:8000/teste). Se tudo estiver certo você irá ver o texto "Essa é a minha primeira rota" no seu navegador. 

Com a nossa primeira rota criada vamos seguir para entender um pouco mais sobre as rotas.

### Metódos HTTP
Como podemos observar na rota anterior nós usamos o método `get`, isso é refêrente ao método HTTP `GET`, sempre que o usuário acessar uma página no navegador ele faz uma requisição do tipo `GET`, caso exista um formulário do tipo `POST` e ele envie esse formulário para alguma rota aquela requisição será do tipo `POST`, nesse caso você deverá usar o método `Route::post` para capturar essa ação.

> Um entendimento básico do protocolo HTTP vai lhe ajudar muito a entender todo esse tutorial. Caso você não saiba o que são métodos HTTP confira o artigo [Métodos de requisição HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Methods) da Mozilla para ter um entendimento sobre o assunto.

Existem 6 métodos disponíveis para configurar as rotas, sendo eles:
```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

Caso você precise criar uma rota que execute em mais de um método HTTP você pode utilizar o método `match` e se precisar que uma rota execute para todos os verbos HTTP utilize `any`:


```php
Route::match(['get', 'post'], '/', function () {
    //
});

Route::any('/', function () {
    //
});
```

### Parâmetros na URL
Eventualmente você irá precisar passar parâmetros na URL, como por exemplo o id de um produto como `/produtos/1`, a slug de um um post como `posts/aprendendo-laravel` ou qualquer tipo de parâmetro.

Para configurar uma rota que recebe um parâmetro dinâmico você deve capturar esse parâmetro com `{}`, por exemplo, para capturar o id de um produto você pode utilizar `/produtos/{id}`, você irá receber esse `id` como parâmetro na função anônima que você passa como callback:

```php
Route::get('posts/{id}', function ($id) {
    return "O post de id:" . $id;
});
```

Caso você precise de múltiplos parâmetros você irá os receber na mesma sequência que informou:

```php
Route::get('posts/{post}/comentarios/{comment}', function ($postId, $commentId) {
    return "O post de id: $id e comentário $commentId";
});
```

#### Parâmetros opcionais
Caso você precise de parâmetros opcionais basta inserir `?` após o nome do parâmetro:

```php
Route::get('usuario/{name?}', function ($name = null) {
    return $name;
});
```

Você também pode definir um valor padrão para o parâmetro:

```php
Route::get('usuario/{name?}', function ($name = "Charlie") {
    return $name;
});
```

#### Expressões regulares
Você pode definir o formato de um parâmetro com uma expressão regular usando o método `where`. O método `where` recebe o nome do parâmetro e a expressão regular em seguida:

```php
Route::get('produtos/{id}', function ($id) {
    //
})->where('id', '[0-9]+');
```

### Rotas de redirecionamento
Se você precisar de uma rota que redireciona para outra você pode utilizar o método `redirect`:
```php
Route::redirect('/se-vier-aqui', '/vai-cair-aqui');
```

Por padrão o método `redirect` retorna o status HTTP `302`, se você quiser alterar esse status você pode passar como terceiro parâmetro o novo statu:
```php
Route::redirect('/se-vier-aqui', '/vai-cair-aqui', 301);
```

Você pode usar `permanentRedirect` para redirecionamentos permanentes, esse método sempre irá retornar o status `301`:
```php
Route::permanentRedirect('/se-vier-aqui', '/vai-cair-aqui');
```

> Caso você não saiba o que são métodos HTTP você pode conferir o artigo [Códigos de status de respostas HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status) da Mozilla.

### Nomeando rotas
Por fim você pode nomear as suas rotas, isso vai facilitar futuros redirects para essas rotas:

```php
Route::get('usuario/perfil', function () {
    //
})->name('profile');
```

Assim quando você estiver em outro local do seu código e quiser redirecionar para a rota `profile` basta utilizar a função `route`:

```php
// Url será igual a URL da rota profile
$url = route('profile');

// Irá redirecionar o usuário para a rota profile
return redirect()->route('profile');
```

Caso uma rota tenha parâmetros você irá passar eles em um array como segundo parâmetro:

```php
$url = route('product', ['id' => 1]);
```

Se você passar parâmetros adicionais eles serão automaticamente convertidos para query string em sua URL:

```php
$url = route('products', ['id' => 1, 'photos' => true]);

// /produtos/1?photos=1
```