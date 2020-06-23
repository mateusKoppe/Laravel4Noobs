# Estruturando o banco de dados com migrations

## _Antes de tudo_

Para absorver melhor deste conhecimento, é importante ter um conhecimento básico, sobre como funcionam SQL e banco de dados, de uma maneira geral, caso ainda tenha dificuldades com esse assunto, recomendo dar uma olhada no [SQL4noobs](https://github.com/gaviusking/SQL4Noobs), antes de continuarmos.

## Introdução

As Migrations, são utilizadas para estruturar o banco de dados, de sua aplicação laravel. Informando a estrutura de suas tabelas, permite a criação,atualização e exclusão das tabelas de forma mais simples e abstrata.

[Documentação Oficial](https://laravel.com/docs/7.x/migrations)

## Iniciando com migrations

Para criar sua primeira migração é muito simples, basta ir ao terminal e digitar o comando:

```properties
php artisan make:migration create_nametable_table
```

Após, esta ação será criado um arquivo em um caminho semelhante a este:
`database/migrations/2020_05_16_224804_create_nametable_table.php`

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateNametableTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('nametable', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('nametable');
    }
}

```

Como visto, a cima neste arquivo há dois métodos principais, os métodos `up()` e `down()`, o qual são antonimos, enquanto um constroi a tabela e realiza as alterações, o outro faz a ação contrária.

Vamos primeiro abordar o método `up()`, o qual informamos a estrutura para criação/atualização da nossa tabela.

```php

    public function up()
    {
        // Todos os parametros são "Não nulos (Not null)", por padrão.

        Schema::create('nametable', function (Blueprint $table) {
            $table->id(); // Por padrão Incremental, Não pode ser nulo, Unico
            // Poderia fazer algo também como $table->string('id')->primary()
            $table->timestamps(); // Adiciona as colunas update_at e create_at ambos do "tipo", date.
            $table->string('nome'); // Cria um varchar.
            $table->cidade('cidade')->nullable() // Um dos meus campos podem ser nulos.
            $table->string('uf',2)->nullable() // No segundo parâmetro, é informado o tamanho do campo.
            $table->decimal('preco', 8, 2); // No caso, do tipo decimal, há um paramêtro extra o qual informa a escala deste número.
            $table->enum('level', ['sim', 'não']); // Em ENUMS, é passado os valores, em vez do tamanho do campo


            // ...
            // A Lista de possibilidades é grande, para criação desta tabela.
        });
    }

```

Já o método `down()`, é destinado para desfazer as alterações realizadas pelo métodos `up()`, o qual o exemplo, abaixo mostra:

```php
    public function down()
    {
        Schema::dropIfExists('nametable');
    }
```

Neste exemplo o método `down()`, deleta a tabela, construida anteriormente.

## Executando migrações

Após escrevermos, para executarmos as intruções descritas no método `up()`, é necessário, executarmos o seguinte comando:

```properties
# Executa o lote de migrações pedentes
php artisan migrate
```

E para revertemos as alterações feitas no banco de dados ou seja executarmos o método `down()`, basta executar o comando:

```properties
# Reverte o último lote
php artisan migrate:rollback
```

Caso queira reverter todas as suas migrações e roda-lás em seguida, basta executar o comando:

```properties
php artisan migrate:refresh
```

Caso queira limpar a sua base dados novamente e excluir as informações existentes sobre migrações, basta executar o comando:

```properties
php artisan migrate:reset
```

### Como assim "lotes" ?

Uma migração, idealmente, é executada apenas uma vez na mesma base de dados, o laravel guarda com si as migrações já executadas, e a ignora
para as próximas execuções.

Então quando rodamos o comando, `php artisan migrate`, por exemplo, estamos rodando apenas um lote com as migrações "pedentes".

Caso queira rodar uma migração novamente é necessário executar os comandos `php artisan migrate:rollback` ou `php artisan migrate:reset`

E agora que você concluiu mais um capitulo do tutorial já pode dar sequencia no próxima capitulo: [Models com eloquent](./6-models-eloquent.md)
