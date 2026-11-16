---
published: false
---

# Multiquímica

Sitio web institucional y comercial de Multiquímica, en español. Presenta productos químicos para piscinas, tratamiento de aguas y usos industriales, información de la empresa y logística, consejos, contacto y postulaciones laborales.

El repositorio contiene un sitio estático de 23 páginas HTML: siete páginas principales y dieciséis fichas de productos. No se encontró una aplicación de comercio electrónico, backend propio ni base de datos en el código.

## Tecnologías

- HTML, SCSS y CSS compilado; JavaScript nativo en `assets/js/main.js`.
- Bibliotecas locales en `assets/vendor/`: Bootstrap 5.3.8, Bootstrap Icons 1.13.1, AOS, GLightbox, imagesLoaded 5.0.0, Isotope 3.0.6 y PureCounter 1.5.0. Las versiones indicadas se leen de las cabeceras de los archivos; AOS y GLightbox no tienen una versión confirmada en esta revisión.
- Google Fonts para Roboto, Lato y Raleway; Google Analytics en las páginas; Google Maps en contacto y FormSubmit para los dos formularios.
- No hay `package.json`, lockfile, scripts npm, configuración de bundler ni suite de pruebas. No se necesita instalar dependencias Node para servir el sitio.

## Estructura

```text
.
├── index.html
├── nosotros.html
├── logistica.html
├── productos.html
├── tips-y-consejos.html
├── contacto.html
├── trabaja-con-nosotros.html
├── productos/              # 16 fichas HTML
├── assets/
│   ├── scss/               # Fuentes: main.scss, variables, layouts y sections
│   ├── css/                # main.css y main.min.css generados
│   ├── js/                 # main.js
│   ├── vendor/             # Bibliotecas incluidas en el repositorio
│   ├── img/                # Logos, fotografías e imágenes de productos
│   └── documentos/         # 11 PDF de hojas de datos de seguridad
├── CNAME                   # multiquimica.cl
├── robots.txt
├── sitemap.xml
├── llms.txt
├── Readme.txt              # Nota original conservada
└── README.md
```

## Desarrollo local

Desde la raíz, con Python 3 disponible:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Abrir `http://127.0.0.1:8000/` y detener con `Ctrl+C`. El servidor sirve archivos estáticos; no reproduce la configuración de alojamiento. La vista local conserva las integraciones externas del HTML y necesita conexión para cargarlas. Los formularios siguen apuntando al servicio real: no enviar datos como parte de una simple revisión local.

Editar los HTML existentes, respetando sus clases y rutas relativas. Cabecera, navegación, pie y metadatos están repetidos en cada página; revisar todas las apariciones cuando cambie un elemento común. Las fichas de `productos/` acceden a recursos con `../assets/`.

## Estilos

La fuente es `assets/scss/main.scss`. Importa las variables y parciales de `layouts/`, además de `_sections.scss`, que reúne los parciales de `sections/`. Las páginas cargan `assets/css/main.min.css`; `main.css` también se conserva en Git.

Modificar los parciales SCSS y regenerar ambos archivos, sin crear hojas CSS paralelas. En el equipo revisado existe Dart Sass 1.89.2 en `/usr/local/bin/sass`:

```bash
/usr/local/bin/sass assets/scss/main.scss assets/css/main.css --style=expanded --no-source-map
/usr/local/bin/sass assets/scss/main.scss assets/css/main.min.css --style=compressed --no-source-map
```

En otro equipo, usar la ruta del ejecutable Sass instalado. Su instalación y versión no están fijadas por el repositorio. `main.scss` menciona Easy Compile para VS Code como alternativa, pero no hay configuración de esa extensión versionada.

La compilación temporal con Sass 1.89.2 se comprobó el 16 de noviembre de 2026. Los `@import` existentes generan advertencias de obsolescencia; la compilación termina correctamente. No hay un paso de compilación automático configurado en este repositorio.

## Formularios y JavaScript

- `contacto.html`: nombre, teléfono, correo, asunto y mensaje.
- `trabaja-con-nosotros.html`: nombre, correo, teléfono y mensaje de postulación; no hay carga de CV en el HTML.
- Ambos usan POST a FormSubmit, al mismo destino. No existe manejador propio de envío ni backend PHP en el repositorio. La recepción, destinatarios y configuración del proveedor deben comprobarse por separado.
- `main.js` gestiona navegación móvil, estados de scroll, preloader, animaciones, indicadores de carrusel, lightbox, filtros Isotope, FAQ y contadores cuando están presentes los elementos correspondientes.

## Comprobaciones disponibles

No hay un comando automatizado de pruebas. Para cada cambio, elegir comprobaciones acordes al alcance:

```bash
git diff --check
node --check assets/js/main.js
python3 -c 'import xml.etree.ElementTree as ET; ET.parse("sitemap.xml"); print("sitemap.xml: XML válido")'
```

La comprobación JavaScript requiere Node y valida sintaxis, no comportamiento. La del sitemap valida XML, no URLs ni SEO. Para cambios SCSS, ejecutar la compilación anterior; para cambios públicos, revisar en navegador las páginas afectadas, navegación, enlaces, recursos, tamaños móvil/escritorio, teclado, etiquetas y textos alternativos. Revisar también canonical, metadatos y JSON-LD cuando corresponda. No enviar formularios reales sin autorización para el envío.

## Publicación

GitHub Pages publica mediante su workflow administrado `pages-build-deployment`, sobre la rama `main`, con compilación Jekyll. Este mecanismo se verificó en las ejecuciones y deployments públicos el 16 de noviembre de 2026. No hay un workflow propio, `_config.yml` o `.nojekyll` en el repositorio. La carpeta exacta del panel no se consultó directamente.

El remoto `origin` corresponde a `MariaJoseVillasante/multiquimica` y `CNAME` declara `multiquimica.cl`.

Este README puede versionarse en GitHub y declara `published: false` en su encabezado YAML para excluirlo del sitio generado por Jekyll, sin añadir configuración de despliegue. El control está documentado en [Front Matter de Jekyll](https://jekyllrb.com/docs/front-matter/). Esta exclusión corresponde al build de Jekyll; un servidor que copie o sirva archivos directamente debe tratar el README por separado.

Antes de una publicación autorizada:

1. Comprobar que el mecanismo de Pages continúa publicando desde `main` y revisar en Settings → Pages la fuente configurada, especialmente si cambia el despliegue.
2. Compilar los estilos si cambiaron y completar las comprobaciones correspondientes.
3. Revisar los archivos que se publicarían: HTML, CSS generado, JavaScript, bibliotecas, imágenes, PDF y archivos SEO. Conservar `CNAME` y las rutas existentes. No cargar el directorio local completo con archivos internos ignorados.
4. Obtener autorización explícita para commit, push y despliegue. Solo entonces seguir el mecanismo confirmado en Pages y verificar el resultado y el dominio.

No hay archivos de configuración de Cloudflare identificados. DNS, proxy, TLS y reglas del panel están pendientes de verificar; no deben deducirse de `CNAME`. Esta documentación no ejecuta ninguna publicación.
