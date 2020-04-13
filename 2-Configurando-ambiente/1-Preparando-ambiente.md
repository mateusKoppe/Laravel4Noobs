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

E é isso aí, agora que seu ambiente está configurado é hora de seguir o turorial e seguir para a sessão: [Criando Projeto](./2-Criando-projeto.md).

### 1.2 Windows
Contribua para esse projeto escrevendo sobre como rodar o Laravel no Windows.

### 1.1 OSX
Contribua para esse projeto escrevendo sobre como rodar o Laravel no OSX.