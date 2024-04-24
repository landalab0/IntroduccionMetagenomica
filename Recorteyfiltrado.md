#

Si alguna de nuestras muestras no cumple con las métricas de calidad utilizadas por FASTQC, no significa que debemos desecharlas. En nuestro flujo de trabajo, eliminaremos algunas secuencias de baja calidad para reducir nuestra tasa de falsos positivos debido a errores de secuenciación.

Utilizaremos el programa **Trimmomatic**, que sirve para filtrar las lecturas de mala calidad y recorta bases de mala calidad de las muestras especificadas.

 **`Code`** 

 $ trimmomatic

  **`Output`** 

Usage: 
       PE [-version] [-threads <threads>] [-phred33|-phred64] [-trimlog <trimLogFile>] [-summary <statsSummaryFile>] [-quiet] [-validatePairs] [-basein <inputBase> | <inputFile1> <inputFile2>] [-baseout <outputBase> | <outputFile1P> <outputFile1U> <outputFile2P> <outputFile2U>] <trimmer1>...
   or: 
       SE [-version] [-threads <threads>] [-phred33|-phred64] [-trimlog <trimLogFile>] [-summary <statsSummaryFile>] [-quiet] <inputFile> <outputFile> <trimmer1>...
   or: 
       -version

El output nos indica que debemos especificar si nuestras lecturas son de extremo emparejado (PE) o de extremo único (SE).
