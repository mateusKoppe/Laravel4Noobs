# Console

## Introdução

Para criar um novo comando, você pode usar o comando do Artisan: `make:command`. 

Isto criará uma nova classe de comando no diretório `app/Console/Commands`.

#### _Comando para gerar comandos_

```bash
php artisan make:command HelloArtisan
```

## Estrutura 

Após criar a classe de Comando, devemos definir as propriedades que esse comando terá e o tipo de entrada.

O método `handle()` é o que será chamado ao executar seu comando no console.

Observe no exemplo abaixo que podemos indicar parâmetros no método handle. O container de serviço Laravel injetará automaticamente todos os parâmetros deste método.

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;

class HelloArtisan extends Command
{
    /**
     * Nome e Assinatura do seu comando (como você vai
     * chamá-lo no console)
     *
     * @var string
     */
    protected $signature = 'say:hello {name}';

    /**
     * Descrição do comando
     *
     * @var string
     */
    protected $description = 'Fala olá para o nome inserido';

    /**
     * Método que será executado
     *
     * @return void
     */
    public function handle()
    {
        $name = $this->argument('name');

        $this->info("Hello $name!");
    }
}
```

## Executando seu comando

Para executar o comando digite o comando abaixo: 
`php artisan say:hello Emmanuel`

- OBS: Mude o nome depois de say:hello para o seu e veja a mágica acontecer ao executar;

### Adicional

Para mais informações consulte a [Documentação](https://laravel.com/docs/9.x/artisan#writing-commands)
