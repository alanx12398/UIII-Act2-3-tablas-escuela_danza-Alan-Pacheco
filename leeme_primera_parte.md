Proyecto: Escuela de Danza

Lenguaje: Python
Framework: Django
Editor: VS Code

1. Procedimiento para crear carpeta del Proyecto

📁 Crear carpeta con el nombre:
UIII_EscuelaDanza_0777

2. Procedimiento para abrir VS Code sobre la carpeta

Abrir VS Code, luego ir a:
➡️ Archivo → Abrir carpeta → seleccionar UIII_EscuelaDanza_0777

3. Procedimiento para abrir terminal en VS Code

Presiona:

Ctrl + ñ


O ve a:
➡️ Ver → Terminal

4. Procedimiento para crear carpeta entorno virtual “.venv” desde terminal de VS Code

En la terminal escribe:

python -m venv .venv

5. Procedimiento para activar el entorno virtual

En Windows:

.venv\Scripts\activate


En Linux o Mac:

source .venv/bin/activate

6. Procedimiento para activar intérprete de Python

En VS Code:
➡️ Ctrl + Shift + P → escribir “Python: Select Interpreter” → seleccionar el entorno .venv

7. Procedimiento para instalar Django
pip install django

8. Procedimiento para crear proyecto sin duplicar carpeta

Desde la terminal (dentro de UIII_EscuelaDanza_0777):

django-admin startproject backend_EscuelaDanza .


(El punto final evita duplicar carpetas).

9. Procedimiento para ejecutar servidor en el puerto 8036
python manage.py runserver 8036

10. Procedimiento para copiar y pegar el link en el navegador

Copia y pega el enlace:

http://127.0.0.1:8036/


Verifica que el servidor esté funcionando.

11. Procedimiento para crear aplicación
python manage.py startapp app_EscuelaDanza

12. Aquí el modelo models.py

📁 Ubicación:
app_EscuelaDanza/models.py

📄 CÓDIGO COMPLETO DE MODELOS
from django.db import models

# ==========================================
# MODELO: PROFESOR
# ==========================================
    class Profesor(models.Model):
    nombre = models.CharField(max_length=50)
    apellido = models.CharField(max_length=50)
    telefono = models.CharField(max_length=15)
    correo = models.EmailField(unique=True)
    especialidad = models.CharField(max_length=50, help_text="Tipo de danza que enseña")
    fecha_contratacion = models.DateField()
    activo = models.BooleanField(default=True)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ==========================================
# MODELO: ALUMNO
# ==========================================
    class Alumno(models.Model):
    nombre = models.CharField(max_length=50)
    apellido = models.CharField(max_length=50)
    fecha_nacimiento = models.DateField()
    correo = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15)
    nivel = models.CharField(max_length=30, choices=[
        ('Principiante', 'Principiante'),
        ('Intermedio', 'Intermedio'),
        ('Avanzado', 'Avanzado')
    ])
    fecha_inscripcion = models.DateField()

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ==========================================
# MODELO: CLASE
# ==========================================
    class Clase(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField(blank=True, null=True)
    duracion = models.DurationField(help_text="Duración de la clase (hh:mm:ss)")
    horario = models.CharField(max_length=50)
    cupo_maximo = models.PositiveIntegerField()
    profesor = models.ForeignKey(Profesor, on_delete=models.CASCADE, related_name="clases")
    # Relación 1 a muchos: un profesor puede impartir varias clases

    alumnos = models.ManyToManyField(Alumno, related_name="clases")
    # Relación muchos a muchos: una clase puede tener varios alumnos y un alumno puede estar en varias clases

    def __str__(self):
        return self.nombre

12.5 Procedimiento para realizar las migraciones
python manage.py makemigrations
python manage.py migrate

13. Primero trabajamos con el MODELO: PROFESOR

Se crean las vistas (views) y plantillas (templates) para gestionar CRUD de profesores.

14. En views de app_EscuelaDanza crear las funciones con sus códigos correspondientes

Funciones:

inicio_danza
agregar_profesor
actualizar_profesor
realizar_actualizacion_profesor
borrar_profesor

15. Crear la carpeta “templates” dentro de “app_EscuelaDanza”

📁 app_EscuelaDanza/templates/

16. En la carpeta templates crear los archivos HTML
base.html
header.html
navbar.html
footer.html
inicio.html

17. En el archivo base.html

Agregar Bootstrap para CSS y JS.

18. En el archivo navbar.html incluir las opciones

“Sistema de Administración Escuela de Danza”

“Inicio”

“Profesores” con submenú:
(Agregar Profesor, Ver Profesores, Actualizar Profesor, Borrar Profesor)

“Alumnos” con submenú:
(Agregar Alumno, Ver Alumnos, Actualizar Alumno, Borrar Alumno)

“Clases” con submenú:
(Agregar Clase, Ver Clases, Actualizar Clase, Borrar Clase)

Agregar íconos a las opciones principales (no en submenús).

19. En el archivo footer.html incluir

Derechos de autor, fecha del sistema y el texto:
“Creado por Ing. Eliseo Nava, Cbtis 128”
Mantenerlo fijo al final de la página.

20. En el archivo inicio.html

Colocar información sobre la escuela de danza e incluir una imagen tomada de Internet.

21. Crear la subcarpeta “profesor” dentro de templates

📁 app_EscuelaDanza/templates/profesor/

22. Crear los archivos HTML correspondientes
agregar_profesor.html
ver_profesores.html      (mostrar en tabla con botones Ver, Editar, Borrar)
actualizar_profesor.html
borrar_profesor.html


Más adelante se crearán los mismos para alumno y clase.

23. No utilizar forms.py
24. Procedimiento para crear el archivo urls.py en app_EscuelaDanza

Archivo:
📄 app_EscuelaDanza/urls.py

Enlazar las funciones de views.py para las operaciones CRUD de profesores.

25. Procedimiento para agregar app_EscuelaDanza en settings.py

En backend_EscuelaDanza/settings.py, agregar dentro de INSTALLED_APPS:

'app_EscuelaDanza',

26. Realizar las configuraciones correspondientes a urls.py

En backend_EscuelaDanza/urls.py agregar:

from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_EscuelaDanza.urls')),
]

27. Procedimiento para registrar los modelos en admin.py

Archivo:
📄 app_EscuelaDanza/admin.py

from django.contrib import admin
from .models import Profesor, Alumno, Clase

admin.site.register(Profesor)
admin.site.register(Alumno)
admin.site.register(Clase)


Volver a ejecutar migraciones si es necesario:

python manage.py makemigrations
python manage.py migrate

28. Utilizar colores suaves, atractivos y modernos

Las páginas HTML deben tener un diseño limpio y agradable, usando Bootstrap.

29. No validar entrada de datos
30. Al inicio crear la estructura completa de carpetas y archivos
31. Proyecto totalmente funcional
32. Finalmente ejecutar el servidor en el puerto 8036
python manage.py runserver 8036


Luego abrir en el navegador:
➡️ http://127.0.0.1:8036/

UIII_EscuelaDanza_0777/
│
├── .venv/                        ← Carpeta del entorno virtual
│
├── backend_EscuelaDanza/         ← Proyecto principal Django
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py               ← Configuraciones generales del proyecto
│   ├── urls.py                   ← Enlaces globales del proyecto
│   ├── wsgi.py
│   └── manage.py                 ← Archivo principal para ejecutar comandos Django
│
├── app_EscuelaDanza/             ← Aplicación principal de la escuela de danza
│   ├── __init__.py
│   ├── admin.py                  ← Registro de modelos para el panel de administración
│   ├── apps.py
│   ├── models.py                 ← Contiene los modelos: Profesor, Alumno, Clase
│   ├── tests.py
│   ├── urls.py                   ← Rutas propias de la aplicación
│   ├── views.py                  ← Funciones o vistas de la aplicación
│   │
│   ├── migrations/               ← Migraciones automáticas de Django
│   │   ├── __init__.py
│   │
│   └── templates/                ← Carpeta para los archivos HTML
│       ├── base.html             ← Plantilla base con Bootstrap
│       ├── header.html           ← Encabezado (logo o título)
│       ├── navbar.html           ← Barra de navegación
│       ├── footer.html           ← Pie de página con créditos
│       ├── inicio.html           ← Página principal del sistema
│       │
│       ├── profesor/             ← Carpeta para vistas del modelo Profesor
│       │   ├── agregar_profesor.html
│       │   ├── ver_profesores.html
│       │   ├── actualizar_profesor.html
│       │   └── borrar_profesor.html
│       │
│       ├── alumno/               ← Carpeta para vistas del modelo Alumno
│       │   ├── agregar_alumno.html
│       │   ├── ver_alumnos.html
│       │   ├── actualizar_alumno.html
│       │   └── borrar_alumno.html
│       │
│       └── clase/                ← Carpeta para vistas del modelo Clase
│           ├── agregar_clase.html
│           ├── ver_clases.html
│           ├── actualizar_clase.html
│           └── borrar_clase.html
│
└── requirements.txt              ← (opcional) lista de dependencias instaladas

