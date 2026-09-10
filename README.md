# SAN — Sistema de AutoGestión de Notas (ETRR)

Herramienta web para docentes: revisa la coherencia entre la **planilla de Calificaciones**
y el **Mapa de Aprendizaje** antes del cierre de cuatrimestre.

Cada docente sube sus dos archivos `.xlsx` exportados del sistema. Cuando ambos tienen el
formato correcto, el botón **Procesar** cruza los datos y muestra un resumen con los errores
y advertencias encontrados. **Todo el procesamiento ocurre en el navegador** — los archivos
no se suben a ningún servidor y al recargar la página no queda nada guardado.

## Uso

Abrir la página publicada con GitHub Pages:

<https://ntetrr.github.io/SAN/>

1. Arrastrar (o elegir) el `.xlsx` de **Calificaciones** y el del **Mapa de Aprendizaje**.
2. Cada archivo se valida al soltarlo; si falta una columna se indica cuál.
3. Con las dos planillas en verde, hacer clic en **Procesar**.
4. Leer el resumen: cantidad de errores / advertencias y el detalle por estudiante.

## Reglas que se controlan (por estudiante)

| Situación | Resultado |
|---|---|
| Nota final ≥ 7 y RITE ≠ TEA | Error |
| Nota final < 7 y RITE = TEA | Error |
| RITE vacío o distinto de TEA / TEP / TED | Error |
| RITE = TEP o TED sin comentario en la columna `Ob` | Error a corregir (marcado más fuerte) |
| Nota final < 7 sin comentario en la columna `Ob` | Error |
| RITE = TEA con nota 10 y sin comentario | Advertencia (alumno sobresaliente) |
| Notas 7 – 9 sin comentario | No se controla |
| Más de una valoración `NM` o `EP` en el mapa con RITE = TEA | Error (debe ser TEP o TED) |
| Exactamente una `NM` o `EP` con RITE = TEA | Advertencia (permitido) |
| Estudiante sin fila en el Mapa, o con el Mapa sin valoraciones cargadas | Error (hay que completarlo) |
| Estudiante solo en el Mapa y no en Calificaciones | Advertencia |
| Las dos planillas parecen de asignaturas distintas | Aviso |
| El Mapa no tiene ninguna valoración cargada (para nadie) | Un solo error general en vez de uno por estudiante |

Las valoraciones del Mapa se leen de las columnas **Participación**, **Responsabilidad**
y **Evidencias** (escala `NM` / `EP` / `S` / `MS`). Se cuentan juntas las `NM` y las `EP`.

## Formato esperado de los archivos

**Calificaciones** — fila de encabezados con las columnas `Estudiante`, una columna que
contenga `RITE`, una que empiece con `Ob` (observación) y una que contenga `Final`.

**Mapa de Aprendizaje** — fila de encabezados con `Estudiante` y las columnas de valoración
(`Participac`, `Responsabi`, `Evidencias`, …) con valores `NM` / `EP` / `S` / `MS`. No debe
tener columna `RITE`.

Los nombres se cruzan entre planillas normalizando mayúsculas, espacios y acentos.

## Desarrollo

Es una única página estática (`index.html`) sin build. Usa
[SheetJS](https://sheetjs.com/) desde CDN para leer los `.xlsx`.
Para probar localmente, abrir `index.html` en el navegador.

> Las planillas reales de estudiantes están excluidas del repositorio (`.gitignore`)
> porque contienen datos personales.
