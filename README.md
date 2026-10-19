# Café Raíces — Sitio web de negocio local

> Proyecto académico de HTML5 y CSS. El negocio, la dirección, las opiniones, los precios y el correo son ficticios y deben sustituirse por datos autorizados antes de usar el sitio para un negocio real.

## 1. El proyecto

Café Raíces es una cafetería de barrio ficticia que ofrece café colombiano, bebidas preparadas y panadería artesanal. El sitio está dirigido a personas que quieren conocer el menú, revisar precios, consultar el horario y dejar una solicitud de pedido sin tener que preguntar todos los datos por redes sociales.

El cliente necesita una presencia digital sencilla, confiable y usable desde un celular. Por eso hay dos páginas: `index.html` presenta el negocio y `pedidos.html` muestra los precios y un formulario de contacto.

## 2. Estructura

```text
cafe-raices/
├── index.html
├── pedidos.html
├── README.md
├── css/
│   └── styles.css
├── images/
│   ├── logo.svg
│   ├── cafe-hero.svg
│   ├── espresso.svg
│   ├── latte.svg
│   └── pan.svg
├── video/
└── capturas/
```

- `header`: contiene la marca y la navegación compartida.
- `nav`: agrupa enlaces para moverse entre páginas y secciones.
- `main`: delimita el contenido principal de cada página.
- `section`: separa bloques temáticos como productos, opiniones, ubicación y formulario.
- `article`: representa una tarjeta de producto o una opinión independiente.
- `aside`: presenta un consejo complementario que no es el contenido central.
- `footer`: reúne navegación secundaria y nota académica.
- `table`: organiza precios en filas y columnas; `rowspan` agrupa categorías y `colspan` destaca la promoción.
- `form`: recoge datos del visitante. Cada control tiene su `label`.

Las rutas a CSS e imágenes son relativas, por ejemplo `css/styles.css` e `images/logo.svg`. El enlace externo al mapa se abre en otra pestaña con `target="_blank"` y `rel="noopener noreferrer"`.

## 3. Sistema visual

Las variables se declaran al principio de `css/styles.css` en `:root` para mantener una paleta consistente:

- `--color-forest` (`#24483b`): verde oscuro para botones y secciones principales.
- `--color-cream` (`#f7f1e7`): fondo cálido que separa las secciones.
- `--color-coffee` (`#754b35`): acento relacionado con el café.
- `--color-caramel` (`#d5a36c`): detalle cálido para promociones y focos visuales.
- `--font-main`: usa `"Trebuchet MS"` y, como respaldo, `Arial, sans-serif`.
- `--content-width`: limita el ancho para que el contenido no se extienda demasiado en pantallas grandes.

También se utiliza un color con canal alfa, por ejemplo `rgb(35 48 39 / 10%)`, y fondos con gradiente, por ejemplo `linear-gradient(145deg, #e7d9bf, #f7f1e7 58%, #dce5d7)`.

## 4. Especificidad

La especificidad se expresa como **(IDs, clases/atributos/pseudoclases, etiquetas/pseudoelementos)**. Los siguientes conflictos están escritos en la hoja del proyecto; las capturas de inspector deben tomarse en el navegador después de abrir el sitio con Live Server.

### Conflicto 1: título dentro de una tarjeta

Reglas que compiten sobre el `h3` de la tarjeta:

- `.product-card__body h3` → **(0,1,1)**; define el tamaño de fuente.
- `.product-card__body h3:first-child` → **(0,2,1)**; define el margen inferior.

No son un conflicto sobre la misma propiedad: la segunda regla agrega el margen y gana solo si ambas declararan la misma propiedad. Para una comparación visual real, en DevTools se puede inspeccionar `margin-bottom` y comprobar que la regla más específica que lo define aparece aplicada.

### Conflicto 2: color de enlaces

- `a` → **(0,0,1)**; define el color normal.
- `.footer-nav a` → **(0,1,1)**; define el color de los enlaces del pie.

Gana `.footer-nav a` para `color` porque contiene una clase además de una etiqueta. Se puede inspeccionar un enlace del pie de página.

### Conflicto 3: enlaces activos de navegación

- `.site-nav__link` → **(0,1,0)**; declara color y fondo base.
- `.site-nav__link--active` → **(0,1,0)**; declara negrita.
- `.site-nav__link:hover` → **(0,2,0)**; cambia color y fondo cuando el puntero pasa por encima.

Para demostrar un empate real por orden, agrega temporalmente estas dos reglas al final de la hoja y observa el color de un enlace normal:

```css
/* Prueba temporal de empate: ambas especificidades son (0,1,0). */
.site-nav__link { color: #24483b; }
.site-nav__link { color: #754b35; }
```

La segunda regla gana porque ambas tienen la misma especificidad y aparece después. Retira estas reglas de prueba antes de la entrega definitiva para conservar el diseño.

**Capturas del inspector:** guarda en `capturas/` una imagen por conflicto con DevTools → Elements/Inspector → Styles. La declaración perdedora debe verse tachada. Las capturas deben corresponder al navegador real; no se incluyen imágenes simuladas.

## 5. Herencia

Una propiedad que se hereda es `font-family`: se declara en `body`, y el texto de tarjetas, secciones y pie de página la utiliza salvo que un control del navegador tenga una regla propia. Se observa, por ejemplo, en el texto de los productos.

Una propiedad que normalmente no se hereda es `border`: el borde del contenedor `.product-card` no se aplica automáticamente a sus párrafos o títulos hijos. Cada elemento tendría que declarar su propio borde si lo necesitara.

## 6. Modelo de cajas

En el modelo estándar aplicado por `box-sizing: border-box`, el ancho declarado incluye contenido, padding y borde. Para entender el cálculo clásico de `.contact-form`, supongamos un ancho de contenido de `600px`, padding horizontal de `32px` por lado y borde de `1px` por lado:

`ancho total = 600 + 32 + 32 + 1 + 1 = 666px`.

En este proyecto se activa `box-sizing: border-box` para que el ancho declarado incluya padding y borde, facilitando el diseño responsive. La tarjeta de formulario usa `padding: 2rem` en escritorio y reduce el padding en móvil. Los márgenes verticales de elementos de bloque pueden colapsar en ciertos contextos; por ejemplo, los márgenes verticales de dos párrafos consecutivos pueden solaparse en lugar de sumarse. En contenedores flex/grid, los márgenes entre elementos no colapsan de la misma manera.

## 7. Selectores

La regla `.product-card:nth-child(2)` selecciona una tarjeta que sea el segundo hijo de su elemento padre. Cuenta los hermanos elementales, aunque sean de etiquetas distintas; no significa “el segundo elemento de tipo `article`”. En esta página, el segundo hijo de `.product-grid` es la tarjeta del latte, por lo que recibe un borde superior color caramelo.

Otros selectores usados:

- `[required]` selecciona controles que tienen el atributo `required`.
- `.site-nav__link:hover` usa una pseudoclase de interacción.
- `::marker` cambia el marcador de los elementos de lista.
- `.product-card__body h3:first-child::before` agrega un detalle decorativo antes del título.

## 8. Responsive

En `@media (max-width: 700px)` la navegación se apila, las columnas de portada y contenido pasan a una columna, se reducen los espacios y el formulario ocupa el ancho disponible. La tabla conserva un ancho mínimo y se coloca dentro de `.table-wrap`, que permite desplazamiento horizontal solo en esa zona en pantallas estrechas. Así no fuerza a toda la página a desbordarse en un móvil de 360px.

Se recomienda comprobarlo en DevTools con ancho de 360px y revisar también la navegación por teclado con `Tab`.

## 9. Problemas y soluciones

Estos son casos de prueba documentados que deben comprobarse al ejecutar el sitio:

1. **La tabla podía ensanchar toda la página en móvil.** Una tabla con varias columnas necesita espacio. Se resolvió con `.table-wrap { overflow-x: auto; }` y un `min-width` aplicado a la tabla dentro del media query, de modo que el desplazamiento queda limitado a la tabla.
2. **Las tarjetas podían verse demasiado apretadas en pantalla pequeña.** La cuadrícula de tres columnas no cabía cómodamente. En `@media (max-width: 700px)` `.product-grid` cambia a una sola columna.
3. **El formulario no envía datos a un servidor.** Se dejó `action="mailto:pedidos@caferaices.example"` como demostración de la estructura HTML. Para producción se debe reemplazar por un endpoint real o un servicio de formularios y probar el flujo completo.

## 10. Capturas

Guarda las capturas reales dentro de `capturas/` con estos nombres sugeridos y reemplaza los marcadores por las imágenes antes de publicar:

### Página de inicio — escritorio

![Inicio en escritorio](capturas/inicio-escritorio.png)

### Página de inicio — 360 px

![Inicio a 360 px](capturas/inicio-360.png)

### Menú y pedidos — escritorio

![Pedidos en escritorio](capturas/pedidos-escritorio.png)

### Menú y pedidos — 360 px

![Pedidos a 360 px](capturas/pedidos-360.png)

### Inspector — conflictos de especificidad

Guarda capturas del panel **Styles** para cada conflicto real y enlázalas aquí:

- `capturas/inspector-conflicto-1.png`
- `capturas/inspector-conflicto-2.png`
- `capturas/inspector-conflicto-3.png`

> Las imágenes anteriores son nombres esperados, no capturas ya generadas. Debes tomarlas en tu navegador para que reflejen el resultado real de tu ejecución.

**Publicación:** después de subir el repositorio público a GitHub, añade aquí el enlace de GitHub Pages. La publicación debe configurarse desde Settings → Pages y verificarse abriendo el enlace en una ventana de incógnito.

---

## Cómo ejecutar el proyecto

1. Abre la carpeta `cafe-raices` en Visual Studio Code.
2. Instala la extensión **Live Server** si no la tienes.
3. Haz clic derecho sobre `index.html` y selecciona **Open with Live Server**.
4. Revisa los enlaces, anclas, tabla, formulario y mapa.
5. Usa las herramientas de desarrollador del navegador para probar 360 px, teclado y capturas.
6. Antes de entregar, reemplaza datos ficticios, toma las capturas reales y publica el repositorio.

## 
