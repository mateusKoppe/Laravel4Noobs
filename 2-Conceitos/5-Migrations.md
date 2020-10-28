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
            // A Lista de possibilidades é grande, para criação desta tabela (veja abaixo)
        });

        // Para utilização de chaves estrangeiras/relacionamento entre tabelas, pode fazer da seguinte forma.
        Schema::create('nametable2', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            
            // Declare anteriomente, o campo.
            $table->unsignedBigInteger('tabela01_id');
            // Apos isso ligue as duas tabelas, seguindo os seguintes paramêtros
            // 1. Informe o campo da sua tabela atual, equivalente
            // 2. Referencie o campo da outra tabela
            // 3. Informe de qual tabela pertence esse campo estrangeiro.
            $table->foreign('tabela01_id')->references('id')->on('nametable');

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


## Tipos suportados pelo laravel (com base na versão 7)

| Comando                                            | Descrição                                                                                                                                 |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| $table->id();                                      | Coluna equivalente  de .$table->bigIncrements('id')                                                                                                     |
| $table->foreignId('user_id');                      | Coluna equivalente  de $table->unsignedBigInteger('user_id')                                                                                           |
| $table->bigIncrements('id');                       | Coluna equivalente de $table->unsignedBigInteger(chavePrimária) de auto incremento.                                                          |
| $table->bigInteger('votes');                       | Coluna equivalente BIGINT.                                                                                                                |
| $table->binary('data');                            | Coluna equivalente a BLOB.                                                                                                                |
| $table->boolean('confirmed');                      | Coluna equivalente BOOLEAN.                                                                                                               |
| $table->char('name', 100);                         | Coluna equivalente CHAR com um tamanho.                                                                                               |
| $table->date('created_at');                        | Coluna equivalente a DATE.                                                                                                                |
| $table->dateTime('created_at', 0);                 | Coluna equivalente a DATETIME com precisão (dígitos totais).                                                                              |
| $table->dateTimeTz('created_at', 0);               | Coluna equivalente a DATETIME (com fuso horário) com precisão (dígitos totais).                                                           |
| $table->decimal('amount', 8, 2);                   | Coluna equivalente DECIMAL com precisão (dígitos totais) e escala (dígitos decimais).                                                     |
| $table->double('amount', 8, 2);                    | Coluna equivalente DUPLA com precisão (dígitos totais) e escala (dígitos decimais).                                                       |
| $table->enum('level', ['easy', 'hard']);           | Coluna equivalente ENUM.                                                                                                                  |
| $table->float('amount', 8, 2);                     | Coluna equivalente FLOAT com precisão (dígitos totais) e escala (dígitos decimais).                                                       |
| $table->geometry('positions');                     | Coluna equivalente de GEOMETRIA.                                                                                                          |
| $table->geometryCollection('positions');           | Coluna equivalente a GEOMETRYCOLLECTION.                                                                                                  |
| $table->increments('id');                          | Coluna equivalente com incremento automático de UNSIGNED INTEGER (chave primária).                                                        |
| $table->integer('votes');                          | Coluna equivalente INTEGER.                                                                                                               |
| $table->ipAddress('visitor');                      | Coluna equivalente ao endereço IP.                                                                                                        |
| $table->json('options');                           | Coluna equivalente a JSON.                                                                                                                |
| $table->jsonb('options');                          | Coluna equivalente a JSONB.                                                                                                               |
| $table->lineString('positions');                   | Coluna equivalente LINESTRING.                                                                                                            |
| $table->longText('description');                   | Coluna equivalente LONGTEXT.                                                                                                              |
| $table->macAddress('device');                      | Coluna equivalente ao endereço MAC.                                                                                                       |
| $table->mediumIncrements('id');                    | Coluna equivalente UNSIGNED MEDIUMINT (chave primária) de incremento automático.                                                          |
| $table->mediumInteger('votes');                    | Coluna equivalente MEDIUMINT.                                                                                                             |
| $table->mediumText('description');                 | Coluna equivalente MEDIUMTEXT.                                                                                                            |
| $table->multiLineString('positions');              | Coluna equivalente MULTILINESTRING.                                                                                                       |
| $table->multiPoint('positions');                   | Coluna equivalente a MULTIPOINT.                                                                                                          |
| $table->multiPolygon('positions');                 | Coluna equivalente a MULTIPOLYGON.                                                                                                        |
| $table->nullableTimestamps(0);                     | Coluna equivalente  do método.timestamps()                                                                                                              |
| $table->point('position');                         | Coluna equivalente a POINT.                                                                                                               |
| $table->polygon('positions');                      | Coluna equivalente a POLYGON.                                                                                                             |
| $table->rememberToken();                           | Adiciona uma remember_tokencoluna equivalente anulável a VARCHAR (100).                                                                   |                                                                                                               |
| $table->smallIncrements('id');                     | Coluna equivalente de SMALLINT NÃO ASSINADA (chave primária) de incremento automático.                                                        |
| $table->smallInteger('votes');                     | Coluna equivalente SMALLINT.                                                                                                              |
| $table->softDeletes('deleted_at', 0);              | Adiciona uma deleted_atcoluna equivalente TIMESTAMP anulável para exclusões dinâmicas com precisão (total de dígitos).                    |
| $table->softDeletesTz('deleted_at', 0);            | Adiciona uma deleted_atcoluna equivalente TIMESTAMP (com fuso horário) anulável para exclusões dinâmicas com precisão (total de dígitos). |
| $table->string('name', 100);                       | Coluna equivalente a VARCHAR com um tamanho.                                                                                          |
| $table->text('description');                       | Coluna equivalente a TEXTO.                                                                                                               |
| $table->time('sunrise', 0);                        | Coluna equivalente a TIME com precisão (total de dígitos).                                                                                |
| $table->timeTz('sunrise', 0);                      | Coluna equivalente a TIME (com fuso horário) com precisão (dígitos totais).                                                               |
| $table->timestamp('added_on', 0);                  | Coluna equivalente TIMESTAMP com precisão (dígitos totais).                                                                               |
| $table->timestampTz('added_on', 0);                | Coluna equivalente TIMESTAMP (com fuso horário) com precisão (dígitos totais).                                                            |
| $table->timestamps(0);                             | Adiciona colunas anuláveis created_ate updated_atequivalentes a TIMESTAMP com precisão (dígitos totais).                                  |
| $table->timestampsTz(0);                           | Adiciona colunas anuláveis created_ate updated_atequivalentes a TIMESTAMP (com fuso horário) com precisão (total de dígitos).             |
| $table->tinyIncrements('id');                      | Coluna equivalente com incremento automático UNSIGNED TINYINT (chave primária).                                                           |
| $table->tinyInteger('votes');                      | Coluna equivalente a TINYINT.                                                                                                             |
| $table->unsignedBigInteger('votes');               | Coluna equivalente BIGINT NÃO ASSINADA.                                                                                                   |
| $table->unsignedDecimal('amount', 8, 2);           | Coluna equivalente DECIMAL NÃO ASSINADA, com precisão (dígitos totais) e escala (dígitos decimais).                                       |
| $table->unsignedInteger('votes');                  | Coluna equivalente INTEGER NÃO ASSINADO.                                                                                                  |
| $table->unsignedMediumInteger('votes');            | Coluna equivalente MEDIUMINT NÃO ASSINADA.                                                                                                |
| $table->unsignedSmallInteger('votes');             | Coluna equivalente SMALLINT NÃO ASSINADA.                                                                                                 |
| $table->unsignedTinyInteger('votes');              | Coluna equivalente TINYINT NÃO ASSINADA.                                                                                                  |
| $table->uuid('id');                                | Coluna equivalente UUID.                                                                                                                  |
| $table->year('birth_year');                        | Coluna equivalente a YEAR.                                                                                                                |

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

E agora que você concluiu mais um capitulo do tutorial já pode dar sequencia no próxima capitulo: [Models com eloquent](./6-Models-eloquent.md)