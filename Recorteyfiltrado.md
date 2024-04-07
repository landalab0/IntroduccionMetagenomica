#

Si alguna de nuestras muestras no cumple con las métricas de calidad utilizadas por FASTQC, no significa que debemos desecharlas. En nuestro flujo de trabajo, eliminaremos algunas secuencias de baja calidad para reducir nuestra tasa de falsos positivos debido a errores de secuenciación.

Utilizaremos el programa **Trimmomatic**, que sirve para filtrar las lecturas de mala calidad y recorta bases de mala calidad de las muestras especificadas.

