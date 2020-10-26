# Enviando E-mails com o Laravel

## Introdução
O disparo de E-mails é uma constante funcionalidade em aplicações, desde verificações de autenticação, a notificações diversas, seu uso é bastante comum. Neste tópico aprenderemos a disparar E-mails utilizando o Laravel, este que inclusive, torna a tarefa extremamente acessível e de fácil implementação 

## Serviço de Mailing
Para aplicações em produção, o indicado é a utilização de um serviço especializado de email, para evitar que seu e-mail caia em caixas de span, e demais vantages. Porém, para fins didáticos, usaremos o servidor SMPT do Google.
SMTP, ou Simple Mail Transfer Protocol, é um protocolo responsável por realizar o envio de emails.

## Configurando sua conta Gmail
A única configuração necessária em seu gmail, é permitir a opção "apps menos seguros", disponível em seu painel de configuração.
Acesse diretamente a página para realizar a configuração [clicando aqui](https://myaccount.google.com/u/0/lesssecureapps)

## Configurando variáveis de ambiente
O próximo passo é configurar suas variáveis de ambiente, no arquivo `.env` do seu projeto Laravel.
```ruby
MAIL_MAILER=smt
MAIL_HOST=smtp.gmail.com #Url do gmail smtp
MAIL_PORT=587 #Porta usada para TLS // utiliza-se a 465 para SSL
MAIL_USERNAME= #Email do usuário
MAIL_PASSWORD= #Senha do usuário
MAIL_ENCRYPTION=tls # TLS
MAIL_FROM_ADDRESS= #Remetente 
MAIL_FROM_NAME="${APP_NAME}"
```

## Criando model Mail

A etapa de configuração está pronta, o próximo passo é criar um objeto Mail, disponibilizado pelo próprio Laravel, utilizando o comando:
```
php artisan make:mail nomeDoEmail
```

seu arquivo será criado em: `app/Mail`

```php
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class nomeDoEmail extends Mailable
{
    use Queueable, SerializesModels;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->view('view.name');
    }
}
```

Com o Mail criado, nosso próximo objetivo é preenche-lo com informações, mais exatamente, utilizando uma [View](../2-Conceitos/4-Views-blade.md)

* Para visualizar seu e-mail antes de sair por aí enchendo caixas de span, utilize as [Rotas](../2-Conceitos/2-Rotas.md) para retornar seu e-mail e assim, visualizá-lo antes de enviar.

## Adicionando uma View ao Mail
Assim como você aprendeu nos tópicos anteriores, crie uma View utilizando o Blade, assim, poderemos utilizar essa View dentro do nosso e-mail:

No arquivo de View: `meuEmail.blade.php`
```php
<h1>Email</h1>
<p>Enviando meu primeiro e-mail com Laravel</p>
```
Nossa View está pronta, agora vamos linkar ela ao nosso objeto Mail. Para isso, utilizamos o médodo build do Mail, passando como parametro para a função view(), o nome da nossa View criada.

```php
    public function build()
    {
        return $this->view('view.meuEmail');
    }
```
Adicionamos agora o remetente e o destinatário:
```php
    public function build()
    {
        $this->to('destinatário');
        $this->from('remetente');
        return $this->view('view.meuEmail');
    }
```

## Enviando o E-mail
Para enviar o email, criaremos um Controller, evitando criar funções dentro das nossas rotas:

```php
class EmailController extends Controller
{
    public function send()
    {
        Mail::send(new meuEmail()); // Nome do meu objeto Mail
    }
}
```
Agora resta apenas adicionarmos uma rota para chamar o controller

* Sintaxe de rotas do Laravel 8.0
```php
Route::get('/mail', [EmailControoller::class, 'send']
```

Pronto, agora basta chamar o endPoint e nosso e-mail será enviado!
