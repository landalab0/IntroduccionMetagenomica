# Recorte y Filtrado✂️

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

Usaremos una ventana deslizante de tamaño 4 que eliminará las bases si su puntuación Phred es inferior a 20 y descartaremos cualquier lectura a la que no le queden menos de 25 bases después de este paso de recorte.

Comprimiremos un archivo nuevamente para utilizarlo antes de ejecutar Trimmomatic. 

**`Code`**

$ gzip JP4D_R1.fastq


**`Code`**

$ trimmomatic PE JP4D_R1.fastq.gz JP4D_R2.fastq.gz \
JP4D_R1.trim.fastq.gz  JP4D_R1un.trim.fastq.gz \
JP4D_R2.trim.fastq.gz  JP4D_R2un.trim.fastq.gz \
SLIDINGWINDOW:4:20 MINLEN:35 ILLUMINACLIP:TruSeq3-PE.fa:2:40:15


**`Output`**

TrimmomaticPE: Started with arguments:
 JP4D_R1.fastq.gz JP4D_R2.fastq.gz JP4D_R1.trim.fastq.gz JP4D_R1un.trim.fastq.gz JP4D_R2.trim.fastq.gz JP4D_R2un.trim.fastq.gz SLIDINGWINDOW:4:20 MINLEN:35 ILLUMINACLIP:TruSeq3-PE.fa:2:40:15
Multiple cores found: Using 2 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 1123987 Both Surviving: 751427 (66.85%) Forward Only Surviving: 341434 (30.38%) Reverse Only Surviving: 11303 (1.01%) Dropped: 19823 (1.76%)
TrimmomaticPE: Completed successfully

Ahora veremos que nuestros archivos de salida sigan ahí:

**`Code`**

$ ls JP4D*

**`Output`**

JP4D_R1.fastq.gz       JP4D_R1un.trim.fastq.gz	JP4D_R2.trim.fastq.gz
JP4D_R1.trim.fastq.gz  JP4D_R2.fastq.gz		JP4D_R2un.trim.fastq.gz

Nuestros archivos de salida son archivos FASTQ, y deberían ser más pequeños que los archivos de entrada porque hemos eliminado las lecturas. 

**`Code`**

$ ls JP4D* -l -h

**`Output`**

-rw-r--r-- 1 dcuser dcuser 179M Nov 26 12:44 JP4D_R1.fastq.gz
-rw-rw-r-- 1 dcuser dcuser 107M Mar 11 23:05 JP4D_R1.trim.fastq.gz
-rw-rw-r-- 1 dcuser dcuser  43M Mar 11 23:05 JP4D_R1un.trim.fastq.gz
-rw-r--r-- 1 dcuser dcuser 203M Nov 26 12:51 JP4D_R2.fastq.gz
-rw-rw-r-- 1 dcuser dcuser 109M Mar 11 23:05 JP4D_R2.trim.fastq.gz
-rw-rw-r-- 1 dcuser dcuser 1.3M Mar 11 23:05 JP4D_R2un.trim.fastq.gz

¡Ejecutamos Trimmomatic! Ufff, lo hicimosss. Pero ojito 👀 solo lo hicimos en una archivo FASTQ, ya que Trimmomatuc solo puede operar con una muestra a la vez, y tenemos más de una muestra. ¿Y ahora? Podemos usar un **`for`** bucle para recorrer nuestros archivps de muestra rápidamente.

**`Code`**
$ for infile in *_R1.fastq.gz
do
base=$(basename ${infile} _R1.fastq.gz)
trimmomatic PE ${infile} ${base}_R2.fastq.gz \
${base}_R1.trim.fastq.gz ${base}_R1un.trim.fastq.gz \
${base}_R2.trim.fastq.gz ${base}_R2un.trim.fastq.gz \
SLIDINGWINDOW:4:20 MINLEN:35 ILLUMINACLIP:TruSeq3-PE.fa:2:40:15
done


Trimmomatic se habrá ejecutado para cada uno de nuestros cuatro archivos de entrada, para verlo, identificaremos el contenido de nuestro directorio. Veremos que aunque ejecutamos Trimmomatic en el archivo JP4D antes de ejecutar el bucle for, solo hay un conjunto de archivos para él ya que lo ejecutamos dos veces y sobreescribimos nuestros primeros resultados.

**`Code`**

$ ls

**`Output`**

JC1A_R1.fastq.gz                     JP4D_R1.fastq.gz                                    
JC1A_R1.trim.fastq.gz                JP4D_R1.trim.fastq.gz                                
JC1A_R1un.trim.fastq.gz              JP4D_R1un.trim.fastq.gz                                
JC1A_R2.fastq.gz                     JP4D_R2.fastq.gz                                 
JC1A_R2.trim.fastq.gz                JP4D_R2.trim.fastq.gz                                 
JC1A_R2un.trim.fastq.gz              JP4D_R2un.trim.fastq.gz                                 
TruSeq3-PE.fa   

Completamos los pasos de recorte y filtrado de nuestro proceso de control de calidad. Ahora moveremos nuestros archivos FASTQ recortados a un nuevo subdirectorio dentro de nuestro directorio data/

**`Code`**

$ cd ~/dc_workshop/data/untrimmed_fastq
$ mkdir ../trimmed_fastq
$ mv *.trim* ../trimmed_fastq
$ cd ../trimmed_fastq
$ ls

**`Output`**

JC1A_R1.trim.fastq.gz    JP4D_R1.trim.fastq.gz                
JC1A_R1un.trim.fastq.gz  JP4D_R1un.trim.fastq.gz              
JC1A_R2.trim.fastq.gz    JP4D_R2.trim.fastq.gz                
JC1A_R2un.trim.fastq.gz  JP4D_R2un.trim.fastq.gz

