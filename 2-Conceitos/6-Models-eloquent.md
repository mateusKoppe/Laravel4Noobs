## 2.6 Models e Eloquent

Eloquent é o ORM (Mapeamento objeto-relacional) do Laravel, ele vai facilitar muito todas ações para manipular registros no banco de dados, como buscas, inserções, exclusões, etc. Cada tabela do banco de dados tem um model correspondente.

Por padrão os models ficam na diretamente na pasta `App`, não existe algo como uma pastas `App/models`, caso você sinta a necessidade você pode criar uma pasta para armazenar os seus models.

Você pode criar um model utilizando o Artisan, para isso basta usar o comando `make`:

```bash
php artisan make:model Flight
```

Junto com o model você também pode criar outros arquivos que você pode precisar, como um controller, migration ou factory, para isso é só passar os parâmetros que você precisa no momento de criar o model com artisan:

| Parâmetros       | Descrição                                                                    |
| ---------------- | ---------------------------------------------------------------------------- |
| -c, --controller | Cria um novo controller para o model                                         |
| -r, --resource   | Indica que o controller deve ser um controller resource                      |
| -f, --factory    | Cria uma nova factory para o model                                           |
| -m, --migration  | Cria uma migration para o model                                              |
| -a, --all        | Generate a migration, factory, and resource controller for the model         |


```bash
# Cria um model com uma migration
php artisan make:model Flight --migration
# Cria um model, um controller resource
php artisan make:model Book -cr
# Cria um model, um controller e uma migration
php artisan make:model Book --controller --migration
# Cria um model, um controller resource, uma migration e uma factory
php artisan make:model Book -a
```

E com isso assim que você executar o comando ele irá criar um model semelhante com esse abaixo:

```php
# App/Book.php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Book extends Model
{
    //
}
```

E agora que você concluiu mais um capitulo do tutorial já pode dar sequencia no próxima capitulo: [Models com eloquent](./7-Request-response.md)
