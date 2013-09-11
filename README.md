# ZerDB

Es una clase para base de datos en PHP por [Zerquix18](http://zerquix18.com/ "Zerquix18 - Sitio oficial") para el proyecto de [TrackYourPenguin](http://trackyourpenguin.com/).

#Detalles

**Versión:** 0.1

# Uso

El uso es bastante fácil... Primero la conexión, supongamos que en todo esto la variable que usaremos será `$zerdb`, la cual será la variable definitiva de la clase. Puede ser cualquier otra.

```php
$zerdb = new zerdb("host", "usuario", "clave", "basededatos");
```

### Errores

Puede ser posible que tengamos un error en la conexión. Para ello tenemos dos métodos de comprobarlo.

``` php
$zerdb = new extraer("host", "usuario", "clave", "basededatos") or die( $zerdb->ult_err );
```

Ó comprobando con la propiedad `self::listo` que es un **bool** que comprueba si la conexión está hecha.

``` php
$zerdb = new extraer("host", "usuario", "clave", "basededatos");

if( ! $zerdb->listo )
    echo $zerdb->ult_err; //Error devuelto
    
```

### Tablas y otras propiedades

Para editar las tablas debes añadirlas a la clase para que la misma la reconozca a la hora de hacer solicitudes. Se hace de la siguiente forma:

``` php
$zerdb->tablas = array(
    "tabla1" => array(
            "columna1", "columna2"
            )
        )
    "tabla2" => array(
            "columna1", "columna2
            )
        )
    );
```

#### Ejemplo

``` php
$zerdb->tablas = array( // tabla de usuarios
    "usuarios" => array( 
         "usuario", "clave" // columnas de la tabla usuarios
    ) // si añades una coma ( , ) y añades otro array será otra tabla.
);

Y así formando un array multidimensional con todas las tablas y columnas. `$zerdb->tablas[ 'tabla' ] = array("columna1", "columna2")`si se te hace más fácil de entender.

Las tablas luego cogen posición a esa variable. Es decir, el nombre de la tabla se convierte en propiedad: `$zerdb->usuarios`

#### Otras propiedades

Para activar el tracking de errores al hacer solicutdes o conexión, cambia la propiedad `self::err_track` a **true**.

``` php
$zerdb->err_track = true;
```

Para cambiar el **charset** del MySQL usa la propiedad `self::charset`

```php

$zerdb->charset = 'utf8';
```

Guarda el último error
```
$zerdb->error
```

Guarda la última solicitud en SQL (no confundir con **query()** )

```php
$zerdb->query
```

## Funciones

Tiene varias funciones, aquí se encuentran para función...

### crear_tabla()

Crea una tabla en la base de datos

``` php
$zerdb->crear_tabla( $nombre, $datos, $add );
```

#### Argumentos
______________

**$nombre** *(string)* 

El nombre de la tabla a crear

**$datos** *(array)*

Array con los datos a entrar

**$add** *(string)*

¿Algo más que quieras añadir? Añádelo aquí como cadena.

#### Ejemplo

``` php
$crear = crear_tabla( 'usuarios', array(
"id" => "NOT NULL AUTO_INCREMENT PRIMARY KEY",
"usuario" => "varchar(300)", 
"clave" => "varchar(32)") );
``` 

Resultado:
``` sql
CREATE TABLE IF NOT EXISTS usuarios (
`id` NOT NULL AUTO_INCREMENT PRIMARY KEY,
`usuario` varchar(300),
`clave` varchar(32)
)
```

### query()

Hace una petición

#### Argumentos

**$query** *(string)*

La solicitud a ser enviada

#### Ejemplo

Selecciona los usuarios:

```php
$q = $zerdb->query("SELECT * FROM usuarios");
```
#### DEBUG

Esta función guarda los errores. Si en cualquier otra función hay un error, esta función lo guarda en la propiedad `$zerdb->error`.

Ejemplo:

```php
if( !$q ) {
  echo $zerdb->error
}
```
Y lanzará el último error devuelto por MySQL. Si `$zerdb->err_track` está activado probablemente no necesites hacerlo ya que el `mysql_query` lanzará su propio error.

### insertar() | reemplazar()

Inserta o reemplaza datos en una tabla

```
 $zerdb->insertar( $tabla, $datos, $add );
 $zerdb->reemplazar( $tabla, $datos, $add );
```
### Argumentos

**$tabla** *(string)*

El nombre de la tabla a la que se insertarán los datos.

**$datos** *(array)*

Los datos que serán insertados en forma de ```array() ```.

**add** *(string)*

¿Algo adicional? Agrégalo aquí

### Ejemplo

Como antes ya habías declarado las columnas de las tablas en `$zerdb->tablas` no necesitas añadirlos de nuevo:

```php
$insertar = $zerdb->insertar( $zerdb->usuarios, array("zerquix", "12345");
$reemplazar = $zerdb->insertar( $zerdb->usuarios, array("zerquix", "12345");
``` 

El resultado es:

``` sql
--insertar--
INSERT INTO usuarios (usuario, clave) VALUES ('zerquix', '12345')
--reemplazar--
REPLACE INTO usuarios (usuario, clave) VALUES ('zerquix', '12345')
```
Recordando que el error por si las moscas se guarda en `$zerdb->error` y la solicitud en `$zerdb->query`.

### seleccionar()

Selecciona datos de una tabla, con la clase extraer.

### Argumentos

**$tabla** *(string)*

Nombre de la tabla a la que se insertarán los datos

**$dato** *(string)*

Datos que se van a extraer, dependiendo de lo que necesites puedes poner "todo" para sacar todo o "*" o simplemente los datos separados por comas.

**$donde** *(array)*

Si por si acaso necesitas un `WHERE` puedes ponerlo en forma de array en cuantas claves quieras. Si no lo necesitas puedes dejarlo en **false** o no agregar nada ya que así está por predeterminado.

**$extra** *(string)*

¿Algo adicional? Agrégalo aquí

### Ejemplos

Hay dos formas, con la función **seleccionar()** o llamando la clase **extraer**

```php
$q = $zerdb->seleccionar( $zerdb->usuarios, "*" );
```

##### Clase extraer: 

``` php
$q = new extraer( $zerdb->usuarios, "*");
```
Ambas peticiones extraen todo de la tabla. El resultado sería:

```sql
SELECT * FROM usuarios
```

Si necesitas un **WHERE** puedes agregarlo así:

```php
$q = $zerdb->seleccionar( $zerdb->usuarios, "*", array("usuario" => "zerquix") );
```

Resultado:

```sql

SELECT * FROM usuarios WHERE usuario = 'zerquix'
```

¿Usuario y clave?

``` php
$q = new extraer( $zerdb->usuarios, "*", array("usuario" => "zerquix", "clave" => "12345" );
```

**Recordando:** Puedes usar `$zerdb->seleccionar` o extraer.

### Resultado y DEBUG

El resultado trae nuevas propiedades...

Esta guarda la solicitud

```php
$q->query
```

Esta obtiene los números seleccionados en la petición si fue correcta.

```php
$q->nums
```

Esta comprueba si hubo error

```php
$q->error
```

Esta obtiene el error.

``` php
$q->obt_err
```

Esta contiene el array de `mysql_fetch_array` 

### actualizar()

Actualiza uno o más datos en una tabla

``` php
$zerdb->actualizar( $tabla, $datos, $donde, $add ) 
```
#### Argumentos

**$tabla** *(string)*

Nombre de la tabla a insertar

**$datos **  *(array)*

Los datos a actualizar

** $donde ** *(array)*

Donde (**WHERE**) se actualizarán

** $add ** *(string)*

¿Algo adicional Agŕegalo aquí

### Ejemplo

Supongamos que actualizaremos la clave a '123' donde el usuario es 'zerquix18'.

```php
$actualizar = $zerdb->actualizar( $zerdb->usuarios, array("clave" => "123"), array("usuario" => "zerquix18") );
```

He aquí la consulta:

```sql
UPDATE FROM usuarios SET clave = "123" WHERE usuario = "zerquix18"
```
Puedes hacer lo mismo para varias actualizaciones y/o en varias tablas repitiendo el array.

```php
$actualizar = $zerdb->actualizar( $zerdb->usuarios, array("clave" => "123", "usuario" => "zerquix"), array("usuario" => "zerquix18", "clave" => "12345") );
```

Consulta:

``` sql
UPDATE FROM usuarios SET clave = "123", usuario = "zerquix" WHERE usuario = "zerquix18" AND clave = "12345"
```

**Recuerda:** No es necesario añadir un `$donde`, pero así se actualizará toda la tabla si no añades un segundo array.
### eliminar()

Elimina los datos de una tabla. 

```php
$zerdb->eliminar( $tabla, $donde, $add );
```

#### Argumentos

**$tabla** *(string)*

La tabla de la que se eliminarán los datos.

**$donde** *(array)*

El array de dónde (**WHERE**) se eliminarán los datos.

**$add** *(string)*

¿Algo más?

Ahora vamos a eliminar la columna del usuario 'zerquix18' con una muy fácil petición.

``` php
$eliminar = $zerdb->eliminar( $zerdb->usuarios, array("usuario" => "zerquix18") );
```
Resultado:

```sql
DELETE FROM usuarios WHERE usuario = 'zerquix18'
```

No es necesario añadir un **$donde** en ninguna petición, pero esto quitará el **WHERE** y afectará todas las columnas...

```php
$eliminar = $zerdb->eliminar( $zerdb->usuarios );
```

Resultado:

``` sql
DELETE FROM usuarios
```

### proteger

``` php
$zerdb->proteger( $string );

Es lo mismo que `mysql_real_escape_string()` o `addslashes` sólo que ésta depende de la conexión para saber cuál de ambas usar.

#### Ejemplo

Para seleccionar un dato desde un ID al que le fue puesto una comilla:

```php
$id = $zerdb->proteger( $_GET['id'] ); // id = '21
$sel = $zerdb->seleccionar($zerdb->usuarios, "*", array("id" => $id) );
```

La solicitud sería...

```sql
SELECT * FROM usuarios WHERE id = '\'21'
```

Evitando ataques de inyección SQL.