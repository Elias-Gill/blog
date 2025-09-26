---
Date: 2025-09-25
Title: 📋 Operativo PoliPlanner
Description: Aprendizajes, desafíos y dolores de cabeza. Bitácora del desarrollo de PoliPlanner.
Image: /portadas/poliplanner.webp
---

# Origenes de PoliPlanner

Primero debemos comenzar hablando acerca de PoliPlanner, cuál es la razón de su existencia, por
qué fue necesario crearla y cuál es el contexto detrás del desarrollo:
Al momento de escribir este post me encuentro terminando la carrera de Ingeniería en
Informática en la FPUNA, y el problema que se presentó desde el primer semestre fue:
"cómo administrar nuestros horarios". 

Verán, en la FPUNA cada una de las materias que podemos cursar en cada semestre normalmente
cuenta con más de una sección, donde cada sección puede tener turnos en distintos horarios,
además de contar con distintos docentes que imparten la asignatura; también es necesario
mencionar que se pueden llevar materias por "adelantado" (de semestres superiores) en tanto no
existan materias correlativas que impidan dicha inscripción (además de, por supuesto,
inscribirse en materias que por algún motivo se dejaron atrás, a modo de regularizar la vida
académica).

El problema surge dado que los estudiantes queremos optar siempre por el horario que más nos
convenga y con los docentes que más nos gustan, pero esto no siempre es posible, esto dado que
hay asignaturas y secciones que colisionan unas con otras en los horarios.
La manera en que podemos ver las materias, secciones y horarios disponibles por cada semestre
es mediante una planilla Excel que la facultad proporciona en su página web.
El inconveniente es que, especialmente cerca de las fechas de inscripción y exámenes, pueden
salir varias versiones de esta planilla en pocos días.

Se estarán preguntando:
¿qué problema existe?
Bueno, realmente ninguno, pero realizar esta tarea de armar los horarios de forma manual y
comparar horarios y secciones puede ser un poco tedioso y aburrido (especialmente cuando salen
3 versiones de la planilla Excel en 4 días).
Entonces, allá por el 2016, un egresado de la universidad creó una pequeña aplicación móvil
llamada "PoliPlanner", la cual ayuda a los alumnos a realizar este proceso de armar sus
horarios de manera más sencilla (recuerden esto, será de gran relevancia).

# Llegan los problemas

Esta aplicación funcionó perfectamente durante años, aunque no sin falencias:
Primero, la aplicación no permitía tener más de un horario registrado dentro de la misma, por
lo que comparar horarios seguía siendo algo complicado de realizar.
Otro de los problemas era que el usuario debía proporcionar la planilla Excel cada vez que
quería generar un nuevo horario, lo cual dista mucho de ser ideal (especialmente porque solo
soportaba formato .xls, no .xlsx, por lo que, por alguna razón, para descargar la planilla de
la página de la universidad necesitabas hacerlo desde una computadora de escritorio).
Además, no contaba con algún mecanismo de "migración" a versiones más nuevas de la planilla.

De todos modos, siguió y sigue siendo de gran ayuda y comodidad para los estudiantes, pero
llegados al 2022 las cosas comenzaron a complicarse.
Un pequeño cambio en el formato de los nombres de las asignaturas rompió completamente el
parseado de las planillas Excel, por lo que durante un semestre entero dicha aplicación quedó
inutilizable.

# Lo puedo hacer mejor

Justo por aquel entonces yo cursaba el segundo semestre de mi carrera, y fue cuando decidí que
antes de recibirme debía hacer algo al respecto (como todo joven sin experiencia, pensaba que
podía hacerlo mejor, más rápido, con juegos de azar y mujerzuelas).

La realidad golpeó enseguida:
mi falta total de experiencia y conocimientos dejó estancada la idea durante poco más de un
año, y cuando ya contaba con los conocimientos necesarios, como el PoliPlanner original volvía
a funcionar sin problemas, pues entonces no me decidí a actuar.

Pero finalmente, antes de comenzar el último semestre de mi carrera, decidí que era hora de
cumplir esa promesa; además, venía inspirado por algunos intentos recientes de sacar algo que
funcionara mínimamente igual que el PoliPlanner original.

El primer paso sería elegir las tecnologías, pero esto fue rápido ya que venía de querer
mejorar mis habilidades en el ecosistema Java y Spring Boot, así que sería un servicio web para
poder acceder desde todas partes.
Luego comenzó el proceso de diseño de la solución, cuyo camino resultó tortuoso dado un factor
que no consideré:
_el factor humano_.

Verán, para implementar un parser de la planilla Excel, primeramente se deben tener varias
cuestiones en consideración.
La primera y más importante es que **el parser no puede fallar por nada en el mundo**.
Esto implica que debe ser resiliente ante cambios de formato de las planillas, cambios de
nombre en las celdas, eliminación de ciertos campos o formatos de datos cambiantes o
inconsistentes (como horas que aparecen algunas veces en formato "hh:mm", otras veces
"hh:mm:ss"; fechas que algunas veces cuentan con el día de la semana como "Lun 01/01/2022", y
otras veces no; y un largo etc.), por lo que diseñar una arquitectura que pueda responder de la
manera más fiable posible ante cambios drásticos fue mi principal prioridad (si quieren más
detalles técnicos pueden ver la [segunda parte](/posts/PoliPlanner_2.md) de este post).

Finalmente creía que el parseo, y a su vez la aplicación, ya eran completamente funcionales,
así que, luego de 3 semanas de desarrollo y unos cuantos días invertidos en el despliegue,
decidí compartirlo en los grupos en los que participaba dentro de mi universidad, en todo
momento pensando "esto no lo va a usar nadie".

La sorpresa que recibí al mirar los registros de usuarios y las métricas del servidor.
Tan solo unos pocos días luego de que compartí la web con mis conocidos, la cantidad de
usuarios ya era superior a **1200** y los picos de requests eran de hasta **300 requests** por
minuto.

Dada la longitud de este post, es mejor que continuemos en la
[tercera parte](/posts/PoliPlanner_3.md), donde veremos cómo mi billetera de estudiante corrió
riesgo por unos días.
