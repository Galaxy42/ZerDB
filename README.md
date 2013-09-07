# ZerDB

Es una clase para base de datos en PHP por [Zerquix18](http://zerquix18.com/ "Zerquix18 - Sitio oficial").

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

Y así formando un array multidimensional con todas las tablas y columnas. `$zerdb->tablas[ 'tabla' ] = array("columna1", "columna2")`si se te hace más fácil de entender.

#### Otras propiedades

Para activar el tracking de errores al hacer solicutdes o conexión, cambia la propiedad `self::err_track` a **true**.

``` php
$zerdb->err_track = true;
```

Para cambiar el **charset** del MySQL usa la propiedad `self::charset`

```php

$zerdb->charset = 'utf8';
```

## Funciones

Tiene varias funciones, aquí se encuentran...

### crear_tabla()

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

**Más próximamente...**