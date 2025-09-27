---
Date: 2025-03-08
Title: 🌐 Implemetando HTTP 1.0
Description: Explicación e implementación simple del protocolo HTTP/1.0 en Java. Una introduccion a los protocolos de transporte.
Image: portadas/http1.0.png
---

# HTTP 1.0: No es magia negra

El protocolo HTTP es un protocolo creado en 1989 para permitir la comunicación entre distintas
computadoras, sin importar el hardware o software que utilicen.

Puede que ya tengas bastante experiencia realizando peticiones web con cualquier lenguaje,
tanto en el backend como en el frontend.
Pero, ¿realmente sabes cómo funciona el protocolo HTTP por debajo?
¿Me creerías si te digo que puedes implementarlo en menos de una hora?
Y no, no me refiero a utilizar librerías como Express o la librería estándar del lenguaje de
turno, sino a implementarlo desde cero.

## ¿Qué es un protocolo?

Primero, debemos definir qué es un "protocolo".
Siempre escuchamos esa palabra, pero ¿qué significa realmente?

Un protocolo es simplemente un conjunto de reglas y pasos que tanto el emisor como el receptor
conocen y entienden, y que deben seguir al pie de la letra para poder comunicarse entre sí.
Esto funciona igual que cualquier comunicación en la vida real, donde existe un emisor, un
receptor, un mensaje que se quiere enviar, el canal por el cual se envía el mensaje, el
contexto de la "conversación" y un código de común entendimiento para codificar el mensaje.

Veamos las partes de una comunicación:

1. Primero debemos definir claramente el canal por el cual se enviará el mensaje (en la vida
   real serían medios físicos, como cartas, mensajes, llamadas, etc).
2. Luego debemos definir el formato (o código) que debe tener dicho mensaje.
   En la vida real este sería nuestro idioma, y del mismo modo que cada idioma tiene unas
   reglas y construcciones, cada protocolo en computación viene con su propio conjunto de
   reglas y formas de estructurar el mensaje de modo que el emisor y receptor estén de acuerdo.
3. Al igual que los elementos de la comunicación (esos que se ven en el colegio), una
   comunicación en un protocolo también cuenta con:
    - emisor, 
    - receptor 
    - y contexto.

# Características de HTTP 1.0

El protocolo HTTP 1.0 es la primera iteración del protocolo HTTP, y es la especificación más
simple y fácil de implementar, tanto que se puede hacer en poco más de una hora por cualquier
persona versada en programación.

Veamos las especificaciones de la comunicación por HTTP 1.0:

1. El canal que se utiliza para enviar los mensajes son **conexiones TCP**.
2. Para realizar una conexión TCP de por sí ya debemos proporcionar un receptor (el IP y puerto
   del receptor al cual queremos contactar) y emisor.
3. El protocolo HTTP es especial, ya que es un protocolo **sin contexto** (más específicamente
   sin estado o _stateless_), esto significa que el servidor naturalmente no guarda información
   acerca del receptor; si se quisiera mantener un contexto dentro de la comunicación (como
   sesiones de usuario, login, etc) entonces se deben implementar mecanismos externos como
   cookies o JWT (es decir, mecanismos que no están _especificados_ dentro de la especificación
   del protocolo HTTP).

## El código de comunicación. Encabezados del protocolo HTTP

La forma más fácil de visualizar cómo está estructurado un mensaje en HTTP 1.0 es usando la
herramienta `curl -i` para mostrar el resultado de las peticiones en formato "crudo".
Además, debemos primeramente notar que existen 2 formatos de encabezados de un mensaje en el
protocolo HTTP:

1. Uno para realizar una petición (request).
2. Otro para responder a una petición (response).

### Response message

Lo que siempre ocurre:
realizamos una _request_ y recibimos un _response_ por parte del server.

Si utilizamos curl para realizar una petición a un sitio web cualquiera y usamos el flag `-i`,
entonces podremos ver el mensaje de respuesta completo por parte del servidor, es decir, el
mensaje de tipo _response_:

```txt
$> curl -i --http1.0 http://example.com/

HTTP/1.0 200 OK
Date: Fri, 26 Sep 2025 15:20:00 GMT
Server: Apache/1.3.42 (Unix)
Content-Type: text/html
Content-Length: 137

<!DOCTYPE html>
<html>
  <head>
    <title>Example Domain</title>
  </head>
  <body>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents.</p>
  </body>
</html>
```

Lo primero que debemos advertir es que esto se envía como **texto plano**, es decir, _strings_
de toda la vida.
Esto hace mucho más fácil implementar nuestro propio servidor HTTP, aunque por supuesto trae
desventajas por el lado del performance del protocolo; es por eso que existen otros protocolos
que operan con headers y mensajes en arrays de bits (uno estándar bastante fácil de parsear y
entender si quieres aventurarte es el de imágenes en formato BMP).

Podemos notar que este mensaje tiene el siguiente formato, el cual se repetirá siempre que
queramos contactarnos con un servidor HTTP, dado que la especificación nos dice que así debe
ser estructurado cada mensaje:

```txt
<VERSION> <STATUS_CODE> <STATUS_MESSAGE>
<Headers>
<Body opcional>
```

Desglosando este mensaje:

1. **Línea de estado (status line)**
    * **VERSION** → versión del protocolo, en este caso `HTTP/1.0`.
    * **STATUS_CODE** → el código numérico que indica el resultado de la petición (`200`,
      `404`, `500`, etc.).
    * **STATUS_MESSAGE** → un texto corto que acompaña al código (`OK`, `Not Found`, `Internal
      Server Error`).
    * Cada línea de encabezado está marcada y separada con los siguientes caracteres:
      `\r\n`.
2. **Encabezados (headers)** Aquí se agrega metadata sobre la respuesta.
   Algunos son opcionales, pero hay ciertos headers que en la práctica resultan
   imprescindibles:
   * `Content-Length`:
     **necesario** en HTTP/1.0, ya que el cliente lo usa para saber cuántos bytes debe leer del
     body.
   * `Content-Type`:
     de existir, contiene el tipo de contenido del body (`text/html`, `application/json`,
     `image/png`, etc.).
     Ejemplo:
    ```txt
    Date: Fri, 26 Sep 2025 15:20:00 GMT
    Server: Apache/1.3.42 (Unix)
    Content-Type: text/html
    Content-Length: 137
    ```
3. **Body (opcional)** Luego de un doble salto de línea (`\r\n\r\n`), viene el contenido
   propiamente dicho:
   HTML, JSON, una imagen, etc. Para eso es que nos sirven los encabezados de Content-Length y
   Content-Type, así el servidor sabe cómo interpretar los bits del body.

> **NOTA:** Los headers en HTTP son case-insensitive.

Otra forma de ver esto es la siguiente:
```txt
HTTP/[Versión] [Código de estado] [Mensaje de estado]
[Header1]: [Valor1]
[Header2]: [Valor2]
...
[HeaderN]: [ValorN]

[Body] (opcional)
```

#### Notación extendida de mensajes

Además del formato clásico con `Content-Length`, existe otro modo de enviar el body que se
utiliza a partir de **HTTP/1.1**:
los **mensajes chunked**.
Este formato consiste en escribir la longitud del bloque en **hexadecimal**, seguida de `\r\n`,
luego el contenido de ese bloque, y repetir el proceso hasta que se envíe un bloque de tamaño
`0`.

No vamos a profundizar en ese formato aquí, pero es importante saber que existe, ya que permite
enviar respuestas de tamaño dinámico sin conocer la longitud total de antemano.

### Request message

Ahora, los mensajes de petición, es decir, los mensajes que por ejemplo un navegador mandaría a
un servidor se ven de la siguiente forma:

```txt
POST /api/users HTTP/1.0
Host: example.com
User-Agent: curl/7.85.0
Content-Type: application/json
Content-Length: 47
Accept: application/json

{
  "username": "elias",
  "email": "test@example.com"
}
```

De nuevo, la estructura que siguen los mensajes es la siguiente:

```txt
<METHOD> <PATH> <VERSION>
<Headers>
<Body opcional>
```

o de forma alternativa

```txt
[Método] [URI] HTTP/[Versión]
[Header1]: [Valor1]
[Header2]: [Valor2]
...
[HeaderN]: [ValorN]

[Body] (opcional)
```

Podemos ver que son bastante parecidos, pero varían en algunas cuestiones, como en la primera
línea la cual define el path de destino, el tipo de método que se está pidiendo, y en este caso
el campo de versión se encuentra al final del encabezado a diferencia del mensaje de
_response_. 

Y eso es todo, esas son las únicas diferencias entre los mensajes de envío y recepción del
protocolo HTTP.
¿Bastante fácil, verdad?

El protocolo HTTP no es más que un formato para estructurar y enviar mensajes, pero el
verdadero significado de la información se lo damos nosotros como programadores.
Ninguno de los headers o los métodos como GET o POST realmente significa nada por sí solo.

# Implementacion en Java

CONTINUARA ...
