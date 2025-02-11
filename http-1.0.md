---
Date: 2025-02-11
Title: Implementando http 1.0
Description: Una simple introduccion al protocolo http1.0.
Image: http1.0.png
---

# 🌐 HTTP 1.0: No es magia negra, solo un protocolo simple  

Si alguna vez has usado un navegador web, es muy probable que hayas interactuado con HTTP sin
siquiera darte cuenta.
Pero, ¿cuántos de nosotros realmente entendemos cómo funciona este protocolo?
Spoiler:
no es magia negra, solo una serie de mensajes bien estructurados.  

Hoy quiero hablar sobre **HTTP 1.0**, una versión sencilla del protocolo, y mostrar cómo
implementar un pequeño servidor en **Java** para entenderlo desde dentro.  

---

## 🕸️ **¿Qué es HTTP 1.0?**  

HTTP 1.0 fue definido en **1996** en la RFC 1945.
A diferencia de versiones posteriores, **no soporta conexiones persistentes** por defecto.
Es decir, **por cada solicitud, se abre y cierra una conexión nueva**.  

Un mensaje HTTP 1.0 típico luce así:  

```HTTP
GET /index.html HTTP/1.0
Host: example.com
User-Agent: curl/8.0
```

El servidor responde con algo como:  

```HTTP
HTTP/1.0 200 OK
Content-Type: text/html
Content-Length: 125

<html>
    <body>
        <h1>Hola, HTTP 1.0</h1>
    </body>
</html>
```

Cada solicitud es **independiente** y el servidor **no mantiene estado** entre ellas.
Simple, ¿verdad?  

---

## 🖥️ **Implementando un servidor HTTP 1.0 en Java**  

Para ver HTTP 1.0 en acción, construí un pequeño **servidor en Java** usando `ServerSocket`.
Aquí una versión simplificada:  

```java
import java.io.*;
import java.net.*;

public class SimpleHttpServer {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(8080);
        System.out.println("Servidor HTTP 1.0 corriendo en el puerto 8080...");

        while (true) {
            Socket client = server.accept();
            handleClient(client);
        }
    }

    private static void handleClient(Socket client) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(client.getInputStream()));
        PrintWriter writer = new PrintWriter(client.getOutputStream());

        String requestLine = reader.readLine();
        System.out.println("Solicitud recibida: " + requestLine);

        writer.println("HTTP/1.0 200 OK");
        writer.println("Content-Type: text/plain");
        writer.println();
        writer.println("Hola desde un servidor HTTP 1.0 en Java!");
        writer.flush();

        client.close();
    }
}
```

Este código:
✅ Escucha en el puerto 8080.
✅ Acepta conexiones entrantes.
✅ Responde con un mensaje HTTP 1.0 válido.
✅ Cierra la conexión después de cada respuesta.  

---

## 🛠️ **Probando con cURL**  

Con el servidor corriendo, podemos hacer una solicitud con `curl`:  

```bash
curl -v http://localhost:8080
```

Salida esperada:  

```txt
> GET / HTTP/1.0
> Host: localhost
> 
< HTTP/1.0 200 OK
< Content-Type: text/plain
< 
< Hola desde un servidor HTTP 1.0 en Java!
```

🎯 **¡Y listo!
Un servidor HTTP 1.0 básico funcionando!**  

---

## 🚀 **Reflexión Final**  

Mucha gente usa HTTP todos los días sin saber cómo funciona.
Pero, como vimos, **no es magia negra**, solo un protocolo simple basado en texto.
Implementar un servidor en Java me ayudó a comprender mejor su funcionamiento, y espero que
este post te motive a hacer lo mismo.  

¿Has construido algo similar?  

#Networking #Java #HTTP #DesarrolloWeb
