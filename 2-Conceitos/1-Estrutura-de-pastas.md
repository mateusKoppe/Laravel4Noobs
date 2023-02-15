## 3.1 Estrutura de pastas

Assim que criado, a estrutura inicial de um projeto Laravel é a seguinte:

```
├── app 
│   ├── Console/
│   ├── Exceptions/
│   ├── Http/
│   │   ├── Controllers/
│   │   └── Middleware/
│   ├── Models/
│   └── Providers/
├── bootstrap/
├── config/
├── database/
│   ├── factories/
│   ├── migrations/
│   └── seeds/
├── public/
├── resources/
├── routes/
├── storage/
└── tests/
```

> Você não precisa se preocupar em entender toda a estrutura por hora, conforme você for trabalhando no projeto você irá se familiarizar com a arquitetura de forma natural.

### A pasta `app`
A pasta `app` contém o toda a lógica e regras de negócio de nossa aplicação.

Dentro da pasta `app` existem inicialmente outras pastas que serão explicadas a seguir.

#### A pasta `Console`
A pasta `app/Console/` contém comandos customizáveis para a sua aplicação que você pode criar para rodar com Artisan.

Para saber como criar esses comandos leia a seção [Console](./9-Console.md)

#### A pasta `Exceptions`
A pasta `app/Exceptions` contém as `Exceptions` e `Handlers` de exceções de sua aplicação.

#### A pasta `Http`
A pasta `app/Http` é responsável por toda lógica que envolvem as Requests e Responses da sua aplicação, essa pasta contém todos os seus controllers, middlewares, form requests e semelhantes.

#### A pasta `Models`
A pasta `app/Models` contém os Models do Eloquent de sua aplicação, para mais informações consulte a seção [Models](../2-Conceitos/6-Models-eloquent.md)

#### A pasta `Provider`
A pasta `app/Provider` contém os Providers de sua aplicação, para mais informações consulte a seção [Providers](../2-Conceitos/10-Providers.md)

### A pasta `bootstrap`
Essa pastá contém os arquivos que irão iniciar a framework, além disso contém arquivos de cache da framework que servem para melhorar a sua performance.

### A pasta `config`
Como o nome já sugere, é aqui que ficarão as configurações da aplicação, invista um pouco de tempo lendo as configurações para se familiarizar um pouco mais com a aplicação.

### A pasta `database`
Assim como já sugere, essa pasta é responsável por tudo que envolve o banco de dados da sua aplicação, é aqui que ficarão as suas migrations, seus seeders e eventualmente suas factories.

Você pode entender mais sobre isso lendo a seção: [Migrations](./5-Migrations.md).

### A pasta `public`
É nessa pasta que ficam os arquivos que serão acessados publicamente, como imagens, arquivos de JavaScript, CSS.

É nessa pasta que fica o arquivo inicial `index.php`.

### A pasta `resources`
Essa pasta contém todas as suas views, o seus assets não compilados como JavaScript, SASS, LESS, e etc. O arquivos de traduções também ficam nessa pasta.

### A pasta `routes`
Aqui estarão os arquivos de configuração das rotas de sua aplicação, os 4 arquivos geralmente são `api.php`, `channels.php`, `console.php` e `web.php`.

Você pode aprender mais sobre rotas na seção: [Configurando rotas](./2-Rotas.md).

### A pasta `storage`
Essa pasta é dividida em 3 pastas: `app/`, `framework/`, `logs/`.

A pasta `storage/app` pode ser usada para salvar arquivos gerados pela sua aplicação, é aqui que geralmente ficarão uploads e arquivos do tipo.

A pasta `storage/framework` armazena arquivos gerados pela framework e arquivos de cache.

A pasta `storage/logs` contém os logs da aplicação.

### A pasta `tests`
A pasta `tests` contém os testes automatizados de sua aplicação. Inicialmente a pasta contém alguns exemplos de testes.

### Outras pastas
Além disso o Laravel também possuí outras pastas que não vem inicialmente na aplicação mas podem ser fácilmente criadas.


Agora que você já sabe para o que serve as pastas você já pode seguir para a próxima etapa do tutorial: [Configurando rotas](./2-Rotas.md)