# Sunrise Habits

Landing page en HTML, CSS y JavaScript puro para vender plantillas de **habit tracker** en PDF (imprimibles y editables en pantalla), con pagos mediante **Stripe Payment Links** y despliegue en **Vercel**. Pensada para enlazarse desde la bio de Instagram.

> **Estado:** la web está lista, pero **Stripe todavía no está conectado**: los botones de compra apuntan a un enlace de ejemplo. Sigue la sección [Conectar Stripe](#conectar-stripe-paso-a-paso) antes de vender.

## Qué incluye la web

- Portada con la propuesta de valor y una frase de Will Durant.
- Sección **"Así es por dentro"** con un ejemplo rellenado de la plantilla (5, 10 y 15 hábitos), incrustado en el propio `index.html`.
- Selector de precios por número de hábitos (5, 10 o 15) y duración (1, 3, 6 o 12 meses).
- Dos packs: **Pack Prueba** y **Pack Año Completo**.
- Casilla de aceptación obligatoria antes de comprar (términos, privacidad y desistimiento de contenido digital).
- Preguntas frecuentes, sección de **Contacto** e **Información legal** (privacidad, términos y aviso legal) en desplegables dentro de la misma página.
- Sin cookies, sin analítica y sin dependencias externas.

## Estructura del repositorio

```
/
├── index.html              # Toda la web (HTML + CSS + JS)
├── ficha-1-escritorio.jpg  # Imagen de portada (opcional; si falta, se oculta)
└── README.md
```

> **No subas al repositorio los PDFs que vendes.** Todo lo que está en el repo se publica en la web y cualquiera podría descargarlo sin pagar. Guárdalos aparte y entrégalos solo tras el pago.

## Despliegue en Vercel

1. Sube `index.html` (y la imagen de portada) a un repositorio de GitHub.
2. En [Vercel](https://vercel.com), pulsa **Add New → Project** e importa el repositorio.
3. Framework Preset: **Other**. Deja vacíos el *Build Command* y el *Output Directory*.
4. Pulsa **Deploy**. Cada `git push` a la rama principal se despliega automáticamente.
5. Opcional: añade tu dominio en **Settings → Domains** y pon esa URL en la bio de Instagram.

## Conectar Stripe (paso a paso)

La web usa **Payment Links**: no necesita servidor ni claves de API en el código. Cada botón de compra abre una página de pago alojada por Stripe. Trabaja primero en **modo de prueba** (sandbox).

### 1. Preparar la cuenta

1. Crea tu cuenta en [stripe.com](https://stripe.com) y completa los datos del negocio y la verificación de identidad.
2. Añade la cuenta bancaria (IBAN) donde quieres cobrar.
3. En los ajustes de datos públicos del negocio, indica el nombre comercial (Sunrise Habits), el correo de soporte y las URL de **términos** y **privacidad**. Como están dentro de la misma página, serían `https://tu-dominio/#terminos` y `https://tu-dominio/#privacidad`.

### 2. Activar Stripe Tax (IVA)

1. Entra en el apartado **Tax** y configura la dirección de tu negocio y tus registros fiscales (España y, si procede, el régimen de ventas a consumidores de la UE).
2. Asigna a cada producto el **código fiscal** de producto digital que corresponda.
3. Crea los precios con el impuesto **incluido**, porque la web los muestra con IVA incluido.

> Consulta con tu gestor cómo declarar el IVA de los productos digitales. Stripe también ofrece **Managed Payments**, donde Stripe actúa como vendedor oficial y se encarga del IVA; su disponibilidad depende de una revisión de tu cuenta.

### 3. Crear los 14 productos y Payment Links

Crea un producto con precio único en euros y un Payment Link por cada fila de esta tabla:

| Clave | Producto | Precio |
|---|---|---|
| `5-1`, `5-3`, `5-6`, `5-12` | 5 hábitos: 1, 3, 6 y 12 meses | 2,90 / 5,90 / 8,90 / 12,90 € |
| `10-1`, `10-3`, `10-6`, `10-12` | 10 hábitos: 1, 3, 6 y 12 meses | 3,90 / 7,90 / 11,90 / 16,90 € |
| `15-1`, `15-3`, `15-6`, `15-12` | 15 hábitos: 1, 3, 6 y 12 meses | 4,90 / 9,90 / 14,90 / 19,90 € |
| `pack-prueba` | 3 tamaños, 1 mes | 7,90 € |
| `pack-ano` | 3 tamaños, 12 meses | 29,90 € |

En cada Payment Link:

- Activa el **cobro automático de impuestos** y recoge la dirección de facturación.
- Activa la **casilla de aceptación de términos** (Stripe guarda la aceptación como prueba) con este texto: *"Acepto los Términos y condiciones y la Política de privacidad. Solicito el acceso inmediato al contenido digital y reconozco que pierdo mi derecho de desistimiento una vez iniciada la descarga."*
- Configura el **mensaje o la redirección posterior al pago** con la entrega de los archivos (ver siguiente punto).

> Los nombres de las opciones en el panel de Stripe pueden cambiar con el tiempo; consulta su documentación.

### 4. Preparar la entrega de los PDFs

Los PDFs no van en el repositorio. Para empezar, puedes enviar al cliente, tras el pago, un enlace de descarga a un almacenamiento privado. Es una solución básica, porque quien tenga ese enlace puede compartirlo. Más adelante se puede mejorar con enlaces de descarga que caduquen, generados por una función de Vercel que reciba un aviso (webhook) de Stripe al completarse el pago.

### 5. Pegar los enlaces en la web

En el `<script>` de `index.html`, sustituye cada `https://buy.stripe.com/REEMPLAZA` del objeto `LINKS` por la URL de su Payment Link, usando la clave de la tabla. Haz `git push` y Vercel desplegará los cambios.

### 6. Probar y pasar a producción

1. En modo de prueba, compra con la tarjeta `4242 4242 4242 4242` (cualquier fecha futura y cualquier CVC) y comprueba el pago, el IVA y la entrega, desde el móvil y desde el navegador de Instagram.
2. Cuando todo funcione, **repite los pasos 3 y 5 en modo real**: los productos y enlaces de prueba no se trasladan automáticamente. Sustituye en `LINKS` las URL de prueba por las reales.

> **Nunca** pegues claves secretas de Stripe (`sk_...`) en `index.html` ni en el repositorio. Con Payment Links no hacen falta; si en el futuro añades una función de Vercel, guárdalas como variables de entorno.

## Cambiar precios

Los precios están en **dos sitios**; actualiza ambos, y también el precio del Payment Link en Stripe:

1. En el `<script>`: el objeto `PRICES` (precios por tamaño y duración).
2. En el HTML de la sección "Packs con todo incluido": los importes de los dos packs y sus precios tachados.

## Antes de publicar

- [ ] Sustituir `[NIF]`, `[NOMBRE Y APELLIDOS DEL TITULAR]` y `[DOMICILIO]` en los textos legales (están dentro de `index.html`).
- [ ] Comprobar que el correo de contacto es correcto (aparece en Contacto y en los textos legales).
- [ ] Conectar Stripe y sustituir los 14 enlaces `REEMPLAZA` por los Payment Links reales.
- [ ] Subir la imagen de portada junto a `index.html`.
- [ ] Probar la compra completa en modo de prueba.
- [ ] Revisar con un gestor o asesor el IVA, el alta como autónomo y los textos legales.

## Aviso

Los textos legales son una base de partida y **no constituyen asesoramiento jurídico**. Revísalos con un profesional. Si añades analítica, anuncios, cookies o formularios de suscripción, actualiza la política de privacidad.

## Licencia

© 2026 Sunrise Habits. Todos los derechos reservados. El diseño, los textos y las plantillas no pueden copiarse, redistribuirse ni revenderse sin autorización.
