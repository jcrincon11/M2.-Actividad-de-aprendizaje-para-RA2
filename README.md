# VentasSuite — Dashboard Administrativo

> Actividad Académica: Taller práctico de **CSS Grid**, **Flexbox** y **Variables CSS**  
> Curso: Diseño Web y Usabilidad · Universidad Distrital / Manuela Beltrán  
> Autor: Jean Carlo Rincón Ochoa

---

## Descripción

**VentasSuite** es un dashboard administrativo de analítica de ventas e inventario, desarrollado como ejercicio práctico para demostrar el dominio de las tecnologías de maquetación modernas de CSS: Grid Layout, Flexbox y Custom Properties (variables CSS). La interfaz presenta indicadores clave de rendimiento (KPIs), un gráfico de ventas mensuales y una tabla de pedidos recientes, todo dentro de un diseño oscuro (dark mode) responsivo y accesible.

---

## Estructura del Proyecto

```
dashboard-css-grid-flexbox/
├── assets/
│   ├── css/
│   │   └── styles.css          ← Hoja de estilos principal (todo el CSS)
│   └── img/
│       └── (reservado para imágenes del proyecto)
├── evidencias/
│   └── (capturas de pantalla — ver nota al final)
├── index.html                  ← Página principal del dashboard
└── README.md                   ← Este archivo
```

---

## Tecnologías Empleadas

| Tecnología | Uso en el proyecto |
|---|---|
| **HTML5 Semántico** | `<aside>`, `<header>`, `<main>`, `<footer>`, `<section>`, `<article>`, `<nav>`, `<table>`, `<time>` |
| **CSS3** | Transiciones, animaciones `@keyframes`, pseudo-clases y pseudo-elementos |
| **CSS Grid Layout** | Macro-estructura del layout (sidebar + header + main + footer) |
| **CSS Flexbox** | Componentes internos: header, nav, KPI grid, tabla, footer |
| **CSS Custom Properties** | Paleta de colores, tipografías, espaciados y breakpoints desde `:root` |
| **SVG inline** | Íconos decorativos, accesibles con `aria-hidden="true"` |
| **Google Fonts** | Syne (display/headings) · Syne Mono (KPI numéricos) · DM Sans (UI/body) |

> **Sin frameworks externos.** No se usa Bootstrap, Tailwind ni ninguna librería de UI. Todo es CSS y HTML puro (Vanilla).

---

## Fases de Desarrollo

### Fase 1 — Inicialización y Estructura Base
Creación de la estructura de carpetas del proyecto: `/assets/css`, `/assets/img`, `/evidencias`, con los archivos `index.html`, `styles.css` y `README.md`. El HTML incluye el boilerplate en español (`lang="es"`) con metadatos de descripción y viewport para responsividad.

### Fase 2 — HTML5 Semántico y Accesibilidad (A11y)
Uso estricto de etiquetas semánticas con roles ARIA explícitos:

- `<aside role="navigation">` → menú lateral
- `<header role="banner">` → barra superior
- `<main role="main">` → contenido principal
- `<footer role="contentinfo">` → pie de página

Jerarquía de encabezados correcta: `h1` (título del dashboard) → `h2` (títulos de secciones KPI, Gráfico, Tabla). Todos los elementos interactivos tienen `aria-label`. Las imágenes y SVGs decorativos usan `aria-hidden="true"`.

### Fase 3 — Variables CSS y CSS Grid (Layout Principal)
`:root` define **Custom Properties** para toda la paleta de colores, tipografías, espaciados y bordes redondeados — nunca valores hardcodeados en propiedades individuales.

El layout principal usa `grid-template-areas` en `.app-container`:

```css
.app-container {
  display: grid;
  grid-template-areas:
    "sidebar  header"
    "sidebar  main"
    "sidebar  footer";
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-rows: var(--header-height) 1fr auto;
  min-height: 100vh;
}
```

**¿Por qué Grid aquí?** Grid es la herramienta adecuada para layouts bidimensionales donde se necesita control simultáneo sobre filas y columnas. La macro-estructura del dashboard (sidebar fijo + área de contenido apilada) es exactamente ese caso.

### Fase 4 — Flexbox y Diseño Visual (Componentes Internos)
Flexbox se usa en:

- **Header**: `display: flex; justify-content: space-between` para los 3 grupos (izquierda/centro/derecha).
- **Sidebar nav**: `display: flex; flex-direction: column` para los enlaces verticales. `margin-top: auto` empuja el perfil al fondo.
- **KPI Grid**: `display: flex; flex-wrap: wrap; gap` para las 4 tarjetas en fila.
- **Celda de cliente en tabla**: `display: flex; align-items: center` para el avatar + nombre.
- **Footer**: `display: flex; justify-content: space-between` para copyright y metadatos.

**¿Por qué Flexbox aquí?** Flexbox es óptimo para distribución en una sola dimensión (una fila o una columna de elementos). Cada componente interno tiene esa naturaleza.

**Interactividad visual:**
- `transition: all 0.3s ease` en todos los elementos interactivos.
- `:hover` en tarjetas KPI (elevación con `transform: translateY(-3px)` y `box-shadow`).
- `:focus-visible` con `outline: 2px solid var(--accent)` en todos los elementos teclado-navegables (sin outline en clics de mouse).
- Animación `@keyframes growBar` con `animation-delay` escalonado en las barras del gráfico.

### Fase 5 — Diseño Responsivo (Media Queries)

| Breakpoint | Comportamiento |
|---|---|
| **> 1024px** (Desktop) | Layout completo. Sidebar visible, 4 KPIs en fila. |
| **≤ 1024px** (Tablet) | Sidebar reducido (220px), KPIs en 2 columnas. |
| **≤ 640px** (Móvil) | 1 sola columna. Sidebar colapsable con menú hamburguesa CSS-only. KPIs apilados. |

**Menú hamburguesa CSS puro:** Se usa un `<input type="checkbox">` oculto con el selector de hermano CSS `~` para controlar la visibilidad del sidebar sin JavaScript. El sidebar tiene `transform: translateX(-100%)` por defecto y `transform: translateX(0)` cuando el checkbox está activo.

---

## Decisiones de Diseño

### Tema oscuro (Dark Mode)
El dashboard usa un tema oscuro basado en colores navy profundos (`#0d1117`, `#161b22`) con un acento teal (`#00c9a7`). La elección responde a dos criterios: (1) reduce la fatiga visual en sesiones de monitoreo largas, y (2) mejora la legibilidad de datos numéricos densos.

### Tipografía
- **Syne** (display): personalidad fuerte y geométrica para headings y la marca.
- **Syne Mono** (monoespaciada): lectura de valores numéricos tabulares (KPIs, IDs, montos).
- **DM Sans** (sans-serif): claridad y neutralidad para el texto de interfaz.

### Paleta validada para contraste (WCAG)
| Color | Uso | Ratio (aprox.) | WCAG |
|---|---|---|---|
| `#e6edf3` sobre `#0d1117` | Texto principal | 13.2:1 | ✓ AAA |
| `#8b949e` sobre `#0d1117` | Texto secundario | 4.8:1 | ✓ AA |
| `#00c9a7` sobre `#161b22` | Acento/interactivo | 7.1:1 | ✓ AA |
| `#3fb950` sobre `#161b22` | Estado success | 5.2:1 | ✓ AA |
| `#f85149` sobre `#161b22` | Estado danger | 4.6:1 | ✓ AA |

---

## Pautas de Accesibilidad (WCAG) Aplicadas

| Criterio | Implementación |
|---|---|
| **1.1.1** (Texto alternativo) | SVGs decorativos con `aria-hidden="true"`; íconos funcionales con `aria-label` |
| **1.3.1** (Info y relaciones) | Etiquetas HTML semánticas + roles ARIA explícitos |
| **1.3.2** (Secuencia significativa) | Jerarquía de headings h1→h2 sin saltos |
| **1.4.3** (Contraste mínimo) | Ratios ≥ 4.5:1 en texto normal, ≥ 3:1 en texto grande |
| **2.1.1** (Teclado) | Todos los elementos interactivos son alcanzables con Tab; tabla con `tabindex="0"` para scroll |
| **2.4.3** (Orden del foco) | El DOM sigue el orden visual lógico |
| **2.4.7** (Foco visible) | `:focus-visible` con outline de 2px en color acento para todos los interactivos |
| **3.1.1** (Idioma de la página) | `<html lang="es">` |
| **4.1.2** (Nombre, función, valor) | `aria-label`, `aria-current`, `aria-haspopup`, `aria-expanded`, `role` explícitos |

