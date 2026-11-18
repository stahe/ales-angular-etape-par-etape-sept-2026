# Introducción paso a paso al framework Angular

📖 **Lea el tutorial: [https://stahe.github.io/ales-angular-etape-par-etape-sept-2026/](https://stahe.github.io/ales-angular-etape-par-etape-sept-2026/)**

Este curso enseña a construir una aplicación web con el framework [Angular](https://angular.dev) 22: una **aplicación de página única** (SPA), cuyas páginas se generan **en el navegador** a partir de datos JSON provenientes de un servidor.

Es la continuación del curso [Introducción paso a paso al framework web NestJS](https://stahe.github.io/ales-nestjs-html-sept-2026/), al que ofrece una perspectiva distinta, y sigue la estructura de la [Introducción paso a paso al framework Vue.js](https://stahe.github.io/ales-vuejs-etape-par-etape-sept-2026/): mismo servidor, mismas páginas, escritas al estilo de Angular.

| Curso NestJS | Curso Angular |
|---|---|
| el servidor genera las páginas HTML (Handlebars) | el navegador genera las páginas (Angular) |
| el navegador muestra lo que recibe | el servidor solo devuelve JSON |
| controladores, vistas, `res.render` | componentes, enrutador, servicios |
| guardias `JwtAuthGuard`, `RolesGuard` | guardias del enrutador (el servidor mantiene las suyas) |
| diccionarios leídos por el servidor | diccionarios en el cliente (ngx-translate); el servidor solo devuelve claves |
| mensaje flash en una cookie | mensaje flash en un servicio de signals |

Las páginas, en cambio, no cambian: son las mismas de la aplicación **RdvMedecins** ya presentada con los otros frameworks.

## El enfoque: muchos ejemplos breves y luego un caso de estudio

Como Angular tiene más funcionalidades que Vue.js, el curso se estructura en torno a **25 ejemplos breves**, cada uno centrado en un único concepto. Forman un único espacio de trabajo de Angular: un solo comando `npm install`, seguido de `npm start <ejemplo>` para ejecutar uno.

| Capítulo | Contenido | Ejemplos |
|---|---|---|
| Primeros pasos | un espacio de trabajo de Angular, signals (`signal`, `computed`, `effect`, `linkedSignal`), plantillas (`@if`, `@for`, `@let`), eventos, `ngModel`, formularios basados en signals, validación, formularios reactivos, pipes | 01–09 |
| Componentes | `input()`, `output()`, `model()`, proyección de contenido, ciclo de vida, servicios e inyección de dependencias, directivas, cuadro de diálogo de confirmación | 10–16 |
| Enrutamiento | el enrutador, parámetros, query, carga diferida, guardias de navegación | 17–18 |
| Observables y estado compartido | RxJS (`debounceTime`, `switchMap`, `toSignal`), servicios de signals, `localStorage` | 19–20 |
| Internacionalización | ngx-translate: parámetros, plurales, fechas, importes | 21 |
| El servidor: una caja negra | instalación del servidor JSON, su API, 48 ejemplos con `curl` | – |
| Comunicación con el servidor | `HttpClient`, proxy de `ng serve`, cookie `httpOnly`, interceptor, capa de acceso a la API, `httpResource`, errores del servidor asociados a los campos | 22–25 |

Cada ejemplo se presenta con su código completo, comentado línea por línea, y una captura de pantalla de su ejecución.

## El servidor: una caja negra

El servidor es el servidor NestJS de la lección anterior, cuyos controladores devuelven **JSON**, igual que para el cliente Vue.js. Esta lección lo trata como una **caja negra**: se instala, se estudia su API y se consulta con `curl`, pero no es necesario leer su código (que se proporciona y está comentado para los curiosos).

- Todos los errores tienen el mismo formato: `{ "statusCode": 409, "cle": "ERRORS. LOGIN_TAKEN", "params": {...}, "champs": {...} }` — **claves** de traducción, nunca texto literal;
- autenticación mediante un token JWT en una cookie `httpOnly` / `sameSite=strict`: el JavaScript del cliente nunca ve el token;
- protección CSRF: cookie `sameSite`, y todas las solicitudes POST deben enviarse en JSON;
- un “modo de prueba” para el CAPTCHA que permite consultar la API con `curl`.


## El caso de estudio: el cliente Angular de RdvMedecins

Una aplicación completa para **reservar citas en un consultorio médico**, con **todos** sus archivos (unos 50) listados y comentados.

- **Angular moderno**: componentes independientes (standalone), signals, detección de cambios sin zone.js, formularios basados en signals con controles personalizados (`FormValueControl`), páginas cargadas bajo demanda.
- **Tres roles**: `ADMIN` (administra médicos y clientes), `DOCTOR` (gestiona la agenda y cancela citas), `USER` (el paciente: reserva citas para sí mismo y administra su cuenta).
- **Confidencialidad**: un paciente nunca recibe los nombres de otros pacientes; el servidor no los envía.
- **Todo el estado de la página en la URL**: `/agenda?idMedecin=1&jour=2026-10-05&reserver=7` vuelve a abrir la ventana de reserva tras pulsar F5; los botones Anterior/Siguiente funcionan.
- **Validación del lado del servidor**: los formularios muestran bajo cada campo los errores devueltos por la API; bloqueo optimista, nombres duplicados, login ya en uso, etc.
- **Sesión**: se restaura tras pulsar F5 (`GET /api/auth/moi`); la expiración se gestiona en un único lugar (un interceptor HTTP, respuesta 401).
- **Francés / inglés**, títulos de pestaña traducidos, ventana de confirmación accesible con el teclado, Bootstrap 5.
- **Despliegue**: el cliente compilado lo sirve el propio servidor JSON (mismo origen, sin CORS).

## Tecnologías

Angular 22.2 (componentes standalone, signals, formularios basados en signals, sin zone.js) · TypeScript 6.0 · Angular CLI · RxJS 7.8 · ngx-translate 18 · Bootstrap 5.3 · del lado del servidor: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Requisitos previos

- Conocimientos básicos de JavaScript (o TypeScript), HTML y del protocolo HTTP.
- Node.js 24 (o al menos 22.22.3), Visual Studio Code con la extensión Angular Language Service y un servidor MySQL (por ejemplo, Laragon en Windows) para el servidor JSON. Las instrucciones de instalación se proporcionan en los apéndices del curso.

## Autor

Este curso, sus ejemplos, el servidor JSON y el caso de estudio fueron escritos por **Claude**, la IA de [Anthropic](https://www.anthropic.com) (septiembre de 2026), a petición de **Serge Tahé**
