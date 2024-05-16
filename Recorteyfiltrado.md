![image](https://github.com/landalab0/IntroduccionMetagenomica/assets/160524982/78bf90eb-2c12-46f8-a327-2174a2d20381)# Recorte y Filtrado✂️

Si alguna de nuestras muestras no cumple con las métricas de calidad utilizadas por FASTQC, no significa que debemos desecharlas. En nuestro flujo de trabajo, eliminaremos algunas secuencias de baja calidad para reducir nuestra tasa de falsos positivos debido a errores de secuenciación.

Utilizaremos el programa **Trimmomatic**, que sirve para filtrar las lecturas de mala calidad y recorta bases de mala calidad de las muestras especificadas, así como para deshacerse de adaptadores.

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

En el modo de extremo emparejado, Trimmomatic espera los dos archivos de entrada y luego los nombres de los archivos de salida: 

| Opción                  |                Significado                             |
| ------------------------| ------------------------------------------------------- |
| <archivo de entrada 1>  	| Ingrese lecturas hacia adelante para recortar. Normalmente, el nombre del archivo contendrá una _1o _R1en el nombre. |
| <archivo de entrada 2> |	Ingrese lecturas inversas para recortar. Normalmente, el nombre del archivo contendrá una _2o _R2en el nombre.|
| < archivo de salida1P> |	Archivo de salida que contiene los pares supervivientes del _1archivo. |
| < archivo de salida1U> |	Archivo de salida que contiene lecturas huérfanas del _1archivo.| 
| < archivo de salida2P> |	Archivo de salida que contiene los pares supervivientes del _2archivo.|
| < archivo de salida2U>	| Archivo de salida que contiene lecturas huérfanas del _2archivo. |


Mientras está en el modo de extremo único, Trimmomatic esperará un archivo como entrada, después de lo cual podrá ingresar las configuraciones opcionales y, por último, el nombre del archivo de salida:

| Paso |	Significado |
|-------|-------------- |
| ILLUMINACLIP	| Realice la extracción del adaptador|
| SLIDINGWINDOW	|Realice un recorte de ventanas corredizas, cortando una vez que la calidad promedio dentro de la ventana caiga por debajo de un umbral. |
| LEADING	| Corte las bases del inicio de una lectura si la calidad está por debajo de un umbral |
| TRAILING |	Corte las bases del final de una lectura si la calidad está por debajo de un umbral |
| CROP |	Corta la lectura a una longitud especificada | 
| HEADCROP |	Corta el número especificado de bases desde el inicio de la lectura |
| MINLEN |	Descarta una lectura completa si está por debajo de una longitud especificada |
| TOPHRED33 |	Convierta puntajes de calidad a Phred-33 |
| TOPHRED64 |	Convierta puntajes de calidad a Phred-64 |

Usaremos algunas opciones y pasos de recorte para nuestro análisis. 
Un ejemplo de un comando para Trimmomatic ( no nos funcionará porque no tenemos todos los archivos a los que hace referencia ): 

 **`Code`** 

 $ trimmomatic PE -threads 4 SRR_1056_1.fastq SRR_1056_2.fastq \
              SRR_1056_1.trimmed.fastq SRR_1056_1un.trimmed.fastq \
              SRR_1056_2.trimmed.fastq SRR_1056_2un.trimmed.fastq \
              ILLUMINACLIP:SRR_adapters.fa SLIDINGWINDOW:4:20

  Pero ese comando qué ¿? 🥱

  Le hemos dicho a Trimmomatic:

| Código	 | Significado |
|---------|------------- |
| PE	| que tomará un archivo de extremo emparejado como entrada |
| -threads 4 |	utilizar cuatro subprocesos informáticos para ejecutar (esto acelerará nuestra ejecución) |
| SRR_1056_1.fastq	| el primer nombre del archivo de entrada. Adelante |
| SRR_1056_2.fastq	| el segundo nombre del archivo de entrada. Contrarrestar |
| SRR_1056_1.trimmed.fastq |	el archivo de salida para los pares supervivientes del _1archivo |
| SRR_1056_1un.trimmed.fastq |	el archivo de salida para lecturas huérfanas del _1archivo |
| SRR_1056_2.trimmed.fastq |	el archivo de salida para los pares supervivientes del _2archivo |
| SRR_1056_2un.trimmed.fastq |	el archivo de salida para lecturas huérfanas del _2archivo |
| ILLUMINACLIP:SRR_adapters.fa |	para recortar los adaptadores de Illumina del archivo de entrada utilizando las secuencias de adaptadores enumeradas enSRR_adapters.fa |
| SLIDINGWINDOW:4:20 |	utilizar una ventana deslizante de tamaño 4 que eliminará las bases si su puntuación Phred es inferior a 20 |


 Ha llegado el momento de ejecutar Trimmomatic pero con nuestros datos. Hay que posicionarse en el directorio de datos. 

  **`Code`** 

$ cd ~/dc_workshop/data/untrimmed_fastq
$ pwd

  
   **`Output`** 
   
$ /home/dcuser/dc_workshop/data/untrimmed_fastq

Deberíamos de tener cuatros archivos en este directorio, correspondientes a los archivos de lecturas directas e inversas de los ejemplos JC1A y JP4D.


**`Code`**

$ ls

**`Output`**

$ JC1A_R1.fastq.gz  JC1A_R2.fastq.gz  JP4D_R1.fastq  JP4D_R2.fastq.gz  TruSeq3-PE.fa 

Ejecutaremos Trimmomatic en una de las muestras de extremos emparejados. 

Mientras usábamos FastQC, vimos que había adaptadores universales en nuestras muestras. Las secuencias del adaptador vienen con la instalación de Trimmomatic y se encuentran en nuestro directorio actual en el archivo TruSeq3-PE.fa.
