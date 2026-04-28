# RCQ 2026 — Dashboard BloodMoon

Dashboard estático para visualizar resultados del torneo RCQ 2025 organizado por BloodMoon Games. Construido con Vue 3 (CDN), Vue Router 4, Bootstrap 5 y CSS custom properties. Sin build step — corre directo en el browser.

---

## Características

- **Standing** — Tabla de posiciones con badge de clasificación (1st, 2nd, Top4, Top8), enlace a decklist y estadísticas por jugador.
- **Metagame** — Cards por arquetipo con imagen de carta clave como fondo, win rate, presencia real en el meta (base: total de jugadores del torneo) y barra visual de presencia.
- **Social** — Entrevista al campeón con foto, blockquote y links a redes sociales. Brackets y foto del Top 8 con modal de imagen a tamaño completo.

---

## Tecnologías

| Tecnología | Versión | Uso |
|---|---|---|
| Vue 3 | CDN unpkg | Reactividad, componentes, router |
| Vue Router 4 | CDN unpkg | SPA con hash history |
| Bootstrap 5 | 5.3.8 CDN | Grid, cards, modales, utilidades |
| Bootstrap Icons | 1.11.3 CDN | Iconografía navbar y redes sociales |
| JSON local | — | Fuente de datos del torneo |

---

## Estructura del proyecto

```
dashboard-bloodmoon/
├── index.html
└── assets/
    ├── StandingRCQ25_4_26.json   ← datos del torneo
    ├── Brackets.png
    ├── Top8.png
    ├── HLobosPortrait.png
    └── *.jpg                      ← imágenes de key cards por arquetipo
```

---

## Estructura del JSON

Cada jugador en `StandingRCQ25_4_26.json` sigue este esquema:

```json
{
  "Place": 1,
  "Nombre": "Jugador Ejemplo",
  "Puntos": 15,
  "V": 5,
  "D": 0,
  "E": 0,
  "%VPO": "60.0%",
  "%JG": "100.0%",
  "%JGO": "55.2%",
  "Decklist": "Nombre del Mazo",
  "keyCard": "Carta Principal",
  "imgRoute": "./assets/carta.jpg",
  "decklistUrl": "https://manabox.app/decks/...",
  "Badge": "1st"
}
```

### Valores válidos para `Badge`

| Valor | Pill | Color |
|---|---|---|
| `1st` | 1° Lugar | Dorado `#FFD700` |
| `2nd` | 2° Lugar | Plateado `#C0C0C0` |
| `Top4` | Top 4 | Bronce `#CD7F32` |
| `Top8` | Top 8 | Verde `#198754` |

---

## Vistas

### Standing (`/standing`)

- Tabla con posición, nombre, badge de clasificación y decklist linkeable.
- Badge renderizado como `span.badge.rounded-pill` con clase dinámica via `badgeClass(badge)`.
- Estadísticas: Puntos, V/D/E, %VPO, %JG, %JGO.

### Metagame (`/metagame`)

- Grid Bootstrap 3 columnas (`col-12 col-md-6 col-lg-4`).
- Cards agrupadas por arquetipo, calculadas desde el JSON con `computed`.
- **Presencia real**: `(copias del mazo / total jugadores) × 100`. Con 27 jugadores, 4 copias = 14.81%.
- Imagen de `imgRoute` como background con degradado inferior (`::after`) para fusionar con la card.
- Nombre del arquetipo linkeable a `decklistUrl`.

### Social (`/social`)

- Links a Instagram con `.social-link` y hover en `var(--accent)`.
- Card entrevista: layout `row g-0` con foto a la izquierda y blockquote con borde accent.
- Imágenes de Brackets y Top 8 con modal Bootstrap (`modal-xl`) al hacer click.

---

## Cómo usar

1. Clona o descarga el repositorio.
2. Coloca las imágenes de key cards en `assets/` con los nombres referenciados en `imgRoute`.
3. Abre `index.html` desde un servidor local (no funciona con `file://` por el fetch al JSON).

```bash
# Con Python
python -m http.server 8080

# Con Node
npx serve .
```

4. Accede a `http://localhost:8080`.

---

## Notas

- El proyecto **no usa build tools** (sin Vite, sin npm). Todo corre por CDN.
- El archivo JSON se genera externamente vía Google Apps Script desde MTG Elo / Melee.gg.
- No coloques el repositorio dentro de carpetas sincronizadas con Google Drive (riesgo de corrupción con `desktop.ini`).

---

*BloodMoon Games — Quilpué, Valparaíso, Chile*
