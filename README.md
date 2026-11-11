# FocusLab — Frontend

**Proyecto de Aula · Desarrollo Web Full-Stack · 2026-2**

Aplicación web para organizar períodos académicos, materias y materiales de estudio. Está desarrollada con Next.js (App Router), React, TypeScript y Tailwind CSS y consume la API de FocusLab.

El objetivo es que cada estudiante pueda organizar su semestre en un solo lugar, relacionar sus materias con períodos académicos y consultar sus archivos de estudio. Este repositorio contiene **únicamente el frontend**; el backend, la base de datos y el almacenamiento de archivos se encuentran en otro repositorio.

## Enlaces

- [Frontend en Vercel](https://gestion-academica-frontend-psi.vercel.app)
- [Repositorio del frontend](https://github.com/ERICKJLLO/Gestion-academica-Frontend)
- [Repositorio del backend](https://github.com/MarioFer299/Gestion-academica-Back)
- [API en Render](https://gestion-academica-back-cgg8.onrender.com)

## Funcionalidades

- Registro, inicio y cierre de sesión con JWT.
- Rutas privadas y panel de administración restringido al rol `ADMIN`.
- Gestión de períodos académicos con fechas de inicio y finalización.
- Registro, consulta, edición y eliminación de materias asociadas a períodos.
- Biblioteca para subir, consultar, buscar, filtrar y eliminar materiales.
- Panel principal con resumen de materias, períodos y archivos.
- Estados de carga, error y listas vacías en las pantallas de gestión.

Las operaciones administrativas adicionales dependen de los endpoints que ofrezca el backend; el panel actual verifica el acceso por rol.

## Tecnologías utilizadas

| Tecnología | Uso |
| --- | --- |
| Next.js 15 (App Router) | Rutas, páginas y compilación del frontend |
| React 19 | Componentes y estado de la interfaz |
| TypeScript | Tipado estático |
| Tailwind CSS 3 | Estilos y diseño responsive |
| Lucide React | Iconos |
| Fetch API | Comunicación HTTP mediante servicios centralizados |
| Prettier | Formato del código |
| Git y GitHub | Control de versiones y colaboración |
| Vercel | Despliegue público del frontend |

## Requisitos previos

- Git instalado.
- Node.js 20 o superior y npm.
- Una terminal (PowerShell, CMD o Bash).
- Acceso a la API de FocusLab, ya sea local o desplegada.

Comprobar las herramientas desde una terminal:

```bash
node --version
npm --version
git --version
```

## Instalación paso a paso

### 1. Descargar el proyecto

Abrir una terminal en la carpeta donde se quiera guardar el código y ejecutar:

```bash
git clone https://github.com/ERICKJLLO/Gestion-academica-Frontend.git
cd Gestion-academica-Frontend
```

### 2. Instalar las dependencias

```bash
npm ci
```

`npm ci` instala las versiones registradas en `package-lock.json`. Es la opción recomendada para reproducir la instalación del proyecto.

### 3. Configurar la conexión con la API

Crear un archivo llamado `.env.local` en la raíz del frontend, al mismo nivel que `package.json`.

En **PowerShell (Windows)** se puede crear a partir del ejemplo mediante:

```powershell
Copy-Item .env.example .env.local
```

Si se trabaja con el backend ejecutándose en el computador, el contenido es:

```env
NEXT_PUBLIC_API_HOST=http://localhost:4000
```

Si se prefiere utilizar el backend desplegado en Render, cambiarlo por:

```env
NEXT_PUBLIC_API_HOST=https://gestion-academica-back-cgg8.onrender.com
```

Se configura **una sola URL**, según el entorno que se vaya a utilizar. No se debe agregar `/api` al final porque las rutas del cliente HTTP ya lo incluyen.

### 4. Iniciar el servidor de desarrollo

```bash
npm run dev
```

Abrir [http://localhost:3000](http://localhost:3000) en el navegador. Para detener el servidor, presionar `Ctrl + C` en la terminal.

### 5. Probar la aplicación

Desde la página de registro se puede crear una cuenta y después iniciar sesión. El flujo recomendado es crear un período, añadir una materia asociada a ese período y subir un material a la biblioteca.

Si se utiliza un backend local, este debe estar encendido antes de probar los formularios.

**Importante:** el backend debe permitir el origen del frontend en CORS. `.env.local` no se sube a Git. Las credenciales de base de datos, JWT y almacenamiento pertenecen exclusivamente al backend.

## Comandos disponibles

| Comando | Descripción |
| --- | --- |
| `npm run dev` | Inicia el servidor local de desarrollo |
| `npm run build` | Genera la compilación de producción |
| `npm run start` | Ejecuta la compilación de producción después de `build` |
| `npm run lint` | Comprueba los tipos con TypeScript (`tsc --noEmit`) |
| `npm run format` | Aplica Prettier al código de `app/` y `src/` |
| `npm run format:check` | Comprueba el formato sin modificar archivos |

**Nota:** el comando `lint` de este proyecto ejecuta TypeScript; no equivale a un análisis de reglas de ESLint.

## Verificación

```bash
npm run lint
npm run build
npm run format:check
```

`npm run lint` comprueba los tipos con TypeScript; `npm run format:check` revisa el formato con Prettier. Para ordenar el formato del frontend se puede ejecutar `npm run format`.

Prueba manual de integración con la API disponible:

1. Registrar un usuario e iniciar sesión.
2. Crear un período con sus fechas.
3. Crear una materia asociada al período y verificar que aparece el nombre del período.
4. Subir un PDF a la biblioteca, consultarlo y probar la búsqueda y los filtros.
5. Editar y eliminar registros de prueba, comprobando los mensajes y confirmaciones.
6. Cerrar sesión e intentar acceder directamente a una ruta privada.
7. Comprobar el menú, las tarjetas y los formularios en una pantalla pequeña.

## Organización del código

```text
app/                            Rutas y páginas: inicio, login, registro,
                                dashboard, períodos, materias, recursos y admin
src/features/auth/              Autenticación y protección de rutas
src/features/academic-periods/  Períodos académicos
src/features/subjects/          Materias
src/features/resources/         Biblioteca de archivos
src/features/common/            Componentes compartidos y cliente HTTP
```

Cada módulo organiza componentes, tipos, hooks y servicios. El cliente HTTP centralizado en `src/features/common/services/http.ts` agrega el token a las solicitudes autenticadas.

La organización por funcionalidades tiene cuatro partes principales:

- `domain/`: tipos e interfaces utilizados por el módulo.
- `services/`: funciones que llaman a los endpoints del backend.
- `hooks/`: estado y lógica de consulta o actualización de información.
- `components/`: elementos visuales reutilizables.

Las páginas de `app/` utilizan estos componentes para construir cada vista.

## Autenticación y seguridad del frontend

El registro y el inicio de sesión consumen la API del equipo. El token JWT devuelto por el servidor se almacena para mantener la sesión al actualizar la página. El cliente HTTP añade automáticamente `Authorization: Bearer <token>` a las solicitudes cuando existe una sesión.

El cierre de sesión elimina los datos locales. Cuando el servidor responde `401`, el frontend borra la sesión y redirige a `/login`. Las rutas privadas y la ruta `/admin` utilizan una comprobación de sesión y rol del lado del cliente. **Las autorizaciones definitivas de acceso a datos deben validarse en el backend.**

## Despliegue en Vercel

El frontend se despliega desde GitHub. En **Settings → Environment Variables** se configura:

| Variable | Valor |
| --- | --- |
| `NEXT_PUBLIC_API_HOST` | `https://gestion-academica-back-cgg8.onrender.com` |

Después de cambiar variables se debe volver a desplegar. El backend tiene que autorizar `https://gestion-academica-frontend-psi.vercel.app` en CORS. Que la página cargue no demuestra que la API funcione: es necesario probar registro, inicio de sesión y subida de archivos desde la URL pública.

### Pasos para desplegar una copia

1. Importar el repositorio de GitHub en Vercel.
2. Seleccionar Next.js como framework (normalmente Vercel lo detecta automáticamente).
3. Mantener el directorio raíz del proyecto.
4. Configurar la variable `NEXT_PUBLIC_API_HOST` con la URL del backend accesible públicamente.
5. Ejecutar el despliegue y abrir la URL que entrega Vercel.
6. Pedir que esa URL quede autorizada en el CORS del backend.
7. Probar registro, inicio de sesión, períodos, materias y subida de archivos desde el sitio publicado.

## Solución de problemas frecuentes

| Problema | Qué revisar |
| --- | --- |
| `npm` o `node` no se reconoce | Instalar Node.js y abrir nuevamente la terminal |
| Falla `npm ci` | Comprobar la versión de Node.js y la conexión a internet |
| La página carga, pero no inicia sesión | Revisar `NEXT_PUBLIC_API_HOST`, que el backend responda y que CORS permita el origen |
| La API funciona en local, pero falla en Vercel | Comprobar la variable de entorno de Vercel y volver a desplegar |
| Aparece `401` | La sesión es inválida o venció; iniciar sesión nuevamente |
| No aparecen materias en el selector de archivos | Crear primero un período y una materia vinculada |
| La compilación falla | Ejecutar `npm ci`, `npm run lint` y revisar el error mostrado |

## Integrantes

- **Jhon Erick Jaramillo Restrepo:** frontend, experiencia de usuario e integración con la API.
- **Mario:** backend, base de datos y almacenamiento de archivos.

Proyecto académico realizado en equipo.

