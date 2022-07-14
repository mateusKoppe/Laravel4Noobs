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
    Blade::directive('datetime', function ($expression) {
        return "<?php echo ($expression)->format('m/d/Y H:i'); ?>";
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

E então no arquivo `resources/views/welcome.blade.php` insira a diretiva `@datetime` à linha 47 deste arquivo. Após rode a aplicação no seu navegador preferido, e então veja a data atual em tela.
