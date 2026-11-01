# Sunrise Habits

Landing page para vender plantillas de **habit tracker** en PDF (imprimibles y editables en pantalla). Está pensada para enlazarse desde la bio de Instagram y cobra con **Stripe Payment Links**.

Es una web estática de un solo archivo: sin dependencias, sin paso de compilación y sin cookies ni analítica.

## Qué incluye la web

- Portada con la propuesta de valor y la frase de Will Durant.
- Sección de ejemplo con la plantilla rellenada (5, 10 y 15 hábitos), incrustada en el propio `index.html`.
- Selector de precios por número de hábitos (5, 10, 15) y duración (1, 3, 6 o 12 meses).
- Dos packs: **Pack Prueba** y **Pack Año Completo**.
- Casilla de aceptación obligatoria antes de comprar (términos, privacidad y desistimiento de contenido digital).
- Contacto, preguntas frecuentes e información legal (privacidad, términos y aviso legal) en desplegables dentro de la misma página.

## Estructura del repositorio

```
/
├── index.html              # Toda la web (HTML + CSS + JS)
├── ficha-1-escritorio.jpg  # Imagen de portada (opcional; si falta, se oculta)
└── README.md
```

> **No subas al repositorio los PDFs que vendes.** Todo lo que está en el repo se publica en la web y cualquiera podría descargarlo sin pagar. Guarda los PDFs aparte y entrégalos solo tras el pago (por ejemplo, con la página o el mensaje de confirmación de Stripe).

## Despliegue en Vercel

1. Sube `index.html` (y la imagen de portada) a un repositorio de GitHub.
2. En [Vercel](https://vercel.com), pulsa **Add New → Project** e importa el repositorio.
3. Framework Preset: **Other**. Deja vacíos el *Build Command* y el *Output Directory*.
4. Pulsa **Deploy**. Cada `git push` a la rama principal se despliega automáticamente.
5. Opcional: añade tu dominio en **Settings → Domains** y pon esa URL en la bio de Instagram.

## Configurar los pagos (Stripe)

Crea un **Payment Link** por cada producto y pega su URL en el objeto `LINKS` del `<script>` de `index.html` (ahora todos valen `https://buy.stripe.com/REEMPLAZA`).

| Clave | Producto | Precio |
|---|---|---|
| `5-1`, `5-3`, `5-6`, `5-12` | 5 hábitos: 1, 3, 6 y 12 meses | 2,90 / 5,90 / 8,90 / 12,90 € |
| `10-1`, `10-3`, `10-6`, `10-12` | 10 hábitos: 1, 3, 6 y 12 meses | 3,90 / 7,90 / 11,90 / 16,90 € |
| `15-1`, `15-3`, `15-6`, `15-12` | 15 hábitos: 1, 3, 6 y 12 meses | 4,90 / 9,90 / 14,90 / 19,90 € |
| `pack-prueba` | 3 tamaños, 1 mes | 7,90 € |
| `pack-ano` | 3 tamaños, 12 meses | 29,90 € |

Recomendaciones para cada Payment Link:

- Precios **con IVA incluido** (la web los muestra así).
- Activa la **casilla de aceptación de términos** con el texto: *"Acepto los Términos y condiciones y la Política de privacidad. Solicito el acceso inmediato al contenido digital y reconozco que pierdo mi derecho de desistimiento una vez iniciada la descarga."* Stripe guarda esa aceptación, que sirve como prueba. La URL de términos en Stripe puede ser `https://tu-dominio/#terminos`.
- Configura el mensaje o la redirección posterior al pago con la entrega de los archivos.

> Los nombres de las opciones en el panel de Stripe pueden cambiar con el tiempo; consulta su documentación.

## Cambiar precios

Los precios están en **dos sitios**; actualiza ambos:

1. En el `<script>`: el objeto `PRICES` (precios por tamaño y duración).
2. En el HTML de la sección "Packs con todo incluido": los importes de los dos packs y sus precios tachados.

## Antes de publicar

- [ ] Sustituir `[NIF]`, `[NOMBRE Y APELLIDOS DEL TITULAR]` y `[DOMICILIO]` en los textos legales (están dentro de `index.html`).
- [ ] Comprobar que el correo de contacto es correcto (aparece en la sección Contacto).
- [ ] Sustituir los 14 enlaces `REEMPLAZA` por los Payment Links reales.
- [ ] Subir la imagen de portada junto a `index.html`.
- [ ] Probar la compra completa en modo de prueba de Stripe, desde el móvil y desde el navegador de Instagram.
- [ ] Revisar con un gestor o asesor el IVA, el alta como autónomo y los textos legales.

## Aviso

Los textos legales son una base de partida y **no constituyen asesoramiento jurídico**. Revísalos con un profesional. Si añades analítica, anuncios, cookies o formularios de suscripción, actualiza la política de privacidad.

## Licencia

© 2026 Sunrise Habits. Todos los derechos reservados. El diseño, los textos y las plantillas no pueden copiarse, redistribuirse ni revenderse sin autorización.
