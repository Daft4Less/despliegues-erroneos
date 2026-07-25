# 🛠️ Navaja Suiza de Comandos - DevOps y CI/CD

## 🐳 Docker - Gestión de Contenedores
| Comando | Descripción |
| :--- | :--- |
| `docker build -t <nombre-imagen> .` | Construye una imagen a partir del Dockerfile en el directorio actual. |
| `docker run -d -p <host>:<contenedor> --name <nombre> <imagen>` | Ejecuta un contenedor en segundo plano mapeando los puertos. |
| `docker ps` | Lista los contenedores que están en ejecución. |
| `docker ps -a` | Lista todos los contenedores (activos y detenidos). |
| `docker stop <nombre-contenedor>` | Detiene un contenedor en ejecución de forma segura. |
| `docker rm -f <nombre-contenedor>` | Fuerza la eliminación de un contenedor (incluso si está corriendo). |
| `docker logs <nombre-contenedor>` | Muestra los registros (logs) del contenedor para diagnóstico. |
| `docker exec -it <nombre-contenedor> sh` | Abre una terminal interactiva dentro del contenedor. |

## ☸️ Minikube & Kubernetes - Orquestación
| Comando | Descripción |
| :--- | :--- |
| `minikube start` | Inicia el clúster local de Minikube. |
| `minikube status` | Verifica el estado de los componentes del clúster. |
| `minikube image load <imagen>:<tag>` | Carga una imagen de Docker local directamente en la caché de Minikube. |
| `kubectl get nodes` | Lista los nodos del clúster y su estado (Ready/NotReady). |
| `kubectl apply -f <archivo.yml>` | Crea o actualiza recursos en el clúster usando un manifiesto YAML. |
| `kubectl get pods` | Lista todos los pods y su estado actual (Running, ImagePullBackOff, etc.). |
| `kubectl get pods --show-labels` | Lista los pods y muestra la columna con las etiquetas asignadas. |
| `kubectl describe service <nombre>` | Muestra información detallada de un servicio (incluyendo selectores). |
| `kubectl get endpoints <nombre>` | Verifica las direcciones IP de los pods que el servicio está detectando. |

## 🐙 Git - Control de Versiones (Guardar y Subir)
Si necesitas guardar tu avance y enviarlo a tu repositorio remoto en GitHub/GitLab, ejecuta estos comandos en orden:

```bash
# 1. Verifica qué archivos han sido modificados
git status

# 2. Agrega todos los cambios al "área de preparación" (staging)
git add .

# 3. Empaqueta los cambios con un mensaje descriptivo
git commit -m "feat: correccion de puerto en Dockerfile y selectores en K8s"

# 4. Sube los cambios a la rama principal de tu repositorio remoto
git push origin main

## 🐙 Git - Control de Versiones (Iniciar, Guardar y Subir)

Si estás empezando desde cero en una carpeta local y necesitas conectarla a tu repositorio remoto, este es el flujo de trabajo completo:

```bash
# 1. Inicializa un repositorio de Git en tu carpeta local (solo se hace la primera vez)
git init

# 2. Conecta tu repositorio local con tu repositorio remoto en GitHub (solo se hace la primera vez)
git remote add origin [https://github.com/Daft4Less/despliegues-erroneos.git](https://github.com/Daft4Less/despliegues-erroneos.git)

git remote add origin https://github.com/Daft4Less/despliegues-erroneos.git

git remote remove origin

# 3. Verifica qué archivos has creado o modificado
git status

# 4. Agrega todos los cambios al "área de preparación" (staging)
git add .

# 5. Empaqueta los cambios con un mensaje descriptivo
git commit -m "feat: configuracion inicial, correccion de Dockerfile y selectores K8s"

# 6. Asegúrate de que tu rama principal se llame "main" (buena práctica actual)
git branch -M main

# 7. Sube los cambios por primera vez vinculando la rama local con la remota
git push -u origin main


# hoja de vida de tu codigo inicial

# inventario-app

Catálogo de inventario con interfaz web y base de datos local. Este repositorio es el **punto de partida** de la tarea de CI/CD — no incluye `Dockerfile`, workflow de GitHub Actions ni manifiestos de Kubernetes: esos tres se construyen como parte del trabajo asignado.

## Qué es

Una app Node.js/Express con:

- **Interfaz web** (`public/index.html`, `public/app.js`, `public/styles.css`): una tabla de productos con formulario para agregar y botón para eliminar.
- **Base de datos local** (`db.js`): un archivo JSON en `data/products.json` que persiste los productos entre reinicios del proceso — sin motor de base de datos externo ni dependencias nativas.
- **API REST** consumida por la interfaz.

## Ejecutar en local

```bash
npm install
npm start
# abrir http://localhost:3000
```

## Pruebas

```bash
npm test
```

## Endpoints

| Método y ruta | Qué hace |
|---|---|
| `GET /health` | Estado de salud: `200` si el proceso y el archivo de base de datos son accesibles, `500` si no (o si `SIMULATE_FAILURE=true`). |
| `GET /version` | Devuelve `version`, `color` y `hostname` — configurables por variables de entorno `APP_VERSION` / `APP_COLOR`. |
| `GET /api/products` | Lista todos los productos. |
| `GET /api/products/:id` | Devuelve un producto por id. |
| `POST /api/products` | Crea un producto (`name`, `sku`, `stock`, `price`). |
| `PATCH /api/products/:id` | Actualiza campos de un producto. |
| `DELETE /api/products/:id` | Elimina un producto. |
| `GET /` | Sirve la interfaz web. |

## Variables de entorno

| Variable | Por defecto | Para qué |
|---|---|---|
| `PORT` | `3000` | Puerto del servidor. |
| `APP_VERSION` | `v1` | Se muestra en `/version` y en el encabezado de la interfaz. |
| `APP_COLOR` | `blue` | Color del encabezado — útil para distinguir versiones en un despliegue. |
| `SIMULATE_FAILURE` | `false` | Si es `true`, `/health` responde siempre `500`. |
| `DB_PATH` | `./data/products.json` | Ruta del archivo de base de datos local. |
