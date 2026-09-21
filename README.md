# Flores Amarillas

Experiencia web interactiva de una sola página para regalar flores amarillas, una carta personalizada y un mensaje secreto.

## Características

- Pantalla inicial para comenzar la experiencia.
- Música ambiental opcional desde `audio/girasol.mp3`.
- Jardín SVG interactivo con 30 girasoles y mensajes en distintos idiomas.
- Toast temporal para mostrar el idioma de cada flor.
- Carta especial con datos personalizables.
- Acceso protegido por contraseña al mensaje secreto.
- Cierre automático del mensaje secreto después de 10 segundos.
- El mensaje secreto conserva el salto de línea antes de la despedida final.
- Transición fluida hacia la celebración final con entrada escalonada del fondo, dedicatoria y carta.
- Celebración final con cielo, estrellas animadas durante toda la estancia y carta.
- Diseño responsive con prioridad en teléfonos, además de ajustes para tabletas, laptops, televisiones y orientación horizontal.
- Soporte para `prefers-reduced-motion`.
- Respuesta háptica opcional en móviles: vibración breve por flor, patrón especial al completar el jardín y pulso suave al terminar el mensaje secreto.
- Parámetros opcionales en la URL para personalizar el contenido sin editar el archivo.

## Estructura

```text
.
├── index.html
├── README.md
└── audio/
    └── girasol.mp3
```

`index.html` contiene el marcado, los estilos y la lógica JavaScript para que la experiencia pueda abrirse directamente como archivo local. El audio es opcional: si no existe, la página continúa funcionando y el control de música muestra el estado correspondiente.

## Ejecución

No se necesita instalar dependencias ni ejecutar un proceso de compilación.

1. Coloca `girasol.mp3` dentro de `audio/` si deseas música.
2. Abre `index.html` en un navegador moderno.

Para probarlo mediante un servidor local, desde esta carpeta puedes usar, por ejemplo:

```bash
python3 -m http.server 8000
```

Después visita `http://localhost:8000`.

El servidor local es recomendable para probar el audio y las APIs del navegador con un comportamiento más parecido a producción.

## Personalización

La configuración principal se encuentra en el objeto `PERSONALIZATION` dentro de `index.html`:

```js
const PERSONALIZATION = {
  name: "Chío",
  heroSubtitle: "Porque iluminas mis días mucho más de lo que crees.",
  letterBody: "Texto de la carta.",
  closing: "Con mucho cariño ❤️",
  secretMessage: "Mensaje que aparecerá en la escena secreta.",
  password: "flores",
};
```

Cambia estos valores para modificar el nombre, la carta, el cierre, el mensaje secreto y la contraseña.

### Personalización por URL

También se aceptan estos parámetros:

- `to`: nombre de la persona.
- `msg`: texto principal de la carta.
- `subtitle`: subtítulo de la portada.
- `closing`: despedida.
- `secret`: mensaje secreto.

Ejemplo:

```text
index.html?to=Ana&subtitle=Para%20ti&msg=Una%20carta%20especial&closing=Con%20cariño&secret=Siempre%20estaré%20contigo
```

Los valores de la URL se insertan con `textContent`; no se interpretan como HTML.

## Flujo de la experiencia

1. La persona pulsa `Comenzar`.
2. Aparece el primer girasol y se habilita la interacción.
3. Cada pulsación en `Presioname` agrega una flor al jardín.
4. Al llegar a 30 flores se muestra la cinta y luego la celebración final.
5. La carta permite solicitar la contraseña.
6. Una contraseña correcta abre el mensaje secreto.
7. El mensaje secreto se muestra durante 10 segundos y la experiencia vuelve automáticamente a la escena final.
8. `Volver` limpia el estado y permite empezar de nuevo.

## Organización del código

- **Estilos base y utility:** reset, clases utilitarias heredadas y estilos visuales.
- **Estilos de componentes:** jardín, carta, modales, toast y celebración.
- **Responsive:** reglas para móvil, táctil, tabletas, escritorio, pantallas grandes y orientación horizontal.
- **Personalización:** contenido editable y parámetros URL.
- **Temporizadores:** `scheduleSequence` gestiona las animaciones y pausa sus tiempos cuando la pestaña queda oculta.
- **Jardín SVG:** `createHeartSunflower` crea cada flor y `plantHeartSunflower` controla el progreso.
- **Celebración:** `showFinalCelebration` genera las estrellas y mueve la carta a la escena final.
- **Transición:** la escena final combina desplazamiento, opacidad y entradas escalonadas para evitar cambios bruscos entre vistas.
- **Háptica móvil:** `vibrate` usa la API opcional del navegador únicamente en dispositivos táctiles compatibles.
- **Limpieza:** `resetExperience` cancela timers, detiene el audio y devuelve la experiencia a su estado inicial.

## Consideraciones de accesibilidad

- Los controles principales son botones reales y tienen etiquetas `aria-label`.
- Las flores SVG se pueden activar con clic, `Enter` o barra espaciadora.
- Los diálogos usan roles y controlan el foco.
- Los mensajes temporales usan `role="status"`.
- Se respetan las preferencias de movimiento reducido del sistema.

## Mantenimiento

- Conserva el contenido personalizado dentro de `PERSONALIZATION`.
- Si cambias tiempos, revisa también los comentarios junto a `scheduleSequence` y `beginSecretReturn`.
- Si agregas elementos dinámicos, limpia sus timers y nodos en `resetExperience`.
- Mantén las posiciones de `heartPositions` dentro del `viewBox` del jardín.
- Prueba al menos `390x844` para móvil y `1440x900` para escritorio después de cambiar estilos responsive.
- Usa un servidor local cuando verifiques audio, foco, modales y comportamiento de navegación.
- Las estrellas deben seguir animándose mientras `finalCelebration` esté visible; solo se pausan cuando la pestaña queda oculta.
- La vibración depende de `navigator.vibrate`, por lo que algunos navegadores móviles pueden ignorarla sin afectar la experiencia.

## Estado de la auditoría

Se eliminaron estilos y animaciones de elementos que ya no existen, como los botones de cierre retirados y las estrellas decorativas del mensaje secreto. El resto de la hoja utility se conserva porque sus clases se usan en el marcado o se activan mediante estados dinámicos.
