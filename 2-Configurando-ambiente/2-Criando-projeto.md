## 2.1 Criando Projeto

O primeiro passo para começar a codar é usar o composer para criar o seu projeto Laravel e instalar suas dependências, a forma mais fácil de fazer isso é executando o comando:

```bash
composer create-project --prefer-dist laravel/laravel [nome-do-projeto]
```

Esse comando irá gerar todos a estrutura de pastas, arquivos e instalará as dependências para iniciar seu projeto.

<!--
 TODO: O conteúdo abaixo é desnecesário para instalação usando composer, mas será necessário quando clonar um projeto, o ideal seria criar uma nova sessão chamada "Iniciando em projeto já existente"
-->

Quando a instalação estiver concluída você deverá gerar uma chave para o projeto, para isso digite:

```bash
php artisan key:generate
```

> Repare que estamos `artisan` para gerar essa chave, `artisan` é uma ferramenta que nos ajudará muito em nosso projetos, para saber mais sobre ele leia: [Link sobre artisan]()

Você pode conferir a chave gerada no arquivo `.env`, esse é o arquivo que irá configurar as variáveis de ambiente do projeto.

Agora abra o arquivo `.env`, confira a chave gerada, e configure as outras variáveis de acordo com seu ambiente.

> O arquivo `.env` não é trackeado pelo git, o  que significa que toda vez que você fazer um clone de alguém projeto já existente de Laravel você precisará gerar esse arquivo, para gerar esse arquivo basta criar uma cópia do arquivo `.env.example` com o nome de `.env`.

Quando tudo estiver configurado é só botar para rodar, para isso execute o comando:

```bash
php artisan serve
```

E prontinho, você iniciou um projeto Laravel ;D