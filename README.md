# KOSAS STUDIO — Catálogo de Objetos & Piezas de Diseño

KOSAS STUDIO es una plataforma web estática orientada a la exhibición, catalogación y publicación de objetos de diseño contemporáneo, mobiliario auxiliar, iluminación y accesorios singulares.

---

## 🎯 Propósito de la Aplicación

El propósito principal del proyecto es ofrecer una experiencia visual comercial, sobria y de alto impacto para la gestión y exploración de un catálogo de piezas artesanales e industriales:

- **Autenticación Central:** Punto de entrada directo enfocado en la privacidad y acceso de clientes mediante un inicio de sesión claro y funcional.
- **Visualización y Consulta:** Catálogo estructurado en retícula con fotografías reales de estudio, fichas técnicas con materiales, dimensiones y garantías.
- **Detalle de Producto:** Vista individualizada para la inspección visual y técnica de cada pieza antes de su adquisición.
- **Catalogación Abierta:** Formulario estandarizado para que diseñadores y artesanos puedan incorporar nuevas piezas a la colección.
- **Información y Soporte al Cliente:** Páginas dedicadas a resolver dudas habituales, políticas de envío sustentable, garantías estructurales y canales de contacto directo.
- **Estética Brutalista Equilibrada:** Identidad visual inspirada en el brutalismo arquitectónico (retícula expuesta, bordes negros estructurales, tipografía Grotesk/Monospace de alto peso visual y un tono amarillo vibrante como acento de energía).
- **Arquitectura 100% Estática:** Desarrollado íntegramente con HTML5 semántico y CSS3 puro, prescindiendo totalmente de JavaScript y dependencias externas.

---

## 📁 Estructura del Encarpetado

```text
kosas-studio/
│
├── index.html                  # VISTA PRINCIPAL: Inicio de Sesión (Acceso de clientes)
├── README.md                   # Documentación técnica y modelos de datos
│
├── pages/                      # Pantallas secundarias de la aplicación
│   ├── catalogo.html           # Explorador y catálogo general de productos
│   ├── detalle-producto.html   # Vista técnica y detallada del objeto seleccionado
│   ├── agregar-cosa.html       # Formulario para publicar y catalogar una nueva pieza
│   ├── registro.html           # Formulario de alta para nuevos clientes
│   ├── faq.html                # Preguntas frecuentes sobre pedidos y producción
│   ├── envios-devoluciones.html# Tarifas de logística, embalaje ecológico y reembolsos
│   ├── garantia.html           # Cobertura técnica de 2 a 5 años según material
│   └── contacto.html           # Información de ubicación física y formulario de contacto
│
├── img/                        # Fotografías de producto en alta resolución
│   ├── lampara-hormigon.jpg
│   ├── silla-acero.jpg
│   ├── reloj-ceramica.jpg
│   ├── bandeja-madera.jpg
│   ├── jarron-terracota.jpg
│   └── altavoz-mesa.jpg
│
├── css/                        # Hojas de estilo modulares
│   ├── base.css                # Variables de color, reset, escala tipográfica y contenedor
│   ├── components.css          # Botones, navbar, tarjetas, badges, inputs y footer
│   ├── pages.css               # Grillas, vistas split y fichas de producto
│   └── style.css               # Hoja principal que consolida todos los módulos
│
└── js/                         # Arquitectura JavaScript planificada (documentación)
    ├── data.js                 # Colección de datos semilla y catálogo predeterminado
    ├── local-storage.js        # Módulo de persistencia local (usuarios y cosas)
    ├── alerts.js               # Gestor de notificaciones visuales y feedback
    ├── generators.js           # Generación de identificadores únicos, tokens y códigos
    ├── auth-controller.js      # Controlador de lógica para login y registro de usuarios
    └── cosas-controller.js     # Controlador de lógica para el registro y publicación de cosas
```

---

## 📦 Arquitectura JavaScript Planificada (`js/`)

En esta sección se documenta el rol y las responsabilidades de los módulos previstos para la capa lógica de la aplicación:

### 1. `js/data.js`
- **Propósito:** Centralizar los datos semilla y las constantes del catálogo.
- **Responsabilidades:**
  - Alojar el arreglo inicial de objetos con sus especificaciones técnicas completas (lámparas, sillas, relojes, bandejas, etc.).
  - Definir las categorías permitidas (`iluminacion`, `mobiliario`, `accesorios`, `estudio`, `decoracion`).
  - Proveer la estructura inicial para alimentar el almacenamiento local cuando el usuario ingresa por primera vez.

### 2. `js/local-storage.js`
- **Propósito:** Abstraer las operaciones de lectura y escritura en la API de `localStorage` del navegador.
- **Responsabilidades:**
  - Persistir el registro de nuevos usuarios y validar credenciales en el inicio de sesión.
  - Guardar y recuperar las nuevas piezas publicadas desde el formulario de catalogación.
  - Mantener el estado de la sesión activa y la lista de objetos favoritos o seleccionados por el cliente.

### 3. `js/alerts.js`
- **Propósito:** Proveer un sistema de alertas y avisos visuales alineado con la estética de diseño brutalista.
- **Responsabilidades:**
  - Mostrar notificaciones flotantes o mensajes en bloque para confirmar acciones (ej. "Objeto publicado exitosamente", "Sesión iniciada con éxito").
  - Gestionar alertas de advertencia o error cuando existan campos incompletos en los formularios.
  - Temporizar el cierre automático de las alertas y permitir el cierre manual por parte del usuario.

### 4. `js/generators.js`
- **Propósito:** Centralizar las funciones utilitarias para la generación de identificadores dinámicos, tokens de sesión y códigos de referencia.
- **Responsabilidades:**
  - **Generador de IDs de Usuario:** Generación de identificadores pseudoaleatorios únicos mediante prefijos controlados (ej. `usr_` + timestamp hexadecimal).
  - **Generador de Códigos de Catálogo:** Creación de referencias correlativas o alfanuméricas para las piezas publicadas (ej. `ART-107`, `ART-108`).
  - **Generador de Tokens de Sesión:** Emisión de tokens de autenticación aleatorios (simulación de tokens tipo Bearer/JWT) para almacenar la sesión activa en el navegador.
  - **Generador de Slugs / Nombres Limpios:** Normalización de nombres de objetos para URLs y nombres de archivo de imagen.

### 5. `js/auth-controller.js`
- **Propósito:** Controlador de interacción y validación para los flujos de autenticación (Login y Registro).
- **Responsabilidades:**
  - **En Inicio de Sesión (`index.html`):** Capturar el envío del formulario, verificar la existencia del correo y coincidencia de contraseña contra los datos en `local-storage.js`, solicitar un token a `generators.js`, registrar la sesión activa y redirigir a `pages/catalogo.html`.
  - **En Registro (`pages/registro.html`):** Validar que las contraseñas coincidan, comprobar que el correo no esté duplicado, verificar la aceptación obligatoria de términos, generar un nuevo ID mediante `generators.js`, instanciar el objeto bajo el esquema `modeloUsuario`, almacenarlo en persistencia y notificar mediante `alerts.js`.

### 6. `js/cosas-controller.js`
- **Propósito:** Controlador de lógica para el formulario de incorporación y publicación de objetos.
- **Responsabilidades:**
  - **En Publicación (`pages/agregar-cosa.html`):** Capturar los campos del formulario (nombre, precio, categoría, materiales, dimensiones, stock y descripción).
  - Sanitizar los valores numéricos y obtener el nuevo código de referencia mediante `generators.js` (ej. `ART-107`).
  - Procesar la selección de imagen o asignar una ilustración/fotografía predeterminada.
  - Construir el objeto literal correspondiente a `modeloCosa`, agregarlo a la lista de productos en `local-storage.js` y desplegar la alerta de confirmación con redirección al catálogo actualizado.

---

## 🗄️ Modelos de Datos (Objetos Literales)

A continuación se definen las estructuras de datos que representan los dos flujos principales de la aplicación: el registro de un cliente y la publicación de un objeto/cosa en el catálogo.

### 1. Modelo de Registro de Usuario

Representa la información capturada durante el proceso de alta de una cuenta de cliente:

```javascript
const modeloUsuario = {
  id: "usr_7849102",
  nombreCompleto: "Sofia Martínez",
  telefono: "+34 600 000 000",
  correoElectronico: "sofia@ejemplo.com",
  passwordHash: "a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3",
  terminosAceptados: true,
  fechaRegistro: "2026-10-15T17:45:00.000Z",
  estadoCuenta: "activo"
};
```

#### Descripción de Campos:
- `id` *(String)*: Identificador único asignado al usuario en el sistema.
- `nombreCompleto` *(String)*: Nombres y apellidos del cliente.
- `telefono` *(String)*: Número de teléfono para notificaciones de entrega y logística.
- `correoElectronico` *(String)*: Dirección de correo electrónico utilizada para el inicio de sesión.
- `passwordHash` *(String)*: Contraseña cifrada del usuario.
- `terminosAceptados` *(Boolean)*: Confirmación de lectura y aceptación de condiciones y privacidad.
- `fechaRegistro` *(String)*: Marca temporal en formato ISO de la fecha de alta.
- `estadoCuenta` *(String)*: Estado operativo del usuario (`"activo"`, `"suspendido"`).

---

### 2. Modelo de Publicación de una Cosa (Objeto)

Representa las propiedades técnicas, físicas y comerciales de cada pieza que se incorpora al catálogo:

```javascript
const modeloCosa = {
  idReferencia: "ART-101",
  nombre: "Lámpara Cilíndrica de Hormigón",
  precioUSD: 68.00,
  categoria: "iluminacion",
  materiales: "Hormigón gris pulido, latón natural cepillado",
  dimensiones: {
    diametroCm: 12.0,
    alturaCm: 22.0,
    pesoKg: 2.4
  },
  stockDisponible: 14,
  descripcion: "Luminaria de sobremesa con base moldeada en cemento pulido hidrófugo y casquillo de latón macizo con rosca E27 para bombilla de filamento expuesto.",
  rutaImagen: "img/lampara-hormigon.jpg",
  garantiaAnios: 2,
  fechaPublicacion: "2026-10-15T17:45:00.000Z",
  disponible: true
};
```

#### Descripción de Campos:
- `idReferencia` *(String)*: Código alfanumérico visible de catálogo (ej. `"ART-101"`).
- `nombre` *(String)*: Título comercial descriptivo del objeto.
- `precioUSD` *(Number)*: Valor monetario en dólares para la venta al público.
- `categoria` *(String)*: Clasificación funcional (`"iluminacion"`, `"mobiliario"`, `"accesorios"`, `"estudio"`, `"decoracion"`).
- `materiales` *(String)*: Composición física y acabados empleados en la manufactura.
- `dimensiones` *(Object)*: Medidas físicas y masa de la pieza para cálculo de transporte y ajuste en espacios.
- `stockDisponible` *(Number)*: Cantidad de unidades físicas en inventario listas para despacho.
- `descripcion` *(String)*: Texto detallado sobre la inspiración, uso, acabados y particularidades del producto.
- `rutaImagen` *(String)*: Enlace relativo al archivo fotográfico del objeto.
- `garantiaAnios` *(Number)*: Plazo de cobertura técnica ofrecida por el taller.
- `fechaPublicacion` *(String)*: Fecha y hora en formato ISO en la que se dio de alta el producto.
- `disponible` *(Boolean)*: Indicador de visibilidad activa en el catálogo general.

---

## 🚀 Navegación y Flujo de Pantallas

1. **Entrada a la aplicación ([index.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/index.html)):** El usuario visualiza la pantalla de inicio de sesión.
2. **Acceso al Catálogo ([pages/catalogo.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/catalogo.html)):** Tras autenticarse o hacer clic en "Explorar Cosas", accede a la grilla con los 6 objetos en stock.
3. **Detalle de Pieza ([pages/detalle-producto.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/detalle-producto.html)):** El botón "Adquirir" de cualquier tarjeta conduce a la ficha técnica completa con selector de unidades.
4. **Publicación ([pages/agregar-cosa.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/agregar-cosa.html)):** Permite subir nuevas piezas con el esquema reflejado en el modelo de datos.
5. **Registro ([pages/registro.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/registro.html)):** Formulario de alta para nuevos compradores con redirección al inicio de sesión.
6. **Soporte y Atención ([pages/faq.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/faq.html), [pages/envios-devoluciones.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/envios-devoluciones.html), [pages/garantia.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/garantia.html), [pages/contacto.html](file:///c:/Users/Jaime%20Zapata/Documents/introduccion-dom/pages/contacto.html)):** Páginas informativas con canales directos y condiciones de compra.
