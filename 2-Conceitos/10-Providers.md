# Service Providers

## Introdução

Service Providers ou Provedores de Serviço, em português, são o centro de inicialização da aplicação do Laravel, aqui que todos os serviços são inicializados.

Aqui que registramos nossos Serviços, Repositórios, Listeners, Middlewares e até rotas.

No arquivo `config/app.php` existe um array com a chave `'providers'` é aqui que todos os providers são setados, por padrão o Laravel vem com vários desses serviços habilidatos. Mas não significa que serão sempre injetados nas requisições e serviços.

## Criando um Provider

Vamos criar uma diretiva que exibe a data atual para o blade. Você pode executar o seguinte código na raiz de sua aplicação Laravel:

```bash
php artisan make:provider BladeDateTimeServiceProvider
```

E você terá no pasta `app/Providers/BladeDateTimeServiceProvider.php` o seguinte código:

```php
namespace App\Providers;
 
use Illuminate\Support\ServiceProvider;
 
class BladeDateTimeServiceProvider extends ServiceProvider
{
    /**
     * Register services.
     *
     * @return void
     */
    public function register()
    {
        //
    }
 
    /**
     * Bootstrap services.
     *
     * @return void
     */
    public function boot()
    {
        //
    }
}
```

Então no método `boot()` vocẽ deve inserir o seguinte código: 

```php
public function boot() 
{
    Blade::directive('datetime', function () {
        return date('d/m/Y H:i');
    });
}
```

Agora no arquivo `config/app.php` insira no array de `'providers'` a seguinte linha: 

```php
//...
'providers' => [

    /*
    * Laravel Framework Service Providers...
    */

    //...

    /*
    * Application Service Providers...
    */

    App\Providers\BladeDateTimeServiceProvider::class, // aqui
]
// ...
```

E então no arquivo `resources/views/welcome.blade.php` insira a diretiva `@datetime` à linha 47 deste arquivo. Após rode a aplicação no seu navegador preferido utilizando sail ou o `php artisan serve`, e então veja a data atual em tela abaixo do logo do Laravel.


## Conclusão

Utilizamos dos Service Providers do Laravel para criar uma diretiva do blade que exibe a data atual, um exemplo simples, mas que conseguimos concluir que: O nosso Serviço personalizado está sendo injetado na inicialização do Laravel, adicionando essa diretiva no "core" das baldes, e então conseguiremos utilizar em qualquer lugar do código que use views.

Mas os Providers são utilizados para muitas outras coisas, um exemplo mais sólido são as injeções de dependências se você criar uma camada de Serviço ou de Repositório ao seu projeto, você precisará criar um Service Provider que injeta esses códigos na inicialização para que seu serviço/repositório fique disponível em qualquer instância da aplicação.

Se você se interessou sobre isso e quer aprofundar seus conhecimentos recomento estudar mais sobre isso nos seguintes artigos: 

- [Medium - Júnior Garcia: Injeção de Dependências com Laravel](https://itamarjunior.medium.com/inje%C3%A7%C3%A3o-de-depend%C3%AAncia-b%C3%A1sica-com-laravel-5-a1303bde854c)
- [Stackoverflow - Antonio Carlos Ribeiro: Melhores formas de se utilizar Injeção de Dependências no Laravel](https://pt.stackoverflow.com/questions/3080/melhores-formas-de-utilizar-inje%C3%A7%C3%A3o-de-depend%C3%AAncia-no-laravel)