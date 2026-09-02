# Chequeo de citas y referencias

Herramientas para auditar la bibliografía de un trabajo académico, pensadas
en particular para detectar **referencias fabricadas por IA** y
**citas que no coinciden con la lista de referencias**.

Detectan:

- Citas en el cuerpo del texto sin entrada en la lista de referencias.
- Referencias en la lista que nunca se citan.
- Posibles años equivocados (el autor existe en la lista, pero con otro año).
- Citas secundarias ("citado en" / "as cited in") que en APA no deberían
  tener entrada propia en la lista.
- Referencias con DOI que no resuelve o título que no existe en
  CrossRef/OpenAlex — patrón típico de una referencia inventada por un LLM
  (requiere internet).

Hay cuatro entregas equivalentes en lógica, para distintos niveles de
comodidad técnica y necesidades de privacidad:

| Entrega | Requiere instalar | Requiere internet | Privacidad |
|---|---|---|---|
| **[Versión offline (HTML)](#versión-offline-recomendada-para-material-sensible)** | No | No (salvo para leer `.docx`) | 100% local, nada se sube a ningún lado |
| **[Notebook de Colab](#notebook-de-colab)** | No | Sí | El archivo se sube a Google — **no usar con trabajos inéditos** |
| **[Python](#python)** | Sí (`pip install`) | Solo para `verificar_refs` | Local |
| **[R](#r)** | Sí (paquetes de R) | Solo para `verificar_refs` | Local |

## Versión offline (recomendada para material sensible)

Corre 100% en el navegador, sin instalar nada y sin subir ningún archivo a
ningún servidor. Pensada para tesis o trabajos inéditos que no pueden salir
de tu computadora.

1. Descargá [`chequear_citas_offline.zip`](chequear_citas_offline.zip) y
   descomprimilo (quedan `chequear_citas_offline.html` y `.css`, deben estar
   juntos en la misma carpeta).
2. Hacé doble click en `chequear_citas_offline.html`.
3. Subí el cuerpo del texto y la lista de referencias, como archivo
   (`.docx` o `.txt`) o pegando el texto directamente.

Solo chequea citas vs. referencias (no verifica contra CrossRef/OpenAlex,
porque eso necesita internet real y una página estática no puede hacer esas
llamadas).

## Notebook de Colab

Abrí [`chequeo_referencias_y_citas.ipynb`](chequeo_referencias_y_citas.ipynb)
en [Google Colab](https://colab.research.google.com/) y `Entorno de
ejecución > Ejecutar todo`. Hace las dos cosas: chequeo de citas y
verificación contra CrossRef/OpenAlex. **El archivo que subís se sube a la
infraestructura de Google** — no usar con trabajos inéditos que no puedan
salir de tu computadora (para eso está la versión offline).

## Python

Instalar dependencias (una sola vez):

```bash
pip install -r requirements.txt
```

Uso:

```bash
python verificar_refs.py referencias.txt [reporte.csv]   # acepta .txt o .docx
python chequear_citas.py trabajo.docx                     # un solo archivo con encabezado "Referencias"
python chequear_citas.py cuerpo.txt referencias.txt        # cuerpo y lista en archivos separados
```

## R

Instalar dependencias (una sola vez):

```r
install.packages(c("httr2", "dplyr", "purrr", "tibble", "stringr", "stringi", "stringdist", "readr"))
```

Uso:

```bash
Rscript verificar_refs.R referencias.txt [reporte.csv]
Rscript chequear_citas.R trabajo.txt                    # un solo archivo con encabezado "Referencias"
Rscript chequear_citas.R cuerpo.txt referencias.txt      # cuerpo y lista en archivos separados
```

## Formato de entrada

Una referencia por línea o párrafo, o separadas por líneas en blanco (se
tolera numeración tipo "1." o "2)"). El modo de un solo archivo (Python y R)
espera un encabezado literal "Referencias" (o "Bibliografia"/"References"/
"Lista de referencias") en su propia línea, separando el cuerpo del texto
de la lista.

## Documentación técnica

Para el detalle de la arquitectura interna (patrones de matching, umbrales,
decisiones de diseño y bugs conocidos) ver [`CLAUDE.md`](CLAUDE.md).
