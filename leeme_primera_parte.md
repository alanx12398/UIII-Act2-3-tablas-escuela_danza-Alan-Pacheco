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
base.html

<!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Escuela de Danza</title>
        
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
        <style>
            body {
                background-color: #f8f9fa;
                padding-bottom: 80px;
            }
            .navbar {
                background-color: #6f42c1;
            }
            footer {
                background-color: #d63384;
                color: white;
                position: fixed;
                bottom: 0;
                width: 100%;
                text-align: center;
                padding: 10px 0;
            }
        </style>
    </head>
    <body>
    
        {% include 'header.html' %}
        {% include 'navbar.html' %}
    
        <main class="container mt-4">
            {% block content %}
            {% endblock %}
        </main>
    
        {% include 'footer.html' %}
    
        <!-- Bootstrap JS -->
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    
    </body>
    </html>
    
    🎀 header.html
    <header class="text-center text-white py-4" style="background-color:#d63384;">
        <h1>💃 Escuela de Danza “Pasos de Arte” 🕺</h1>
        <p class="mb-0">Sistema de Administración - Profesores, Alumnos y Clases</p>
    </header>
    
    🧭 navbar.html
    <nav class="navbar navbar-expand-lg navbar-dark">
      <div class="container-fluid">
        <a class="navbar-brand fw-bold" href="{% url 'inicio' %}">Inicio</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#menuDanza">
          <span class="navbar-toggler-icon"></span>
        </button>
    
        <div class="collapse navbar-collapse" id="menuDanza">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
    
            <!-- PROFESORES -->
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" id="profesoresDropdown" role="button" data-bs-toggle="dropdown">Profesores</a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="{% url 'agregar_profesor' %}">Agregar Profesor</a></li>
                <li><a class="dropdown-item" href="{% url 'ver_profesores' %}">Ver Profesores</a></li>
              </ul>
            </li>
    
            <!-- ALUMNOS -->
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" id="alumnosDropdown" role="button" data-bs-toggle="dropdown">Alumnos</a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="{% url 'agregar_alumno' %}">Agregar Alumno</a></li>
                <li><a class="dropdown-item" href="{% url 'ver_alumnos' %}">Ver Alumnos</a></li>
              </ul>
            </li>
    
            <!-- CLASES -->
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" id="clasesDropdown" role="button" data-bs-toggle="dropdown">Clases</a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="{% url 'agregar_clase' %}">Agregar Clase</a></li>
                <li><a class="dropdown-item" href="{% url 'ver_clases' %}">Ver Clases</a></li>
              </ul>
            </li>
    
          </ul>
        </div>
      </div>
    </nav>
    
    🦶 footer.html
    <footer>
        <p>© 2025 - Creado por Ing. Eliseo Nava, CBTis 128</p>
        <p id="fecha-actual"></p>
    
        <script>
            document.getElementById("fecha-actual").textContent = new Date().toLocaleDateString();
        </script>
    </footer>
    
    🏫 inicio.html
    {% extends 'base.html' %}
    {% block content %}
    <div class="text-center">
        <h2 class="fw-bold text-primary">Bienvenido al Sistema de la Escuela de Danza</h2>
        <p class="lead mt-3">Administra de manera sencilla a tus <strong>profesores, alumnos y clases</strong>.</p>
    
        <div class="mt-4">
            <img src="https://cdn.pixabay.com/photo/2017/01/16/19/40/dance-1988914_1280.jpg"
                 alt="Escuela de Danza"
                 class="img-fluid rounded shadow"
                 style="max-width: 700px;">
        </div>
    
        <p class="text-muted mt-3">Inspirando arte y movimiento desde 2025 💫</p>
    </div>
    {% endblock %}
18. En el archivo base.html

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
    👨‍🏫 agregar_profesor.html
    {% extends 'base.html' %}
    {% block content %}
    <div class="card shadow p-4">
        <h2 class="text-center mb-4 text-primary">Agregar Profesor</h2>
        <form method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <div class="text-center mt-3">
                <button class="btn btn-success px-4">Guardar</button>
                <a href="{% url 'ver_profesores' %}" class="btn btn-secondary px-4">Cancelar</a>
            </div>
        </form>
    </div>
    {% endblock %}
    
    📋 ver_profesores.html
    {% extends 'base.html' %}
    {% block content %}
    <h2 class="text-center mb-4 text-primary">Lista de Profesores</h2>
    
    <table class="table table-bordered table-striped align-middle shadow-sm">
        <thead class="table-dark text-center">
            <tr>
                <th>Nombre</th>
                <th>Especialidad</th>
                <th>Teléfono</th>
                <th>Email</th>
                <th>Experiencia</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for profesor in profesores %}
            <tr>
                <td>{{ profesor.nombre }}</td>
                <td>{{ profesor.especialidad }}</td>
                <td>{{ profesor.telefono }}</td>
                <td>{{ profesor.email }}</td>
                <td>{{ profesor.experiencia }} años</td>
                <td class="text-center">
                    <a href="{% url 'actualizar_profesor' profesor.id %}" class="btn btn-warning btn-sm">Editar</a>
                    <a href="{% url 'borrar_profesor' profesor.id %}" class="btn btn-danger btn-sm">Borrar</a>
                </td>
            </tr>
            {% empty %}
            <tr><td colspan="6" class="text-center text-muted">No hay profesores registrados.</td></tr>
            {% endfor %}
        </tbody>
    </table>
    
    <div class="text-center mt-4">
        <a href="{% url 'agregar_profesor' %}" class="btn btn-success">Agregar Nuevo Profesor</a>
    </div>
    {% endblock %}
    
    ✏️ actualizar_profesor.html
    {% extends 'base.html' %}
    {% block content %}
    <div class="card shadow p-4">
        <h2 class="text-center mb-4 text-primary">Actualizar Profesor</h2>
        <form method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <div class="text-center mt-3">
                <button class="btn btn-primary px-4">Actualizar</button>
                <a href="{% url 'ver_profesores' %}" class="btn btn-secondary px-4">Cancelar</a>
            </div>
        </form>
    </div>
    {% endblock %}
    
    ❌ borrar_profesor.html
    {% extends 'base.html' %}
    {% block content %}
    <div class="card shadow p-5 text-center">
        <h2 class="text-danger mb-3">Eliminar Profesor</h2>
        <p>¿Deseas eliminar al profesor <strong>{{ profesor.nombre }}</strong>?</p>

    <form method="post">
        {% csrf_token %}
        <button class="btn btn-danger px-4">Sí, eliminar</button>
        <a href="{% url 'ver_profesores' %}" class="btn btn-secondary px-4">Cancelar</a>
    </form>
    </div>
    {% endblock %}

