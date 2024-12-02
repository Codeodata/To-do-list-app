To-Do List App: Configuración con Docker
Este repositorio contiene una aplicación To-Do List desarrollada en React.js, con un backend en Node.js y una base de datos MongoDB. Aquí aprenderás cómo levantar la aplicación completa utilizando Docker.

Prerrequisitos
Asegúrate de tener instalados los siguientes componentes antes de comenzar:

Docker
Docker Compose (opcional)
Red de Docker personalizada:
bash
Copiar código
docker network create mse-calendar
Servicios
1. Base de Datos: MongoDB
Este comando despliega un servidor MongoDB con persistencia de datos:

bash
Copiar código
docker run -d \
  --name mongo \
  --hostname mongo \
  --network mse-calendar \
  -v mongodb_data:/data/db \
  mongo:5.0.14
Detalles:
--network mse-calendar: Conecta el contenedor a la red personalizada.
-v mongodb_data:/data/db: Asegura que los datos sean persistentes incluso si el contenedor se detiene.
mongo:5.0.14: Usa la imagen oficial de MongoDB, versión 5.0.14.
2. Backend: API
Este comando despliega el backend de la aplicación:

bash
Copiar código
docker run -d \
  --name api \
  --hostname api \
  --network mse-calendar \
  -p 3001:3001 \
  adrabenchemse/mse-calendar:backend
Detalles:
-p 3001:3001: Expone el puerto del backend en localhost:3001.
--network mse-calendar: Conecta el backend a la misma red que MongoDB.
3. Frontend: React.js
Finalmente, este comando despliega el frontend de la aplicación:

bash
Copiar código
docker run -d \
  --name web \
  --hostname web \
  --network mse-calendar \
  -p 3000:3000 \
  --env REACT_APP_API_URL=http://127.0.0.1:3001/api \
  adrabenchemse/mse-calendar:frontend
Detalles:
-p 3000:3000: Expone el frontend en localhost:3000.
--env REACT_APP_API_URL: Configura la URL base para conectar el frontend con el backend.
--network mse-calendar: Conecta el frontend a la red donde están MongoDB y el backend.
Acceso a la aplicación
Abre tu navegador y ve al frontend:
http://localhost:3000

El frontend interactúa con el backend en http://localhost:3001/api.

