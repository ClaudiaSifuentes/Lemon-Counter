
# 🍋 Lemon Counter

**Detección y conteo automático de limones** en video mediante visión por computadora y modelos YOLOv8 con seguimiento (tracking) integrado.

Este proyecto implementa un pipeline reproducible en Python que **detecta, sigue y cuenta** limones que pasan por una banda transportadora, generando:
- Un **video de salida** con las detecciones visuales (cajas, IDs, centroides y contador).
- **Registros CSV** con el conteo por frame y cada evento de cruce.

---

## 📂 Estructura del repositorio

| Carpeta / Archivo | Descripción |
|--------------------|--------------|
| `lemon_counter.ipynb` | Notebook principal con todo el flujo de detección, tracking y conteo. |
| `data/` | Carpeta donde colocar el video de entrada (ej. `data/lemons_video.mp4`). |
| `outputs/` | Carpeta de salida con el video procesado y los archivos CSV. |
| `requirements.txt` | Dependencias necesarias para ejecutar el proyecto. |

---

## 🧠 Descripción general

**Lemon Counter** utiliza **YOLOv8** para detectar limones y su tracker integrado para mantener un **ID persistente por objeto**.  
El conteo se realiza cuando el **centro del limón cruza una línea virtual**, calculando el cambio de lado (signo) respecto a dicha línea usando el **determinante 2D**.  

Para evitar errores o dobles conteos, se aplican controles como:
- **Edad mínima del track (`MIN_TRACK_AGE`)**  
- **Enfriamiento (`COOLDOWN_FRAMES`)**  
- **Umbral de ruido (`EPS_SIDE`)**  

### Salidas generadas:
- 🎥 `outputs/output_video.mp4` — video procesado con detecciones e indicador de conteo.  
- 📊 `outputs/results.csv` — conteo por frame.  
- 🧾 `outputs/crosses.csv` — log detallado de cada evento de cruce.  

---

## ⚙️ Requisitos e instalación

**Versión mínima de Python:** 3.8+  
Se recomienda usar un entorno virtual:

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .\.venv\Scripts\activate
pip install -r requirements.txt
````

Dependencias principales:

```
ultralytics
opencv-python
pandas
tqdm
```

Si no tienes el archivo `requirements.txt`:

```bash
pip install ultralytics opencv-python pandas tqdm
```

---

## 🧩 Uso rápido

1. Copia tu video a `data/lemons_video.mp4`.
2. Descarga el modelo si no está localmente (`yolov8n.pt`).
3. Abre el notebook en Jupyter o VS Code y ejecútalo paso a paso.

También puedes ejecutar el notebook completo por terminal:

```bash
jupyter nbconvert --to notebook --execute lemon_counter.ipynb --output lemon_counter_executed.ipynb
```

**Resultados esperados en `/outputs`:**

* `output_video.mp4` → video con overlays
* `results.csv` → conteo por frame
* `crosses.csv` → cada cruce detectado

---

## 🧮 Parámetros ajustables

| Parámetro            | Descripción                            | Valor sugerido                   |
| -------------------- | -------------------------------------- | -------------------------------- |
| `LINE_P1`, `LINE_P2` | Extremos de la línea virtual de conteo | Ajustar según el flujo del video |
| `CONFIDENCE`         | Confianza mínima de detección YOLO     | 0.35–0.5                         |
| `IOU`                | Umbral de intersección (NMS)           | 0.5                              |
| `EPS_SIDE`           | Umbral de ruido (para jitter)          | 3–10                             |
| `MIN_TRACK_AGE`      | Frames mínimos para validar track      | 2–5                              |
| `COOLDOWN_FRAMES`    | Frames entre conteos del mismo ID      | 10–20                            |

---

## ✅ Validación de resultados

* Abre `outputs/crosses.csv` para inspeccionar los cruces detectados.
* Puedes verificar manualmente en el video que cada cruce corresponde a un limón real.
* Si tienes etiquetas “ground truth”, puedes calcular métricas como **precisión**, **recuperación** y **F1-score** comparando con el CSV.

---

## 🧑‍💻 Licencia

Licencia **MIT** — uso y modificación libre con atribución.
Puedes agregar un archivo `LICENSE` con el texto correspondiente.

