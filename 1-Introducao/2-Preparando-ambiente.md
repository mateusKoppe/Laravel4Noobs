## 1. Preparando ambiente

### 1.1 Linux
Se você vai desenvolver seu projeto em um ambiente linux basta seguir essa sessão para preparar seu ambiente.

Primeiramente você vai precisar do [PHP](https://www.php.net/) em uma versão superior a `7.2.5` e de algumas extensões, se você estiver em uma distro baseada em Debian como Ubuntu basta você utilizar o gerênciador de pacotes `apt-get` para instalar o PHP, para isso execute em seu terminal:

```bash
sudo apt-get install -y php php-dev php-mbstring php-xml
```

> Caso a sua distro não seja baseada em Debian o comando acima não irá funcionar, você precisará utilizar o gerênciador de pacotes padrão sua distro. Caso sua distro seja baseada em Arch você irá utilizar o [Pacman](https://wiki.archlinux.org/index.php/pacman) e caso seja uma distro RedHat como Fedora você irá utilizar o [Dnf](https://fedoraproject.org/wiki/DNF).

Para conferir se o PHP está instalado é só rodar ```php -v```:
```bash
php -v

PHP 7.3.11-0ubuntu0.19.10.3 (cli) (built: Feb 12 2020 15:22:33) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.11, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.3.11-0ubuntu0.19.10.3, Copyright (c) 1999-2018, by Zend Technologies
```

Agora que o PHP está instalado vamos precisar instalar o [Composer](https://getcomposer.org/), existem várias formas de utilizar o composer, aqui nós vamos baixa-lo utilizando o gerênciador de pacotes da nossa distro da mesma forma que fizemos com o PHP:

```bash
sudo apt-get install composer
```

Para conferir se o composer foi instalado corretamente basta rodar ```composer -V```

```
composer -V

Composer 1.9.0 2019-08-02 20:55:32
```

Por fim você vai precisar de um banco de dados, nesse tutorial iremos usar [MariaDB](https://mariadb.org/) mas você pode utilizar o banco de dados de sua prefêrencia.

```bash
sudo apt install maridb-server
```

Para configurar utilize o comando:
```
sudo mysql_secure_installation
```

E por fim é só acessar o banco de dados e ver funcionando:
```
mysql -u root -p
```

E é isso aí, agora que seu ambiente está configurado é hora de seguir o turorial e seguir para a sessão: [Criando Projeto](./3-Criando-projeto.md).

### 1.2 Windows
Se você pretende usar o Windows como sistema operacional para desenvolver suas aplicações usando o framework Laravel, você antes precisa de alguns requisitos mínimos.

Primeiramente você irá precisar do [COMPOSER](https://getcomposer.org/) instalado em sua máquina, a instalação é bem simples e pode ser feita de diversas maneiras, buscando no site você fica por dentro de mais informações sobre a instalação do COMPOSER. Após a instalação do composer você pode rodar códigos em seu terminal (PowerShell Do Windos, CMD, Cmder e etc).

Existem duas maneiras de se inicializar a criação de uma aplicação Laravel, a primeira é através do própio composer em seu terminal

```bash
composer create-project --prefer-dist laravel/laravel NomeDaAplicação
```

Com este comando você consegue criar o projeto através do COMPOSER, porém, nota-se que este comando é extremamente grande e pode ser chato lembrar dele a todo momento que você quiser construir alguma aplicação usando o Laravel, pensando nisso e em diversos outros fatores, a equipe de desenvolvimento do Laravel criou uma própia CLI para o Laravel, e manena qual você pode criar e versionar projetos dira mais simples. Para isso, antes de mais nada você deve instalar essa CLI em sua máquina através do comando:

```bash
composer global require laravel/installer
```

Após o carregamento da instalação ser sucedido você terá a CLI do Laravel pronta para o uso em sua máquina, portanto agora você pode rodar o código:

```bash
laravel new NomeDaAplicação
```

E você já terá toda sua aplicação sendo construida da mesma maneira, porém, muito mais fácil não é verdade?
A CLI do Laravel te permite obter algumas informações a mais sobre sua aplicação, para saber um pouco mais sobre isso basta digitar o comando abaixo em seu terminal.

```bash
laravel -h
```

Antes de mais nada, agora que você já criou sua aplicação Laravel, você deve entrar na pasta dela para vermos como está ficando, você pode fazer isso através do comando:


```bash
cd NomeDaAplicação
```
E assim você estará dentro da pasta de sua mais nova aplicação Laravel.

Por fim, agora que já criamos nossa aplicação, que tal inciarmos ela e ver como ficou? Bem... vamos lá! existem diversas maneiras de se inicializar a sua aplicação Laravel e ver ela funcionando em sua máquina, você pode executá-la através de um servidor Apache (criado com o XAMPP por exemplo) ou até mesmo através do própio servidor nativo do PHP, utilizando o comando

```bash
php -S localhost:8000 -t public
```
>Obs: 
Você deve possuir o php instalado em sua máquina ou servidor na versão 7.2 ou superior;
Você pode utilizar também outra porta para inicializar sua aplicação, porém a mais recomendada é a porta 8000.

O Laravel também possui um modo de inicialização própio através do comando:

```bash
php artisan serve
```

Bem, agora que você inicializou o servidor, basta entrar no seu navegador e digitar na barra de endereço para navegar em:
```bash
localhost:8000
```
E você verá sua aplicação Laravel funcionando!

> Obs: 
Caso você tenha utilizado o comando 'php artisan serve' para inicializar sua aplicação, o própio terminal irá retornar a URL necessária para que você tenha acesso à sua aplicação.

### 1.1 OSX
Contribua para esse projeto escrevendo sobre como rodar o Laravel no OSX.

Agora que você tem o seu ambiente configurado já tem tudo para criar o seu projeto: [Criando o Projeto](./3-Criando-projeto.md)