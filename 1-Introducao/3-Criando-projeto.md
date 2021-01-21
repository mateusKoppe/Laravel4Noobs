## 2.1 Criando Projeto

O primeiro passo para começar a codar é usar o composer para criar o seu projeto Laravel e instalar suas dependências, a forma mais fácil de fazer isso é executando o comando:

```bash
composer create-project --prefer-dist laravel/laravel [nome-do-projeto]
```

Esse comando irá gerar todos a estrutura de pastas, arquivos, instalará as dependências e irá gerar uma pré-configuração de ambiente para iniciar seu projeto.

Certifique-se de que você possui o arquivo `.env` e de que a variável `APP_KEY` foi preenchida.

Caso o arquivo `.env` não exista você precisará criar-lo manualmente, para isso basta copiar o arquivo `.env.example` para o novo arquivo `.env`.

Você pode fazer isso da forma que se sentir mais confortável ou utilizando o comando `cp` do padrão unix:
```bash
cp .env.example .env
```

> O arquivo `.env` não é trackeado pelo git, o  que significa que toda vez que você fazer um clone de algum projeto já existente de Laravel você precisará gerar esse arquivo com base no arquivo `.env.example` como citado acima.

Caso você tenha criado um novo arquivo `.env` ou percebeu que o `.env` gerado não possúi a variável `APP_KEY` você terá que gerar uma nova `APP_KEY`, para isso a forma mais indicada é executando o seguinte comando:

```bash
php artisan key:generate
```

> Repare que estamos utilizando `artisan` para gerar essa chave, `artisan` é uma ferramenta que nos ajudará muito em nosso projetos, para saber mais sobre ele leia: [Link sobre artisan]()

Agora abra o arquivo `.env`, confira a chave gerada, e configure as outras variáveis de acordo com seu ambiente como por exemplo as variáveis de banco de dados.

Tudo configurado é só botar para rodar, para isso execute o comando:

```bash
php artisan serve
```

Por padrão o programa iniciará na porta `:8000`, agora basta acessar [http://localhost:8080](http://localhost:8080) e prontinho, você iniciou um projeto Laravel ;D

Com o seu projeto configurado você já pode aprender um pouco mais sobre o Cli do Laravel: [Entendendo Artisan](./4-Artisan.md)