# MiniScrum Distribuido

## Objetivo

Aplicación web distribuida para gestionar tareas de un proyecto Scrum. Permite crear tareas con predicción automática de puntos Scrum usando regresión lineal, guardarlas en una base de datos relacional y actualizar su estado.

## Arquitectura

```
Navegador -> Node Gateway -> PHP API -> MySQL
Navegador -> Node Gateway -> Python ML API
```

## Servicios

- **gateway**: Frontend estático y API Gateway en Node.js + Express.
- **php-api**: CRUD de tareas en PHP + Apache conectado a MySQL.
- **python-ml**: Predicción de puntos Scrum con FastAPI + scikit-learn.
- **mysql**: Base de datos relacional MySQL 8.

## Endpoints principales

- `GET  /api/tasks`            — Listar tareas
- `POST /api/tasks`            — Crear tarea
- `PUT  /api/tasks/:id/status` — Actualizar estado
- `POST /api/predict`          — Predecir puntos Scrum

## Base de datos

Tabla principal: `tasks`

| Campo           | Tipo                                          |
|----------------|-----------------------------------------------|
| id             | INT AUTO_INCREMENT PRIMARY KEY               |
| title          | VARCHAR(150)                                  |
| description    | TEXT                                          |
| estimated_hours| DECIMAL(5,2)                                  |
| scrum_points   | INT                                           |
| status         | ENUM('Pendiente', 'En proceso', 'Terminada') |
| created_at     | TIMESTAMP                                     |

## Como ejecutar localmente

```bash
docker compose up --build
```

Luego abrir: http://localhost:3000

Para detener y borrar datos:

```bash
docker compose down -v
```

## URL desplegada

<!-- Pegar aquí la URL de Dockploy -->

## Evidencias

<!-- Agregar capturas de:
- Aplicación funcionando
- Tareas guardadas
- Contenedores activos
- Repositorio con commits -->

## Retrospectiva

**Qué salió bien:**

**Qué fue difícil:**

**Qué mejoraría:**

---

## Preguntas de reflexión

1. **¿Por qué el navegador no se conecta directamente a MySQL?**  
   MySQL es un servicio interno que no debe estar expuesto al exterior. El navegador solo puede hacer peticiones HTTP/HTTPS; una conexión directa a MySQL requeriría credenciales en el cliente, lo cual sería un riesgo de seguridad grave.

2. **¿Qué ventaja tiene separar el servicio PHP del servicio Python?**  
   Cada servicio puede escalar, actualizarse y desplegarse de forma independiente. Si el modelo de ML mejora, solo se redeploya Python sin tocar el CRUD.

3. **¿Qué función cumple el API Gateway?**  
   Es el único punto de entrada público. Sirve el frontend y redirige las peticiones hacia los servicios internos (PHP o Python), abstrayendo la complejidad del sistema al navegador.

4. **¿Qué pasaría si el servicio Python deja de funcionar?**  
   La predicción de puntos fallaría, pero el CRUD de tareas seguiría operando normalmente. El sistema degrada parcialmente sin caerse por completo.

5. **¿Qué diferencia hay entre ejecutar localmente con Docker Compose y desplegar en Dockploy?**  
   Localmente los contenedores corren en tu máquina y solo son accesibles desde localhost. En Dockploy corren en un VPS con una IP pública y un dominio asignado, accesible desde cualquier lugar.

6. **¿Qué parte del sistema representa la capa de datos?**  
   MySQL, gestionado a través del servicio PHP.

7. **¿Qué parte representa la capa de presentación?**  
   El frontend estático (index.html, style.css, app.js) servido por el Gateway.

8. **¿Qué parte representa la lógica de negocio?**  
   El servicio PHP (reglas del CRUD y validaciones) y el servicio Python (lógica de predicción de puntos Scrum).

URL : http://10.14.255.93:3000/api/deploy/compose/h7tUX1hVvRiLRkXSnEmM6