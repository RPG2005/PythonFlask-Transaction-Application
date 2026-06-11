$readmeContent = @"
# Flask Transactions App

Una aplicación web simple con Flask para gestionar transacciones financieras usando operaciones CRUD (Create, Read, Update, Delete).

## 📋 Descripción

Esta aplicación permite:
- 📖 **Listar** todas las transacciones
- ➕ **Crear** nuevas transacciones con fecha y monto
- ✏️ **Editar** transacciones existentes
- 🗑️ **Eliminar** transacciones

---

## 📁 Estructura del proyecto

`
obmnl-flask_assignment/
│
├── app.py                      # Aplicación principal con rutas CRUD
├── requirements.txt            # Dependencias Python
├── Dockerfile                  # Configuración Docker
├── docker-compose.yml          # Orquestación Docker
├── .dockerignore               # Archivos a ignorar en Docker
├── README.md                   # Este archivo
│
├── templates/                  # Plantillas HTML
│   ├── transactions.html       # Lista de todas las transacciones
│   ├── form.html               # Formulario crear/editar transacción
│   ├── search.html             # Búsqueda de transacciones (opcional)
│   └── edit.html               # Plantilla de edición (opcional)
│
└── static/                     # Archivos estáticos (CSS, JS, imágenes)
    └── css/
        └── style.css           # Estilos CSS (opcional)
`

---

## 🛣️ Rutas disponibles

### Rutas GET (Lectura)

| Ruta | Método | Descripción | Plantilla |
|------|--------|-------------|-----------|
| `/` | GET | Redirige a lista de transacciones | - |
| `/transactions` | GET | Lista todas las transacciones | `transactions.html` |
| `/transactions/new` | GET | Formulario para crear nueva transacción | `form.html` |
| `/transactions/<id>/edit` | GET | Formulario para editar transacción | `edit.html` |

### Rutas POST (Escritura)

| Ruta | Método | Descripción | Datos requeridos |
|------|--------|-------------|------------------|
| `/transactions/new` | POST | Crear nueva transacción | `date`, `amount` |
| `/transactions/<id>/edit` | POST | Actualizar transacción existente | `date`, `amount` |
| `/transactions/<id>/delete` | POST | Eliminar transacción | - |

---

## 📊 Formato de datos

Cada transacción tiene la siguiente estructura:

`
{
  "id": 1,
  "date": "2023-06-01",
  "amount": 100
}
`

### Campos:
- **id** (int): Identificador único
- **date** (string): Fecha en formato YYYY-MM-DD
- **amount** (float/int): Monto (positivo = ingreso, negativo = gasto)

### Datos de ejemplo:
`
[
  {"id": 1, "date": "2023-06-01", "amount": 100},
  {"id": 2, "date": "2023-06-02", "amount": -200},
  {"id": 3, "date": "2023-06-03", "amount": 300}
]
`

---

## 🚀 Requisitos previos

### Opción 1: Ejecución local
- Python 3.8+
- pip (gestor de paquetes Python)

### Opción 2: Ejecución con Docker
- Docker
- Docker Compose

---

## 🔧 Instalación

### Opción 1: Instalación local

1. **Ir a la carpeta del proyecto:**
   `
   cd C:\docker\coursera\PythonFlask\obmnl-flask_assignment
   `

2. **Crear un entorno virtual (recomendado):**
   `
   python -m venv venv
   venv\Scripts\activate  # En Windows PowerShell
   `

3. **Instalar dependencias:**
   `
   pip install -r requirements.txt
   `

### Opción 2: Instalación con Docker

Solo necesitas tener Docker y Docker Compose instalados. La imagen se construirá automáticamente.

---

## 🏃 Ejecución

### Opción 1: Ejecución local

`
python app.py
`

**Salida esperada:**
`
* Running on http://127.0.0.1:5000
* Debug mode: on
  `

**Acceda a:** http://127.0.0.1:5000

### Opción 2: Ejecución con Docker Compose

`
docker-compose up --build
`

**Salida esperada:**
`
Creating flask-transactions-app ... done
Attaching to flask-transactions-app
flask-transactions-app | Running on http://0.0.0.0:5000
`

**Acceda a:** http://localhost:5000

#### Detener la aplicación:
`
docker-compose down
`

#### Detener y remover volúmenes:
`
docker-compose down -v
`

### Opción 3: Ejecución manual con Docker

`
# Construir la imagen
docker build -t flask-transactions-app .

# Ejecutar contenedor (Windows PowerShell)
docker run -p 5000:5000 -v `${PWD}:/app flask-transactions-app

# Ejecutar contenedor (Windows CMD)
docker run -p 5000:5000 -v %cd%:/app flask-transactions-app
`

---

## 🐳 Comandos Docker útiles

### Ver estado de contenedores

`
# Ver contenedores en ejecución
docker-compose ps

# Ver todos los contenedores (incluyendo parados)
docker container ls -a

# Ver imágenes disponibles
docker image ls
`

### Acceder y depurar

`
# Acceder al shell del contenedor
docker-compose exec flask-app sh

# Ver logs en tiempo real
docker-compose logs -f

# Ver logs de un contenedor específico
docker-compose logs flask-app

# Ver últimas N líneas de logs
docker-compose logs --tail=50
`

### Construcción y reconstrucción

`
# Reconstruir imagen sin cachés
docker-compose build --no-cache

# Reconstruir e iniciar
docker-compose up --build

# Solo construir sin iniciar
docker-compose build
`

### Limpieza y mantenimiento

`
# Parar todos los servicios
docker-compose stop

# Parar y remover contenedores
docker-compose down

# Remover contenedores, volúmenes y redes
docker-compose down -v

# Remover imágenes no usadas
docker image prune

# Remover todo (contenedores, volúmenes, redes, imágenes)
docker system prune -a
`

### Ejecución de comandos dentro del contenedor

`
# Instalar nuevo paquete dentro del contenedor
docker-compose exec flask-app pip install nombre-paquete

# Ejecutar Python dentro del contenedor
docker-compose exec flask-app python -c "print('Hello')"

# Ver archivos en el contenedor
docker-compose exec flask-app ls -la
`

---

## 📝 Configuración Docker

### `Dockerfile`

`
FROM python:3.9-slim
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
WORKDIR /app
RUN apt-get update && apt-get install -y gcc && rm -rf /var/lib/apt/lists/*
COPY requirements.txt /app/
RUN pip install --upgrade pip && pip install -r requirements.txt
COPY . /app/
EXPOSE 5000
CMD ["python", "app.py"]
`

### `docker-compose.yml`

`
version: '3.8'

services:
flask-app:
build: .
container_name: flask-transactions-app
ports:
- "5000:5000"
volumes:
- .:/app
environment:
- FLASK_ENV=development
- FLASK_APP=app.py
command: python app.py
`

---

## 📝 Notas importantes

- **Almacenamiento de datos:** Los datos se almacenan en memoria; se pierden al reiniciar la app
- **Para producción:** Migra a una base de datos real (SQLite, PostgreSQL, MySQL)
- **Volúmenes Docker:** El volumen `-v` sincroniza cambios en tiempo real
- **Debug mode:** En Docker, el debug mode está habilitado para desarrollo

---

## 🚨 Solución de problemas

### Puerto 5000 en uso
`
# Encuentra qué proceso usa el puerto 5000
netstat -ano | findstr :5000

# Termina el proceso (reemplaza PID con el ID encontrado)
taskkill /PID <PID> /F

# O cambia el puerto en docker-compose.yml: "5001:5000"
`

### Permisos de Docker en Linux
`
sudo usermod -aG docker `$USER
newgrp docker
`

### Reconstruir sin cachés si hay cambios en requirements.txt
`
docker-compose build --no-cache
docker-compose up
`

---

## 📄 Licencia

MIT

## 👤 Autor

Proyecto de laboratorio Coursera Flask Assignment
"@

Set-Content -Path "C:\docker\coursera\PythonFlask\obmnl-flask_assignment\README.md" -Value $readmeContent -Encoding UTF8

Write-Host "✅ README.md creado exitosamente en C:\docker\coursera\PythonFlask\obmnl-flask_assignment\README.md"