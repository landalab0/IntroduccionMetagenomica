## Binning de metagenomas 📊

Los genomas que se encuentran en la muestra se pueden separar mediante el proceso llamado binning. El **Binning** permitirá el análisis por separado de cada especie contenida en el metagenoma con lecturas suficientes para reconstruir un genoma. 

Los MAGs son los Genomas Ensamblados de Metagenomas. En este proceso, los contigs ensamblados del metagenoma serán asignados a diferentes bins (archivos FASTA que contienen determinados contigs). Se bysca que cada bin corresponda a un solo genoma original (un MAG).

Existin distintas maneras de separar los contigs, mediante su asignación taxonómica, y utilizando las características de los contigs como su contenido de GC, el uso de tetranucleótidos o su cobertura.

# Maxbin 
Es un algoritmo de clasificación que divide a los contigs que pertenecen a diferentes contenedores según sus niveles de cobertura y las frecuencias de tetranucleótidos que contienen. 

Agruparemos en bin la muestra que ensamblamos. El comando para ejecutar Mxbin es run_MaxBin.pl y los argumentos que se necesitan son el archivo FASTA del ensamblaje, el FASTQ con las lecturas directas e inversas, el directorio de salida y el nombre.

```bash
 cd ~/dc_workshop/results/assembly_JC1A
 mkdir MAXBIN
 run_MaxBin.pl -thread 8 -contig JC1A_contigs.fasta -reads ../../data/trimmed_fastq/JC1A_R1.trim.fastq.gz -reads2 ../../data/trimmed_fastq/JC1A_R2.trim.fastq.gz -out MAXBIN/JC1A
```

 **`Output`** 
 
MaxBin 2.2.7
Thread: 12
Input contig: JC1A_contigs.fasta
Located reads file [../../data/trimmed_fastq/JC1A_R1.trim.fastq.gz]
Located reads file [../../data/trimmed_fastq/JC1A_R2.trim.fastq.gz]
out header: MAXBIN/JC1A
Running Bowtie2 on reads file [../../data/trimmed_fastq/JC1A_R1.trim.fastq.gz]...this may take a while...
Reading SAM file to estimate abundance values...
Running Bowtie2 on reads file [../../data/trimmed_fastq/JC1A_R2.trim.fastq.gz]...this may take a while...
Reading SAM file to estimate abundance values...
Searching against 107 marker genes to find starting seed contigs for [JC1A_contigs.fasta]...
Running FragGeneScan....
Running HMMER hmmsearch....
Try harder to dig out marker genes from contigs.
Marker gene search reveals that the dataset cannot be binned (the medium of marker gene number <= 1). Program stop.

El Output menciona que es imposible clasificar el conjunto de datos que ingresamos porque el número de genes marcadores es inferior a 1. Utilizaremos otra muestra del mismo estudio que es más grande. El ensamblaje precalculado está en dc-workshop/mags/.

```bash
 cd ~/dc_workshop/mags/
 mkdir MAXBIN
 run_MaxBin.pl -thread 8 -contig JP4D_contigs.fasta -reads ../data/trimmed_fastq/JP4D_R1.trim.fastq.gz -reads2 ../data/trimmed_fastq/JP4D_R2.trim.fastq.gz -out MAXBIN/JP4D
```

  **`Output`** 

  ========== Job finished ==========
Yielded 4 bins for contig (scaffold) file JP4D_contigs.fasta

Here are the output files for this run.
Please refer to the README file for further details.

Summary file: MAXBIN/JP4D.summary
Genome abundance info file: MAXBIN/JP4D.abundance
Marker counts: MAXBIN/JP4D.marker
Marker genes for each bin: MAXBIN/JP4D.marker_of_each_gene.tar.gz
Bin files: MAXBIN/JP4D.001.fasta - MAXBIN/JP4D.004.fasta
Unbinned sequences: MAXBIN/JP4D.noclass

Store abundance information of reads file [../data/trimmed_fastq/JP4D_R1.trim.fastq.gz] in [MAXBIN/JP4D.abund1].
Store abundance information of reads file [../data/trimmed_fastq/JP4D_R2.trim.fastq.gz] in [MAXBIN/JP4D.abund2].


========== Elapsed Time ==========
0 hours 6 minutes and 56 seconds.

Con .summary archivo, podremos ver los bins que Maxbin produjo.


```bash
cat MAXBIN/JP4D.summary
```

  **`Output`** 

 Bin name	Completeness	Genome size	GC content
JP4D.001.fasta	57.9%	3141556	55.5
JP4D.002.fasta	87.9%	6186438	67.3
JP4D.003.fasta	51.4%	3289972	48.1
JP4D.004.fasta	77.6%	5692657	38.9 

La calidad del MAG es altamente dependiente del tamaño del genoma de las especies, su abundacia en la comunidad y la profundidad a la que se secuenció. 

CheckM es un programa para medir la calidad de los MAG ( ¿es un genoma completo?, si está contaminado ¿el MAG contiene solo un genoma?). Mide la integridad y la contaminación contando los genes marcadores  en los MAG. 

Ejecutaremos CheckM especificando el uso de marrcadores a nivel dominio, para el rango Bacteria, también especificaremos que nuestros bins están en formato FASTA, y que se encuentran en el directorio MAXBIN, y finalmente, que los resultados los queremos en el directorio CHECKM/.

```bash
 mkdir CHECKM
 checkm taxonomy_wf domain Bacteria -x fasta MAXBIN/ CHECKM/
```

  **`Output`** 
  

-------------------------------------------------------------------------------------
  Bin Id     Marker lineage   # genomes   # markers   # marker sets   0    1    2    3    4   5+   Completeness   Contamination   Strain heterogeneity  
-------------------------------------------------------------------------------------
  JP4D.002      Bacteria         5449        104            58        3    34   40   21   5   1       94.83           76.99              11.19          
  JP4D.004      Bacteria         5449        104            58        12   40   46   6    0   0       87.30           51.64               3.12          
  JP4D.001      Bacteria         5449        104            58        24   65   11   3    1   0       70.48           13.09               0.00          
  JP4D.003      Bacteria         5449        104            58        44   49   11   0    0   0       64.44           10.27               0.00          
--------------------------------------------------------------------------------------------------------------------------------------------------------

Para poder visualizar de mejor manera el output, podemos correr el paso de calidad de CheckM **checkm qa** y hacer que se imprima el output en una tabla **tsv** en lugar de la consola.

Estamos buscando obtener un solo contig por bin, con una longitud parecida a la del genoma de los taxones correspondientes. Dado que esto es dificil de obtener, se utilizan algunos parámetros que muestren la calidad del ensamblaje.

Para obtenerla tabla con los parametros extra, necesitamos especificar el archivo de los marcadores que CheckM utilizó en el paso anterior, **Bacteria.ms**, el nombre de archivo de salida que queremos, **quality_JP4D.tsv**, el archivo donde queremos la tabla **--tab_table**, y la opción número 2 **-o 2** es para pedir los parámetros extra en la tabla. 

```bash
 mkdir CHECKM
 checkm qa CHECKM/Bacteria.ms CHECKM/ --file CHECKM/quality_JP4D.tsv --tab_table -o 2
```

