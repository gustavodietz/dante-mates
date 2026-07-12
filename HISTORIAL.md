# 📓 Historial del proyecto — La Galaxia de las Mates (juego para Dante)

Documento de contexto para retomar el proyecto en futuras sesiones.
Última actualización: **2026-07-12** (nuevo modo "Caza estelar" añadido; Firebase ya funcionaba).

---

## 🎯 Qué es
Juego web para que **Dante (8 años)** aprenda las **tablas de multiplicar (del 1 al 12)**, con
temática de **Star Wars**. Es un **único archivo** `index.html` (HTML + CSS + JS, sin dependencias
ni build), pensado para abrirse en cualquier navegador (ordenador, tablet, móvil).

- **Carpeta local:** `/Users/gustavodiez/Nirakara Dropbox/Gustavo Diez/CLAUDE/DANTE MATES/`
- **Repositorio GitHub:** https://github.com/gustavodietz/dante-mates (público, cuenta `gustavodietz`)
- **URL online (GitHub Pages):** https://gustavodietz.github.io/dante-mates/

---

## 🕹️ Cómo funciona el juego (estado actual)

### Pantalla de nombre / perfil
- Al entrar pide el **nombre**; Dante escribe "DANTE".
- El progreso se guarda con `localStorage` (clave `dante_galaxia_v3`), estructura
  `{ current, profiles: { NOMBRE: { tables: { N: { bestLevel, bestStars, levels:{...} } } } } }`.
- Botón **"CAMBIAR JUGADOR"** en el menú. Migra datos antiguos de `dante_galaxia_v2` la 1ª vez.

### Menú
- Rejilla de tablas 1–12; cada casilla muestra **en qué nivel está Dante** ("Nv 3"), sus
  **estrellas** y marca en verde "✓ MAX" si completó la tabla.

### 📚 Aprender — La cantina (fase de aprendizaje)
- Ambientada en una **cantina de Star Wars** con un **camarero alienígena** (SVG).
- Dante elige un **plato/bebida** de la saga (leche azul de bantha, asado de nerf, cerveza
  correlliana, porg asado, zumo jawa/jogan, guiso de ronto, pastelito de Endor).
- Cada plato cuesta lo que marca la tabla (tabla del 9 → 9 créditos). Elige cuántos quiere.
- **Visualización elegida:** monedas como **círculos dorados numerados** (1, 2, 3… hasta el total),
  agrupadas por plato, y **total acumulado por fila** (9, 18, 27…) para enseñar a "contar de 9 en 9".
- El camarero lo explica por texto.

### 🚀 Misión — estilo Space Invaders
- Dante pilota un **X-wing** (SVG) y debe **destruir la flota imperial** (naves TIE, SVG).
- **20 preguntas**, dos formatos: `9 × 5 = ?` (→45) y `9 × ? = 45` (→5).
- Nº de naves enemigas = aciertos necesarios; **vidas = 20 − aciertos necesarios**.
- **Acierta** → aparece botón para **lanzar un misil** que destruye una nave.
- **Falla** → una nave le **dispara** (pierde una vida) y aparece el **maestro Yoda** (SVG) con la
  solución (`Padawan Dante, 9 × 5 son 45...`). Dante **pulsa para seguir** (botón o cualquier tecla),
  así lee con calma; ya no avanza solo.
- Gana al destruir toda la flota; pierde si se queda sin vidas ("tocado" en la última).
- **5 niveles** que se **desbloquean de uno en uno por cada tabla** (solo el 1 al empezar):
  | Nivel | Nombre | Naves (aciertos) | Vidas | Entrada |
  |---|---|---|---|---|
  | 1 | Cadete | 10 | 10 | opciones |
  | 2 | Piloto | 12 | 8 | opciones |
  | 3 | Caballero Jedi | 15 | 5 | **escribir** (teclado en pantalla) |
  | 4 | Maestro Jedi | 17 | 3 | escribir |
  | 5 | Gran Maestro | 19 | 1 | escribir |
- Al ganar: aviso **"🔓 Nivel X desbloqueado"** o **"🏆 Tabla dominada"**. Estrellas según fallos.

### Estética / técnico
- Look **arcade**: fuente pixel (Press Start 2P), neón, scanlines CRT, campo de estrellas animado
  (canvas), naves y personajes dibujados en **SVG** (X-wing, TIE, Yoda, camarero alien).
- **Sin voz** (se probó Web Speech API pero no sonaba a Yoda; se quitó a petición de Gustavo).
- Efectos de sonido con **Web Audio API** (sin archivos): toques, aciertos, fallos, misiles,
  explosiones, fanfarrias.

---

## ✅ Peticiones ya resueltas (en orden)
1. Juego base: fase aprender + misión con persecución, 5 niveles con umbrales 10/12/15/17/19.
2. Cambiada la misión a **Space Invaders** (flota + misiles + vidas) en vez de persecución.
3. **Yoda** aparece al fallar con la solución.
4. Fase de aprendizaje convertida en **cantina** con camarero y **monedas**.
5. Quitada la **voz**; añadido **"pulsar para seguir"** tras fallar.
6. **Perfiles con nombre**, **desbloqueo de niveles por tabla**, nivel visible por número en el menú,
   monedas **numeradas + total por plato**.
7. Publicado en **GitHub Pages** (repo + Pages activo, HTTP 200 verificado).
8. **Sincronización en la nube con Firebase** (historial por nombre entre dispositivos). ✅
9. **Tercer modo de juego: "Caza estelar"** (cabina + mirilla). ✅

---

## 🎯 Caza estelar — tercer modo (HECHO)
Nuevo modo **seleccionable desde el menú** (tarjeta 🎯 `#goHunter`), independiente de la tabla elegida.
Todo dentro del mismo `index.html`, reutilizando sonido, `show()`, progreso/nube, `$`/`rnd`/`shuffle`, `TOTAL_Q`.

- **Preguntas:** mezcla **todas las tablas 1–12**; alterna *resultado* (`4 × 7 = ?`) y *factor desconocido*
  (`6 × ? = 24`, `? × 8 = 56`), con respuesta única. La proporción de incógnitas sube por nivel.
- **Estructura:** **5 niveles**, **10 vidas**, **20 preguntas** máx. Objetivos: **5 / 7 / 9 / 12 / 15** naves.
  Dificultad creciente: más naves simultáneas, más pequeñas, más rápidas y trayectorias menos predecibles.
- **Dos fases por turno:** (1) **resolver** con teclado en pantalla; si falla → error + sonido, −1 vida,
  muestra la solución y pasa de pregunta (sin disparo). Si acierta → (2) **apuntar y disparar** una sola vez.
- **Mirilla:** círculo exterior + retícula interior, movimiento suave, cambia a rojo sobre una nave.
  Controles: **flechas/WASD** + **espacio/Enter** para disparar, y **D-pad táctil** (▲◀🔥▶▼). Bloqueado el
  doble disparo y la repetición de tecla; se evita el scroll de la página. Colisión por **centro/radio**
  (no rectángulos permisivos).
- **Disparo:** láser desde el cañón; impacto → explosión + sonido + naves destruidas + respawn; fallo →
  el láser sigue al fondo y cuenta como fallado. Ambas cosas pasan de pregunta.
- **Cabina** de estética espacial: metal envejecido, paneles, luces de navegación, ventana frontal,
  espacio y naves al fondo, y un **acompañante estilo Grogu** (SVG dibujado a mano, fan art para uso
  personal/no comercial de Dante y Darío) con reacciones (alegría, celebración, susto, decepción,
  neutro, sueño si tarda). Dice la solución al fallar en un bocadillo (como Yoda en el modo clásico).
- **Cuadro de mandos** integra: operación, respuesta, nivel, vidas, nº de pregunta, aciertos, errores,
  naves destruidas, barra de progreso y estado (respondiendo/apuntando/acierto/fallo/victoria/derrota).
- **Resumen** (`scHResult`): nivel, preguntas, aciertos, errores, % acierto, naves, disparos fallados,
  vidas y **tiempo**. Botones **Repetir / Siguiente / Menú** (Siguiente oculto al ganar el nivel 5).
- **Progreso propio** bajo `perfil.hunter.levels` (no toca `tables`), sincronizado en la nube
  (`mergeProfiles` extendido). Desbloqueo de niveles progresivo, igual que el modo clásico.
- **Limpieza:** `hunterCleanup()` cancela `requestAnimationFrame`, timers y listeners al salir/reiniciar
  (enganchado en `show()`).
- **Pantallas nuevas:** `scHLevel` (selección), `scHunter` (juego), `scHResult` (resumen).
- **Pruebas:** arnés headless con jsdom (fuera del repo) — 50 asserts del modo + 14 de regresión de los
  modos clásicos, todos en verde; `node --check` OK; sin errores de consola.

---

## ☁️ Firebase — sincronización entre dispositivos (HECHO)
- **Proyecto Firebase:** `dante-mates` (cuenta Google de Gustavo). **Realtime Database** en
  **europe-west1**. URL: `https://dante-mates-default-rtdb.europe-west1.firebasedatabase.app`.
- **Reglas publicadas** (abiertas pero acotadas a `/profiles`):
  ```json
  { "rules": { "profiles": { ".read": true, ".write": true } } }
  ```
- **Integración en `index.html`:**
  - `<script type="module">` que carga Firebase por CDN (v10.12.5) e inicializa; expone
    `window.cloudGet(name)` y `window.cloudSet(name,data)`. La `firebaseConfig` está en el HTML
    (es pública por diseño, no es secreta).
  - Al **entrar con el nombre** (`doEnter` → `cloudSync`): descarga de la nube, **fusiona** con lo
    local (`mergeProfiles`, máximo de bestLevel/bestStars/passed por tabla y nivel), guarda local y
    vuelve a subir. Muestra "☁ SINCRONIZANDO…".
  - En cada `recordResult` y en el borrado → `cloudPush()` sube el progreso.
  - **Offline-safe:** si no hay red, `cloudGet` devuelve `undefined` y se usa solo lo local; sincroniza
    al volver la conexión / en el siguiente arranque.
  - Datos en la nube bajo `profiles/<nombre_saneado>` (minúsculas, sin caracteres inválidos de Firebase).
- **Opcional futuro:** **PIN de 4 dígitos** por nombre para proteger lectura/escritura.

---

## ⏳ Pendiente / en curso

### 1. Bug de audio (APARCADO por Gustavo)
- En el juego el `AudioContext` daba estado **`running`** pero **no se oía**, aunque un archivo de
  prueba aislado (`test-sonido.html`, ya borrado) **sí sonaba** con el mismo código.
- Ya aplicado: desbloqueo robusto (buffer silencioso + `resume()` en el primer gesto) y `try/catch`.
- Se quitó todo el instrumental de diagnóstico antes de publicar.
- **Siguiente diagnóstico pendiente:** el usuario iba a decir si el "tono de prueba" del juego se oía
  y los datos `sampleRate/channelCount`. Quedó a medias; retomar si vuelve a molestar.

### 2. Ideas futuras mencionadas
- Más **fases de mates** (sumas, restas, divisiones) como nuevos mundos.
- Ajustes finos: ritmo de disparo/explosión, tamaño de naves, resaltar más la secuencia 9-18-27.

---

## 🛠️ Notas de trabajo
- Herramientas: `git` y `gh` instalados; autenticado como **gustavodietz** (scopes repo, workflow, gist).
- Identidad git del repo: `gustavodietz` / `gustavodietz@users.noreply.github.com`.
- Validar siempre el JS extrayendo el `<script>` y `node --check` antes de dar por bueno un cambio.
- Para probar en local: `open index.html` (abre en el navegador por defecto del Mac).
- Para publicar cambios: editar `index.html` → `git add` → `git commit` → `git push` (Pages se
  actualiza en 1–2 min). El repo contiene `index.html`, `README.md`, `HISTORIAL.md`, `.gitignore`.
