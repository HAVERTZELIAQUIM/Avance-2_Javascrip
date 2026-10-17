# Ivy & Oak — E-commerce en Angular

**PC2 · Javascript Avanzado · UTP**

Tienda de ropa urbana (oversize, streetwear, preppy moderno y casual minimalista).
Es la evolución de la PC1 (HTML/CSS/JS puro) llevada a Angular con componentes,
rutas, formularios, servicios REST, Bootstrap y despliegue online.

- **Link desplegado:** _pega aquí el enlace de Netlify / Vercel / GitHub Pages_
- **Integrantes:** _nombres del grupo_

## Tecnologías

Angular 17 (componentes standalone) · TypeScript · Bootstrap 5 · JSON Server · HTML · CSS

## Cómo ejecutarlo

Requisitos: Node.js 18.13 o superior.

```bash
npm install
npm run dev      # levanta JSON Server (puerto 3000) y Angular (puerto 4200)
```

También se pueden levantar por separado en dos terminales:

```bash
npm run api      # JSON Server  -> http://localhost:3000/productos
npm start        # Angular      -> http://localhost:4200
```

## Cuentas de prueba (login simulado)

| Rol | Correo | Contraseña |
|---|---|---|
| Administrador | admin@ivyoak.pe | admin123 |
| Cliente | cliente@ivyoak.pe | cliente123 |

La sesión se guarda en LocalStorage (`ivyoak_sesion`). Si escribes `/dashboard`
sin haber iniciado sesión, el guard te redirige al Home.

## Páginas

| Ruta | Página |
|---|---|
| `/` | Home |
| `/productos` | Productos (buscador + filtro por categoría, datos desde JSON Server) |
| `/ofertas` | Ofertas (productos con `precioOferta`) |
| `/tienda` | Tienda: reserva de prendas con validación de talla y stock + guía de tallas |
| `/contacto` | Contacto (formulario con validaciones) |
| `/mi-cuenta` | Mi cuenta (datos de sesión y reservas guardadas) |
| `/login` | Login |
| `/dashboard` | Dashboard protegido por `authGuard` |
| `**` | Página 404 personalizada |

## Estructura

```
src/app
├── components/    navbar, footer, ticker, producto-card (reutilizables)
├── pages/         una carpeta por página
├── services/      producto.service (REST), auth.service, reserva.service
├── guards/        auth.guard (protege /dashboard)
├── models/        producto.ts
├── app.routes.ts  rutas + 404
└── app.config.ts  router + HttpClient
db.json            base de datos simulada (JSON Server)
```

## Cumplimiento del enunciado

- **Navbar y footer reutilizables, arquitectura por componentes:** `components/`
- **Routing, 404 y redirección del Dashboard:** `app.routes.ts` y `guards/auth.guard.ts`
- **Login con ngModel, validaciones y LocalStorage, con cerrar sesión:** `pages/login`, `services/auth.service.ts`
- **Data binding:** interpolación `{{ }}`, property binding `[src]`, event binding `(click)`, two-way `[(ngModel)]`
- **Directivas:** `*ngFor`, `*ngIf`, `[ngClass]`, `[ngStyle]` (barra de stock en `producto-card`)
- **Pipes:** `currency`, `uppercase`, `date`, `lowercase`, `titlecase`
- **Bootstrap:** grid responsive, cards, navbar, formularios, botones y tablas
- **db.json con nombre, imagen, categoria, precio y stock** (más `tallas`, `descripcion` y `precioOferta`)
- **Consumo REST con servicio Angular:** `services/producto.service.ts`

## Datos y JSON Server en producción

`db.json` en la raíz es la única fuente de datos. Los scripts `prestart` y `prebuild`
la copian a `src/assets/db.json`.

- En desarrollo, `ProductoService` consume `http://localhost:3000/productos` (JSON Server).
- En el sitio desplegado no hay JSON Server, así que el servicio lee `assets/db.json`.
  Si alojas la API online (por ejemplo en Render), cambia `apiUrl` en `producto.service.ts`.

`db.json` trae 12 productos (8 en oferta). Los 4 primeros usan fotos externas y los demás ilustraciones locales en `src/assets/productos/`. Si una foto externa falla, la tarjeta muestra `assets/placeholder.svg`. Los valores de `stock` son de ejemplo.

## Despliegue

Compilar: `npm run build` (salida en `dist/ivy-oak/browser`).

- **Netlify:** conecta el repositorio. `netlify.toml` ya trae comando, carpeta y redirecciones.
- **Vercel:** importa el repositorio. `vercel.json` ya trae comando, carpeta y rewrites.
- **GitHub Pages / Firebase Hosting:** sube el contenido de `dist/ivy-oak/browser`.
  Configura la redirección de todas las rutas a `index.html` para que `/dashboard` y el 404 funcionen al recargar.

## Checklist de entrega

- [ ] Subir el proyecto a GitHub (un commit por integrante como mínimo)
- [ ] Desplegar en Netlify o Vercel y pegar el link arriba
- [ ] Completar los nombres de los integrantes arriba
- [ ] Probar en el sitio desplegado: recargar `/productos`, entrar a `/dashboard` sin sesión (debe ir al Home) y escribir una ruta inexistente (debe salir el 404)
