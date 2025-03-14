# Wiiki

Repositorio con los archivos de configuración y las extensiones de la Wiiki, la wiki Wixárika.

## Objetivo

El objetivo de este repositorio es permitir hacer fácilmente cambios en la configuración de la wiki y así como facilitar, en caso de ser necesaria, la migración de la misma de un servidor a otro.

## Instrucción de instalación en un nuevo servidor (o localmente)

### Crear los archivos necesarios

```sh
# En /var/www/html:

# clonar el repositorio
git clone https://github.com/Cenzontle-AC/wiiki.git .


# instalar mediawiki
curl -L https://releases.wikimedia.org/mediawiki/1.41/mediawiki-1.41.1.tar.gz | tar -xz --strip-components=1

# crear archivo .env
cp .env.example .env
```

**Importante**: Éstas instrucciones asumen que ya existe una base de datos con algunos contenidos existentes. De otro modo, será necesario que al terminar este paso se borre el archivo `LocalSettings.php`. Para dar paso al que el instalador de la wiki inicialice una base de datos.

### Configurar el ambiente (.env)

Una vez creado el archivo `.env` es necesario ingresar los valores requeridos, tales como la URL base del sitio, y los datos de conexión con la base de datos.

### Configurar la base de datos

#### Crear la base de datos y cargar los datos usando el usuario root, o algún otro con todos los privilegios suficientes

Primero es necesario subir el backup de la base de datos al servidor, esto puede realizarse via SFTP.

El archivo tendrá un nombre como `wiiki_mediawiki.sql.xz`.

Una vez subida la base de datos ingresar mediante un tunel ssh al servidor.

```sh
# En la carpet a que contiene el archivo del backup:

# descomprimir el backup
xz -d wiiki_mediawiki.sql.xz

# Crear la base de datos
mariadb --user root --password < wiiki_mediawiki.sql
```

#### Crear el usuario de la base de datos

Abrir el REPL de `mariadb`:

```sh
mariadb
```

Una vez en el REPL de `mariadb`:

```sql
CREATE USER 'miusuario'@'localhost' IDENTIFIED BY 'mipassword';

GRANT ALL PRIVILEGES ON wiiki_mediawiki.* TO  'miusuario'@'localhost' ;
```

**Importante**: Asegurarse que los datos de usuario (e.g. `miusuario`, `mipassword`) así como el nombre de la base de datos (e.g. `wiiki_mediawiki`) se correspondan con los usados en el archivo `.env`.

## Abrir la página

### En producción

Una vez realizado esto, así como habiendo configurado el archivo `.env` se puede proceder a abrir cargar la página en la URL definida en en archivo `.env` en el campo `wgServer`.

Es necesario que el DNS del dominio apunte al servidor que hostea la

### En desarrollo

Iniciar un servidor `php` con el siguiente comando:

```sh
php -S localhost:9000
```

Ingresar a http://localhost:9000.
