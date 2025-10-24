# BMT-PI

## Integrantes

    David González Villanueva  C13388
    Marco Angulo Rodríguez     C10458
    Alejandro Barboza Taylor   C10886
    Brandon Trigueros Lara     C17899
    Jose Carlos Mena Diaz      C14653

## Descripción del proyecto

BMT-PI es una plataforma de comercio/mercado (Backoffice y Frontend) desarrollada como proyecto integral que incluye una API en .NET (backend), una aplicación frontend en Vue.js, pruebas unitarias y de interfaz, y scripts de base de datos SQL. El sistema gestiona empresas, productos, pedidos, carritos de compra, direcciones y usuarios, e incluye mecanismos para envío de correos, manejo de imágenes y reportes.

El repositorio está organizado para separar claramente responsabilidades: dominio y reglas de negocio (Backend), interfaz de usuario (Frontend), scripts de base de datos y pruebas automatizadas.

## Contenido del repositorio

- `backend/` - Código fuente del backend (.NET)
- `frontend/` - Código fuente del frontend (Vue.js)
- `database/` - Scripts SQL para creación y migración de la base de datos
- `UITests/` - Pruebas de interfaz (Selenium/SIDE)
- `README.md` - Este archivo

---

## Resumen por partes

### Backend (API)

Ubicación: `backend/BMT-backend/`

Descripción:
- Aplicación principal desarrollada en C# con ASP.NET Core.
- Implementa controladores REST en `Controllers/` (p. ej. `ProductController.cs`, `UserController.cs`).
- Contiene capas y carpetas que apuntan a una arquitectura por capas/limpia: `Domain/` (entidades), `Application/` (interfaces, queries, servicios), `Infrastructure/` (persistencia y dependencias externas), `Handlers/` (lógica o handlers específicos), `Services/` y `Presentation/`.
- Configuración en `appsettings.json` y `appsettings.Development.json`.

Dependencias aproximadas (recomendadas):
- .NET SDK 6.0 o 7.0 (ver la versión objetivo en `BMT-backend.csproj`)
- Paquetes NuGet comunes: Entity Framework Core (o ADO.NET si se usa SQL raw), AutoMapper, FluentValidation, Microsoft.Extensions.*

Servicios y responsabilidades:
- Gestión de usuarios y autenticación (endpoints en `UserController`).
- Gestión de empresas y productos (`EnterpriseController`, `ProductController`).
- Procesamiento de pedidos, carritos de compra y reportes.
- Envío de mails (`MailController`) y manejo de archivos/imagenes (`ImageFileController`).

Pruebas:
- `backend/BMT-Unit-Tests/` contiene pruebas unitarias (xUnit/NUnit/MSTest según `*.csproj`).
- `backend/BMT-UITesting/` incluye pruebas UI automatizadas.

Arquitectura (diseño):
- El backend sigue una arquitectura en capas inspirada en Clean Architecture / Onion:
    - Domain: entidades y reglas de negocio puras.
    - Application: casos de uso, interfaces y DTOs.
    - Infrastructure: implementación de repositorios, acceso a datos y servicios externos.
    - Presentation: controladores y adaptadores web.

    Esta separación facilita pruebas unitarias, mantenimiento y reemplazo de infraestructuras (por ejemplo, cambiar la base de datos) sin tocar la lógica de negocio.

Base de datos:
- Hay scripts SQL en la carpeta `database/` para crear tablas, procedimientos almacenados y ajustes (p. ej. `01_TableCreation.sql`, `16_GetProductDetails.sql`, etc.).
- Se usó SQL Server.

Notas:
- Ver `appsettings*.json` para cadenas de conexión y claves; configure la base de datos antes de ejecutar.

---

### Frontend (SPA)

Ubicación: `frontend/bmt-frontend/`

Descripción:
- Interfaz de usuario construída con Vue.js (estructura típica de proyecto Vue).
- Archivos importantes: `package.json`, `babel.config.js`, `vue.config.js`, y la carpeta `src/` con componentes, vistas y servicios.

Responsabilidades:
- Proveer la experiencia de usuario para explorar productos, gestionar el carrito, realizar pedidos y administrar empresas/usuarios según permisos.
- Consumir la API del backend para autenticación, obtención y modificación de datos.

Dependencias aproximadas:
- Node.js (14+ o 16+ recomendado)
- npm o yarn
- Dependencias en `package.json` (Vue, Vuex/router si aplica, Axios para llamadas HTTP, otras librerías de UI)

Cómo ejecutar el frontend (Windows / PowerShell):

```powershell
cd frontend/bmt-frontend
npm install
npm run serve    # o npm run dev, según package.json
```

Construcción para producción:

```powershell
npm run build
```

---

## Scripts de base de datos

Carpeta: `database/`

Contiene scripts numerados para:
- Crear tablas y relaciones
- Procedimientos almacenados (SP)
- Migraciones y ajustes de esquema
- Índices y triggers

Ejecutar estos scripts en el servidor de base de datos antes de levantar la API.

---

## Pruebas y CI

- Pruebas unitarias: en `backend/BMT-Unit-Tests/`.
- Pruebas de interfaz/automáticas: `UITests/` y `backend/BMT-UITesting/`.
- Recomendación: integrar en CI (GitHub Actions, Azure DevOps, etc.) pasos para restaurar, compilar y ejecutar las pruebas unitarias.

---

## Notas de diseño y decisiones técnicas

- Separación de capas para mantener dominio independiente de infraestructuras.
- Uso de controladores REST y handlers para mantener controladores finos y mover la lógica a servicios/handlers.
- Persistencia desacoplada mediante interfaces en `Application/Interfaces` e implementaciones en `Infrastructure/`.
- Configuración externa (appsettings, variables de entorno) para no hardcodear cadenas de conexión ni secretos.

---

## Archivos/clasificadores importantes (rápida guía)

- `backend/BMT-backend/BMT-backend.csproj` - Proyecto backend principal
- `backend/BMT-backend/Program.cs` - Punto de entrada y configuración del host
- `backend/BMT-backend/Controllers/` - Endpoints HTTP
- `backend/BMT-backend/Application/Interfaces/` - Interfaces y contratos para casos de uso
- `frontend/bmt-frontend/package.json` - Dependencias y scripts del frontend
- `database/` - Scripts para infra de datos

---
