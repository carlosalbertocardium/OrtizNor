# ORTIZNOR

Sistema experto para la generación automática de informes de ecocardiografía transtorácica, a partir de una entrada mínima de datos codificados.

Desarrollado por el **Dr. Carlos Alberto Ortiz** — Internista Cardiólogo (RM. 19497274).

## ¿Qué es esto?

ORTIZNOR transforma una hoja manuscrita con formato fijo (cama/nombre, condiciones, medidas y códigos abreviados) en un informe ecocardiográfico completo — descripción anatómica + conclusiones diagnósticas — siguiendo reglas clínicas fijas, sin improvisación.

El motor es **código determinístico** (JavaScript puro): las mismas reglas se aplican siempre igual, de forma reproducible y auditable. No es una IA generando texto libremente — la IA solo se usa, opcionalmente, para el paso de lectura de la foto (ver `motor-foto.html`).

## Archivos de este repositorio

| Archivo | Qué es | Cuándo usarlo |
|---|---|---|
| **`index.html`** | Motor de reglas — vista de columnas lado a lado (una por paciente) | Uso diario general; ideal cuando quieres ver/editar varios pacientes a la vez, igual que la hoja física |
| **`texto-continuo.html`** | Motor de reglas — cuadro de texto único corrido | Dictado rápido en el celular, escribiendo todo de corrido sin cambiar de columna |
| **`motor-foto.html`** | Igual que `index.html`, más un botón para subir foto de la hoja e interpretarla con visión | Solo funciona dentro del entorno de Claude (requiere llamada a la API); fuera de ahí, usar `index.html` |
| **`ORTIZNOR_Documento_Maestro.docx`** | Especificación técnica completa: filosofía del sistema, diccionario de códigos, reglas clínicas, algoritmos (diástole, hipertensión pulmonar), plantillas de reporte | Referencia para entender el *porqué* de cada regla |
| **`ORTIZNOR_Tabla_Maestra.xlsx`** | Diccionario de códigos en formato tabla, con reglas de redacción, reglas clínicas y validaciones | Consulta rápida de códigos, o como fuente para reprogramar el motor en otro lenguaje |

## Las tres herramientas HTML son 100% offline

No requieren internet, cuenta, ni servidor — son archivos autocontenidos que corren enteramente en el navegador. La única excepción es el botón de foto en `motor-foto.html`, que sí necesita conexión y el entorno de Claude.

## Instalación como app en iPhone

1. Abre el link de GitHub Pages de este repositorio en Safari.
2. Toca **Compartir** → **"Añadir a pantalla de inicio"**.
3. Queda como ícono fijo, cargando siempre la última versión subida a este repositorio.

## Formato de entrada (sin etiquetas necesarias)

Cada paciente se escribe en este orden fijo, un dato por línea:

```
317              ← cama (o "AMB" para ambulatorio)
Sergio Hernández ← nombre
81               ← frecuencia cardiaca (o "81/min")
170cm            ← talla (identificada por "cm")
70kg             ← peso (identificada por "kg", talla y peso en cualquier orden entre sí)
IOT              ← condición del paciente, si aplica (opcional)
31               ← a partir de aquí, las 7 medidas fijas (mm, se convierten a cm)
28
14
8
26
46
8
65               ← FEVI (o "65%", o rango "66-70")
AI17             ← resto de códigos, uno por línea
CV13
em
S-30
```

Convención de tachado: `~~código~~` anula un dato; si hay un reemplazo al lado, se usa ese.

## Estado del proyecto

Motor validado contra casos reales del Dr. Ortiz. Diccionario en expansión continua — cualquier código no reconocido se reseña al final del informe del paciente correspondiente, para revisión manual, en vez de interpretarse a ciegas.
