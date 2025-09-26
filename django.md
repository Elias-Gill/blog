---
Date: 2024-09-21
Title: Django Notes
Description: Notas personales de como utilizar django para el desarrollo. Es mas para probar las capacidades de mi web.
---

# Articulos de lectura

- [Tutorial oficial de Django](https://docs.djangoproject.com/es/5.0/intro/)
- [Implementando roles](https://permify.co/post/rbac-in-django/)
- [Mas de roles](https://forum.djangoproject.com/t/how-to-create-custom-users-with-different-roles-types/20772)
- [FBV vs CBV](https://spookylukey.github.io/django-views-the-right-way/)
- [Code standars](https://docs.pylint.org/tutorial.html)

---

# Instalacion e inicializacion de proyecto

La cli de django se llama django-admin
[instalacion](https://docs.djangoproject.com/es/5.0/intro/install/)

## Iniciar nuevo proyecto

Para generar un nuevo proyecto se utiliza el comando:

```bash
django-admin startproject <mysite>
```

Esto generara la siguiente estructura de archivos:

- The outer mysite/ root directory is a container for your project.
  Its name doesn’t matter to Django; you can rename it to anything you like.
  manage.py:
  Una utilidad de la línea de comandos que le permite interactuar con este proyecto Django de
  diferentes formas.
- En interior del directorio `mysite/` es el propio paquete de Python para su proyecto.
  Su nombre es el nombre del paquete de Python que usted tendrá que utilizar para importar todo
  dentro de este (por ejemplo, mysite.urls).
- `mysite/__init__.py`:
  Un archivo vacío que le indica a Python que este directorio debería ser considerado como un
  paquete Python.
- `mysite/settings.py`:
  Ajustes/configuración para este proyecto Django.
  Django settings le indicará todo sobre cómo funciona la configuración.
- `mysite/urls.py`:
  Las declaraciones URL para este proyecto Django; una «tabla de contenidos» de su sitio basado
  en Django.
- `mysite/asgi.py`:
  An entry-point for ASGI-compatible web servers to serve your project.
- `mysite/wsgi.py`:
  Un punto de entrada para que los servidores web compatibles con WSGI puedan servir su
  proyecto.

El archivo `manage.py` contiene todos los comandos necesarios para nuestro proyecto con django.

---

## Creando la primera app

Ahora, cada proyecto puede contar con distintas apps, para crear mas apps se cuenta con el
comando:

```bash
python manage.py startapp <name>
```

Esto creara otro directorio nuevo que contiene nuestra nueva app.
Este directorio contiene:

- Una carpeta `migrations` que contiene las migraciones de la base de datos para nuestros
  modelos
- Un archivo `admin.py` donde se encuentra los modelos para las cuentas de administrador
- Un archivo `apps.py` con la configuracion de la app
- Un archivo `models.py` con los modelos para la base de datos
- Un archivo `views.py` con las vistas
- Un archivo `tests.py` con los tests para nuestra app

---

## Creando la primera vista

Para crear la vista podemos simplemente anadir una nueva vista en `views.py` e importarla en
`urls.py` (ubicada en el directorio de la nueva app, si no existe, crearlo).

La función `include()` permite hacer referencia a otros URLconfs.
Cada vez que Django encuentra `include()` corta cualquier parte de la URL que coincide hasta
ese punto y envía la cadena restante a la URLconf incluida para seguir el proceso.

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("project.apps.todoApp.urls"))
]
```

---

# Bases de datos y modelos

La configuracion de la base de datos y el resto de la aplicacion se encuentra dentro del
archivo `settings.py`.

Aqui podemos observar la configuracion del motor de bases de datos (por defecto sqlite3): 

```python
# Database
# https://docs.djangoproject.com/en/5.0/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Django ya viene con distintas configuraciones por defecto que necesitan de realizar las
migraciones.

> _Filosofía_
> A model is the single, definitive source of information about your data. It contains the essential 
> fields and behaviors of the data you’re storing. Django follows the DRY Principle. The goal 
> is to define your data model in one place and automatically derive things from it.
> 
> This includes the migrations - unlike in Ruby On Rails, for example, `migrations are 
> entirely derived from your models file`, and are essentially a history that Django can roll 
> through to update your database schema to match your current models.

Example models: 

```python
# polls/models.py 
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")

    def __str__(self):
        return self.question_text

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```

---

# Instalar aplicacion

Para indicar a django que nuestra aplicacion `polls` esta instalada, asi podremos correr
nuestras migraciones y generar los modelos:

```python
# poc/settings.py

INSTALLED_APPS = [
    "polls.apps.PollsConfig",
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

Ahora podemos generar las migraciones con:

```bash
python manage.py makemigrations polls
```

Luego podemos aplicar las migraciones con:

```bash
python manage.py migrate
```

Si queremos inspeccionar el sql generador por las migraciones:

```bash
$ python manage.py sqlmigrate polls 0001
```

---

# Sitios administrativos

> Filosofía
> 
> Django automatiza completamente la creación de interfaces de sitios
> administrativos para los modelos.
> 
> Django fue escrito en un entorno de sala de redacción, con una separación muy clara entre
> «los editores de contenido» y el sitio «público».
> Los administradores del sitio utilizan el sistema para agregar noticias, eventos, resultados
> deportivos, etc., y ese contenido se muestra en el sitio público.
> Django resuelve el problema de crear una interfaz unificada para los administradores del sitio
> para editar el contenido.

Para crear una cuenta de administrador:

```bash
python manage.py createsuperuser
```

Para decirle django que un modelo cuenta con una interfaz dentro de la pantalla de
administrador podemos usar lo siguiente:

```python   
# polls/admin.py
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```

NOTA:
para customizar el sitio de administrador visitar:
[https://docs.djangoproject.com/es/5.0/intro/tutorial07/](https://docs.djangoproject.com/es/5.0/intro/tutorial07/)

---

# Plantillas y forms

## Plantillas simples

Para crear nuevas plantillas de las creamos en una carpeta `templates/<app>`: 

```html
{% block content %}
    {% if latest_question_list %}
        <ul>
            {% for question in latest_question_list %}
                <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
            {% endfor %}
        </ul>
    {% else %}
        <p>No polls are available.</p>
    {% endif %}
{% endblock %}
```

> Namespace de plantillas
>
> Now we might be able to get away with putting our templates directly in polls/templates (rather
> than creating another polls subdirectory), but it would actually be a bad idea.
> Django will choose the first template it finds whose name matches, and if you had a template
> with the same name in a different application, Django would be unable to distinguish between
> them.
>
> We need to be able to point Django at the right one, and the best way to ensure this is by
> namespacing them.
> That is, by putting those templates inside another directory named for the application itself.


Para utilizarla dentro de nuestra vista:

```python
from django.template import loader

def index(request):
    latest_question_list = Question.objects.order_by("-pub_date")[:5]
    template = loader.get_template("polls/index.html")
    context = {
        "latest_question_list": latest_question_list,
    }

    return HttpResponse(template.render(context, request))
```

---

## Forms
Un ejemplo de como estructurar un formulario en django (no olvidar el `method`):

```html
<form action="{% url 'polls:vote' question.id %}" method="post">
    {% csrf_token %}
    <fieldset>
        <legend><h1>{{ question.question_text }}</h1></legend>
        {% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}
        {% for choice in question.choice_set.all %}
        <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
        <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
        {% endfor %}
    </fieldset>
    <input type="submit" value="Vote">
</form>
```

Para Utilizar el form en nuestra vista:

```python
from django.db.models import F
from django.http import HttpResponse, HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse

from .models import Choice, Question

def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST["choice"])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(
            request,
            "polls/detail.html",
            { 
                "question": question, 
                "error_message": "You didn't select a choice.", 
            },
        )
    else:
        selected_choice.votes = F("votes") + 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse("polls:results", args=(question.id,)))
```

Ahora podemos crear la vista para la respuesta de redireccion luego de realizar el POST:

```python
from django.shortcuts import get_object_or_404, render

def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, "polls/results.html", {"question": question})
```

NOTA:
como forma de seguridad para evitar las peticiones de sitios cruzados Django utiliza el token
`{% csrf_token %}` para determinar la validez de la peticion.

NOTA 2:
El metodo `F()` es parte de las llamadas a base de datos de django, para mas detalles ver
[aqui](https://docs.djangoproject.com/es/5.0/ref/models/expressions/#avoiding-race-conditions-using-f)

NOTA 3:
Django cuenta con muchos
[shorcuts](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/) para las peticiones
http, tales como `get_object_or_404()`

---

#Para las operaciones CRUD comunes es mas conveniente utilizar las vistas genericas, las cuales
son autogeneradas por django mediante el modelo, por lo cual no hace falta escribir formularios
repetitivos para realizar dichas operaciones.

Podemos modificar estas vistas de modo que utilizamos las vistas genericas:

```python
from django.db.models import F
from django.http import HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse
from django.views import generic

from .models import Choice, Question

class IndexView(generic.ListView):
    template_name = "polls/index.html"
    context_object_name = "latest_question_list"

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by("-pub_date")[:5]

class DetailView(generic.DetailView):
    model = Question
    template_name = "polls/detail.html"

class ResultsView(generic.DetailView):
    model = Question
    template_name = "polls/results.html"

def vote(request, question_id):
    # same as above, no changes needed.
    ...
```

Solo debemos heredar de las vistas genericas, las cuales pueden ser:

- `View` Clase base para todas las vistas.
- `TemplateView` Renderiza una plantilla.
- `RedirectView` Redirige a una URL específica.
- `ListView` Muestra una lista de objetos.
- `DetailView` Muestra un detalle de un objeto específico.
- `CreateView` Maneja la creación de un nuevo objeto.
- `UpdateView` Maneja la actualización de un objeto existente.
- `DeleteView` Maneja la eliminación de un objeto.
- `FormView` Maneja la presentación y procesamiento de formularios.
 
Para mas informacion https://docs.djangoproject.com/es/5.0/topics/class-based-views/

---

# Testing

Dentro del archivo `tests.py` podemos colocar nuestros tests:

```python
import datetime

from django.test import TestCase
from django.utils import timezone

from .models import Question

class QuestionModelTests(TestCase):
    def test_was_published_recently_with_future_question(self):
        """
        was_published_recently() returns False for questions whose pub_date
        is in the future.
        """
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        self.assertIs(future_question.was_published_recently(), False)
```

Para correr nuestros tests:

```bash
python manage.py test polls
```

---

# Estilos y archivos estaticos

Django viene por defecto con la aplicacion `django.contrib.staticfiles` la cual permite listar
los archivos estaticos, los cuales los definimos en la carpeta `static/`, de la misma manera
que se hace con las templates.

Por ejemplo, creamos un archivo `static/polls/style.css` y agregamos un archivo
`static/polls/images/background.png`:

```css
li a {
    color: green;
}

body {
    background: white url("images/background.png") no-repeat;
}
```

Para utilizar nuestro estilo:

```html
{% load static %}
{% extends "base.html" %}
<link rel="stylesheet" href="{% static 'polls/style.css' %}">
```

La etiqueta de plantilla `{% static %}` genera la URL absoluta de los archivos estáticos.

> Advertencia
> 
> The {% static %} template tag is not available for use in static files which aren’t generated
> by Django, like your stylesheet.
> You should always use relative paths to link your static files between each other, because then
> you can change STATIC_URL (used by the static template tag to generate its URLs) without having
> to modify a bunch of paths in your static files as well.

---

# Extensibilidad y cosas avanzadas

Como nota final DJango es tremendamente extensible, lo cual se puede realizar facilmente para
agregar aplicaciones de terceros, del mismo modo se puede empaquetar aplicaciones para
reutilizarlas y realizar aplicaciones genericas.
Para mas informacion ver:

- [Aplicaciones reusables](https://docs.djangoproject.com/es/5.0/intro/reusable-apps/)
- [Un ejemplo de extensiones de terceros](https://pypi.org/project/django-debug-toolbar/)

---

# Extensiones

## Compressor

Django Compressor es una herramienta para gestionar y optimizar archivos estáticos en proyectos
Django.

Sus principales funciones son:
- Minificación:
  Reduce el tamaño de archivos CSS y JavaScript eliminando espacios en blanco, comentarios y
  otros caracteres innecesarios.
- Concatenación:
  Combina múltiples archivos CSS o JavaScript en uno solo para reducir el número de solicitudes
  HTTP al servidor, lo que mejora el rendimiento.

```html
{% load compress %}
{% compress css %}
<link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
{% endcompress %}
```

```html
{% compress js %}
<script src="{% static 'js/script.js' %}"></script>
{% endcompress %}
```

# Extending user authentication

Para extender el sistema de autenticacion de Django, se puede utilizar:

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    bio = models.TextField(max_length=500, blank=True)
    location = models.CharField(max_length=30, blank=True)
    birth_date = models.DateField(null=True, blank=True)
```

Luego se modifica el archivo `settings.py` y ya podemos utilizar el mecanismo de Django de
autenticacion:

```python
AUTH_USER_MODEL = 'core.User'
```
