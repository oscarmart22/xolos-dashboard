# Dashboard Xolos · Rendimiento físico y factores de riesgo de lesión

Prototipo interactivo del dashboard descrito en el **Capítulo III (META 3.2 · Metodología)** del proyecto
*Propuesta de dashboard para la visualización del rendimiento físico y el monitoreo de factores de riesgo de lesión en el Club Tijuana (Xolos)*.

> **Datos simulados.** Todas las métricas de carga y movimiento son generadas por un script y **no corresponden a mediciones reales**
> de ningún jugador. Lo que se demuestra es la lógica del tablero (cómo se calculan los indicadores, cómo se filtran y cómo se leen),
> no el estado físico de la plantilla. El dashboard **describe, no predice**: no calcula la probabilidad de lesión.

![Resumen](docs/resumen.png)

## Cómo usarlo

Es un solo archivo HTML sin dependencias de compilación.

- **Local:** abre `index.html` en el navegador.
- **GitHub Pages:** *Settings → Pages → Deploy from a branch → `main` / root*. Quedará en `https://<usuario>.github.io/<repositorio>/`.
- Solo carga la tipografía desde Google Fonts; sin conexión usa tipografías del sistema.

## Qué incluye

| Pestaña | Contenido |
|---|---|
| **Resumen** | Estado del plantel, ACWR por jugador y jugadores en observación. |
| **Tablero** | Los 10 KPI (K1–K10) en tres bloques (carga, intensidad, movimiento), 4 gráficos del documento, 3 segmentadores (jugador, semana/sesión, tipo de entrenamiento) y umbral de asimetría editable. |
| **Vista de jugador** | Carga y ACWR, asimetrías, histograma de Vmax, zonas de calor, radar y comparación contra su posición y su línea base de 28 días. |
| **Comparar** | Dos jugadores lado a lado (tabla, ACWR y asimetría). |
| **Equipo** | Tabla del plantel ordenada por estado (rojo, ámbar, verde). |
| **Tabla** | Registros por jugador y sesión; copia como CSV. |
| **Datos** | Carga de CSV, validaciones, bitácora de limpieza, diccionario de 17 variables. |
| **Metodología** | Resumen del documento: alcance, KPI, definiciones y equivalencias con las hojas del Excel. |

Además: modo claro/oscuro, animaciones que respetan `prefers-reduced-motion`, ayuda «i» por gráfico y por pantalla, navegación por teclado en los gráficos y diseño adaptable a móvil.

## Concordancia con el documento

- **10 KPI** con las fórmulas de la sección 3.4 (K1 promedio de distancia por jugador y sesión, K6 Σdistancia ÷ Σduración, K7 variación semanal, K9 jugadores sobre el umbral, etc.).
- **Diccionario de 17 variables** con los nombres, rangos y tratamiento de faltantes de la sección 3.2.
- **Limpieza y validación:** duplicados (se conserva el primero), fechas homologadas a `AAAA-MM-DD`, faltantes **no imputados** (se muestran como «sin dato» y se excluyen solo de los KPI que usan esa variable), fuera de rango corregido o marcado, atípicos plausibles conservados, asimetría recalculada `100 × |izq − der| ÷ promedio`, coherencia interna y recálculo independiente de totales.
- **Errores controlados** (2 duplicados, fechas mal escritas, faltantes y un valor fuera de rango) para comprobar que la limpieza los detecta. Se pueden desactivar en la pestaña *Datos*.
- **Periodo:** 4 semanas × 4 sesiones = 16 sesiones; **18 jugadores** → 288 registros.

### Diferencias respecto al documento

| Documento | Este prototipo | Motivo |
|---|---|---|
| 20 jugadores (J01–J20), 320 registros | 18 jugadores, 288 registros | El plantel mostrado corresponde a los 18 jugadores del primer equipo con ficha pública. |
| Jugadores identificados solo con códigos | Nombres y fotos del primer equipo | Mejor lectura del prototipo; las métricas siguen siendo simuladas. |
| Herramienta: Excel (tablas dinámicas y segmentadores) | HTML/JavaScript | Versión web interactiva del mismo diseño. |
| Paleta azul/naranja del boceto | Paleta sobria rojo/negro | Identidad del club; se mantiene un color por categoría. |

## Usar datos propios

En *Datos → Cargar CSV* se acepta un archivo con las columnas del diccionario (coma o punto y coma como separador).
Los archivos de `data/` sirven de ejemplo:

| Archivo | Contenido |
|---|---|
| `data/plantilla.csv` | Encabezado y una fila de ejemplo. |
| `data/demo_limpio.csv` | 288 registros sin errores. |
| `data/demo_con_errores_controlados.csv` | Los mismos datos con errores inyectados para ver la bitácora. |

Los datos se procesan en el navegador; no se envían a ningún servidor. Con un CSV, el periodo termina en la última fecha cargada y el ACWR necesita al menos 28 días previos para ser confiable (la pestaña *Datos* lo advierte).

## Estructura del repositorio

```
index.html        Dashboard completo (HTML, CSS y JavaScript en un archivo)
data/             CSV de ejemplo y plantilla
docs/             Capturas de pantalla
```

## Autoría y avisos

Proyecto académico de Zilli Casas Alonzo, Oscar Isaac Martinez Garcia y Ruth Aide Delgado Sanchez (grupo 981).

- Los nombres, dorsales, posiciones y fechas de nacimiento provienen de la página pública del primer equipo (xolos.com.mx); las fotos y el escudo pertenecen al Club Tijuana. Se usan con fines académicos y sin relación con el club. Si el repositorio será público, considera retirar las imágenes.
- No se incluye licencia: elige la que corresponda antes de publicarlo.

## Nota de verificación

Los KPI K1–K10, con y sin filtros, se verificaron contra un recálculo independiente a partir de los CSV de `data/`. Se eliminó un ajuste artificial que forzaba valores fijos de ACWR en la vista de demostración: ahora el ACWR (carga media de 7 días ÷ carga media de 28 días, con la duración de la sesión en minutos como carga) se calcula únicamente con los datos. Las capturas de `docs/` corresponden a esta versión.
