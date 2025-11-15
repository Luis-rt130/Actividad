# 🐳 Proyecto PHP + MySQL en Docker  
### (Explicado paso a paso pa' que nadie se pierda)

Este proyecto levanta una aplicación **PHP** que muestra **los nombres guardados en una base de datos MySQL**, todo corriendo con **Docker**.  
No hay ciencia, no hay magia: la página **solo lee los nombres desde la tabla `clientes` y los muestra**.

---

## 📦 Requisitos

Antes de empezar, necesitas:

- **Docker** instalado  
- Saber abrir una terminal (mínimo indispensable)

---

## 🚀 Cómo levantar el proyecto (paso a paso)

### 1️⃣ Entrar a la carpeta del proyecto

Ve a la carpeta donde están estos archivos:

- `docker-compose.yml`
- `index.php`
- `init.sql`

### 2️⃣ Levantar los contenedores

- En la terminal:

```bash
- docker-compose up -d
````

- Esto levanta:nube-web → servidor PHP

- nube-db → servidor MySQL

- - Ejecuta init.sql automáticamente para crear la BD y agregar los nombres

### 3️⃣ Abrir la app en el navegador

- Entra en:

- http://localhost:8080

- (ó el puerto que tengas configurado)

- Deberías ver los nombres guardados en la BD:

- Luis Romero

- Benjamin Ponce

- Si no los ves → revisa los pasos, porque te saltaste alguno.

### 🗄️ Estructura de Base de Datos

- Este proyecto usa un archivo init.sql que crea la BD, la tabla y mete los nombres.

- init.sql
- CREATE DATABASE IF NOT EXISTS empresa;
- USE empresa;
- CREATE TABLE IF NOT EXISTS clientes (
-   id INT AUTO_INCREMENT PRIMARY KEY,
-   nombre VARCHAR(100) NOT NULL,
- creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP
- );
- INSERT INTO clientes (nombre) VALUES
- ('Luis Romero'),
- 
- ('Benjamin Ponce');
### 🖥️ ¿Qué hace el index.php?

- Tu archivo index.php se conecta a MySQL y muestra todos los nombres de la tabla clientes.

- Código completo:
- <?php
- $dbHost = getenv('DB_HOST') ?: 'nube-db';
- $dbPort = getenv('DB_PORT') ?: '3306';
- $dbName = getenv('DB_NAME') ?: 'empresa';
- $dbUser = getenv('DB_USER') ?: 'appuser';
- $dbPass = getenv('DB_PASSWORD') ?: 'apppass';
- try {
-     $pdo = new PDO(
- 
-         "mysql:host=$dbHost;port=$dbPort;dbname=$dbName;charset=utf8mb4",
-         $dbUser,
-         $dbPass,
-         [
-             PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
-             PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
-         ]
-    );
-     $stmt = $pdo->query("SELECT nombre FROM clientes");
-     $nombres = $stmt->fetchAll();
- 
- } catch (Throwable $e) {
-     die("Error conectando a la BD: " . $e->getMessage());
- }
- ?>
- <!DOCTYPE html>
- <html lang="es">
- <head>
-    <meta charset="UTF-8">
-    <title>Nombres desde la BD</title>
- </head>
- <body>
-    <h1>Nombres guardados en MySQL</h1>
-    <ul>
-        <?php foreach ($nombres as $n): ?>
- 
-            <li><?= htmlspecialchars($n['nombre']) ?></li>
-        <?php endforeach; ?>
-    </ul>
-</body>
-</html>
### 🛑 Problemas comunes y cómo arreglarlos
Problema	Explicación	Solución
-
- “No se conecta a la BD”	El contenedor PHP arrancó antes que MySQL	Espera 5s o corre: docker-compose restart
- “No salen los nombres”	La tabla está vacía	Revisar BD: docker exec -it nube-db mariadb -u root -p
- “Página en blanco”	Error PHP sin mostrar	Revisar logs: docker logs nube-web
- “Error 500”	Copiaste mal el código	Revisa comillas, llaves y el init.sql
- ➕ Agregar más nombres manualmente
- Si quieres agregar más personas a la tabla:

- Entra a MySQL dentro del contenedor:

- docker exec -it nube-db mariadb -u root -p

- Inserta un nuevo nombre:

- INSERT INTO clientes (nombre) VALUES ('Otro Nombre');

- Recarga la página y listo.

 ### 📁 Estructura recomendada del proyecto
/proyecto
- 
│── docker-compose.yml
│── index.php
│── init.sql
└── README.md
### 🏁 Final

- Con esto ya tienes todo:

- Base de datos creada

- PHP leyendo los nombres

- Docker levantando todo sin romper tu PC

- README que explica todo paso a paso

- Si quieres un Dockerfile, un diagrama de arquitectura, o un CRUD completo… me avisai nomás.

- 
- ---

- Listo causa, si querís lo hago más serio, más flaite, más universitario o más profesional.
