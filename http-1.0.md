---
Date: 2025-03-08
Title: 🌐 Implemetando HTTP 1.0
Description: Explicación e implementación simple del protocolo HTTP/1.0 en Java. Una introduccion a los protocolos de transporte.
Image: portadas/http1.0.png
---

# HTTP 1.0: No es magia negra

El protocolo HTTP, como probablemente ya sabes, es un protocolo creado en 1989 para permitir la
comunicación entre distintas computadoras, sin importar el hardware o software que utilicen.

Puede que ya tengas bastante experiencia realizando peticiones web con cualquier lenguaje,
tanto en el backend como en el frontend.
Pero, ¿realmente sabes cómo funciona el protocolo HTTP por debajo?
¿Me creerías si te digo que puedes implementarlo en menos de una hora?
Y no, no me refiero a utilizar librerías como Express o la librería estándar del lenguaje de
turno, sino a implementarlo desde cero.

## ¿Qué es un protocolo?

Primero, debemos definir qué es un "protocolo".
Siempre escuchamos esa palabra, pero ¿qué significa realmente?

Un protocolo es simplemente un conjunto de reglas que tanto el emisor como el receptor conocen
y entienden, y que deben seguir al pie de la letra para poder comunicarse entre sí.

Por ejemplo, supongamos que queremos ponernos de acuerdo con nuestro compañero de piso sobre
qué comeremos hoy.
Para esto, podemos inventarnos un protocolo de comunicación entre nosotros.

> Para comunicarnos, nos escribiremos cartas, las cuales serán entregadas al destino por un perro salchicha mensajero (como todo buen servicio de correo).
>
> Además, nuestra carta debe tener el siguiente formato:

```txt
version
remitente
destinatario
elegir | aceptar | rechazar
[dia comida]
```

_Ejemplo:_

```txt
1
Elias
Marcos
elegir
lunes pizza

1
Marcos
Elias
aceptar
```

Pero, ¿qué significa todo esto?
Vamos paso a paso:
- Primero, definimos claramente cuál será el medio para escribir el mensaje (en una carta).
- Segundo, cuál será el canal por el cual se enviará dicha carta (un perro salchicha
  mensajero).
- Luego, definimos el formato que debe tener dicha carta:
    - El primer campo, como en todo buen protocolo que puede cambiar con el tiempo, es la
      versión.
      En este caso, esta es la versión 1 de nuestro protocolo de comunicación.
    - Luego siguen el remitente y el destinatario (bastante autoexplicativo).
    - Después, le sigue la acción que queremos realizar, en este caso pueden ser de tres tipos:
      elegir, aceptar o rechazar.
    - Finalmente, contamos con el último campo, el cual solo es requerido cuando la acción a
      realizar es "elegir".
      Aquí ponemos el día de la semana y la comida que deseamos comer ese día.

Este protocolo, categóricamente, es un protocolo de juguete y bastante simple, pero nos sirve
para ilustrar el concepto detrás de HTTP.

Entonces, desde ahora, para comunicarnos entre compañeros, cada uno debe mandar cartas con ese
formato.
Si algún día se nos une otra persona, dicha persona solo debe aprender a leer y escribir dicho
formato para poder comunicarse y ser parte de la discusión.

Pero, ¿esto qué tiene que ver con HTTP?
Todo.
Los protocolos de comunicación no son más que un conjunto de reglas que definen el formato, el
canal y el idioma en el que se deben mandar los mensajes.
Estos mensajes, a su vez, tienen un significado común entre el emisor y el receptor.

# Características de HTTP 1.0

Tomando como base nuestro mini protocolo, ahora podemos ver cómo está estructurado el protocolo
HTTP 1.0:
- El medio de codificación del mensaje es mediante un STRING (sí, UN SIMPLE STRING).
- El canal de enlace es mediante una conexión
  [TCP](https://es.wikipedia.org/wiki/Protocolo_de_control_de_transmisi%C3%B3n).
- Un mensaje HTTP 1.0 puede ser de "petición" o de "respuesta".
  Ambos cuentan con una ligera variación en formato.

## Mensaje de respuesta

Podemos analizar una respuesta de cualquier servidor web utilizando `curl` e imprimiendo la
petición de manera cruda o verbosa.
Si hacemos esto, podemos ver algo parecido a esto:

```http
HTTP/1.0 200 OK
Server: Apache/1.3.37
Content-Type: text/html
Content-Length: 1234
Date: Mon, 23 Oct 2023 12:00:00 GMT

<!DOCTYPE html>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        Hello, World!
    </body>
</html>
```

El primer campo es **siempre** "HTTP/<versión>" (en este caso 1.0), luego le sigue el código de
respuesta (puedes ver los códigos
[aquí](https://es.wikipedia.org/wiki/Anexo:C%C3%B3digos_de_estado_HTTP)), y luego el mensaje de
estado.

Vemos que los siguientes campos están separados por una nueva línea, pero esto no es así del
todo.
En realidad, cada nueva línea se realiza utilizando el formato de nueva línea llamado CRLF, el
cual es simplemente `\r\n`.

Cada nueva línea representa un nuevo header.
Cada header está formado por un "nombre" seguido de ":" y luego el contenido del header.
En cada header, podemos guardar la información que queramos.
De hecho, podemos colocar un header personalizado como "mi_header:
contenido personalizado", pero debemos tener en cuenta que existe un estándar de headers que la
comunidad emplea y son reconocidos por cualquier implementación.

Dentro de los headers, el más importante es el header "Content-Length", el cual nos dice el
tamaño de los datos del body en bytes.
Este header es completamente obligatorio, salvo en los casos donde el código de retorno sea 204
(No Content).

Luego de la sección de headers, la cual puede contener un número indefinido de headers, viene
el body.
Como dijimos antes, el tamaño del body está dado por el header "Content-Length", pero, para
separar el body de la sección de headers, se utiliza un _doble_ salto de línea.

Por tanto, el formato nos quedaría así:

```txt
HTTP/[Versión] [Código de estado] [Mensaje de estado]
[Header1]: [Valor1]
[Header2]: [Valor2]
...
[HeaderN]: [ValorN]

[Body] (opcional)
```

> **NOTA:** Los headers en HTTP son case-insensitive.

## Petición

Para las peticiones HTTP, lo único que cambia es el formato del primer campo, el cual contiene
primero el método (GET, POST, PUT, etc.), seguido del URL a donde va la petición.

```txt
[Método] [URI] HTTP/[Versión]
[Header1]: [Valor1]
[Header2]: [Valor2]
...
[HeaderN]: [ValorN]

[Body] (opcional)
```

Podemos notar que el protocolo HTTP es simplemente un formato para enviar mensajes, pero el
verdadero significado de la información que mandamos se lo damos nosotros como programadores.
Ninguno de los headers o los métodos como GET o POST realmente significa nada por sí solo.
Por ejemplo, cualquier programador podría hacer que en su método POST se realice la eliminación
de alguna entrada de la base de datos, o que la petición GET cree un nuevo usuario.
Pero el estándar nos dice qué cosas se realizan con cada información y header, por lo que la
comunidad sigue esas reglas.

# Implementacion en JAVA
