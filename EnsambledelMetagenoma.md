# Ensamblaje del Metagenoma 🧩 

 Cóntigo (contig): es un conjunto de segmentos o secuencias de ADN que se superponen parcialmente de forma tal que colectivamente dan una representación continua de región genómica ( ,2024).

 El proceso de ensambaje agrupa las lecturas en cóntigos y los cóntigos en estructuras (scaffold) para obtener idealmente la secuencia de un cromosoma completo. 

 Existen distintos programas dedicados al ensamblaje del genoma y metagenoma, como la extensión Greedy, OLC y los gráficos de De Brujin. EL shotgun metagenomics necesita un paso de ensamblaje.

 MetaSPAdes es un ensamblador NGS de novo para ensamblar datos metagenómicos grandes y complejos, y es uno de los más utilizados. Es parte del kit de herramientas SPAdes, que contiene varios canales de ensamblaje.

Problemas que se pueden presentar en el ensamblaje metagenómico:

-> Diferencias de cobertura entre los genomas debido a las diferencias de abuendancia en la muestra.
-> Diferentes especies a menudo comparten regiones conservadas.
-> Presencia de varias cepas de una sola especie en la comunidad.

Para trabajar iniciaremos una nueva sesión en la terminal, y le daremos un nombre descriptivo:

```bash
screen -S assembly
```

Si necesitas salir de la sesión puedes hacerlo presionando **control + a** seguido de **d**

Ahora veremos si el programa está instalado:

```bash
metaspades.py
```

   **`Output`** 
SPAdes genome assembler v3.15.0 [metaSPAdes mode]

Usage: spades.py [options] -o <output_dir>

Basic options:
  -o <output_dir>             directory to store all the resulting files (required)
  --iontorrent                this flag is required for IonTorrent data
  --test                      runs SPAdes on a toy dataset
  -h, --help                  prints this usage message
  -v, --version               prints version

Input data:
  --12 <filename>             file with interlaced forward and reverse paired-end reads
  -1 <filename>               file with forward paired-end reads
  -2 <filename>               file with reverse paired-end reads

Si el ambiente "metagenomics" no está activado, el comando anterior marcara error. Activa el ambiente:

``` bash
conda activate metagenomics
```

Especificaremos las lecturas de extremo emparejando directo con -1 y lecturas de extremo emparejado inverso con -2, y el directorio de salida en donde queremos que se almacenen nuestros datos.

 ``` bash
cd ~/dc_workshop/data/trimmed_fastq
metaspades.py -1 JC1A_R1.trim.fastq.gz -2 JC1A_R2.trim.fastq.gz -o ../../results/assembly_JC1A
```
Ahora que se está ejecutando, desconectaremos nuestra pantalla **control + a** **d** y esperar unos minutos mientras se ejecuta. Y luego adjunta la pantalla **screen -r assembly** para ver cómo te fue.

Cuando haya finalizado podremos ver:

   **`Code`** 

======= SPAdes pipeline finished.

SPAdes log can be found here: /home/dcuser/dc_workshop/results/assembly_JC1A/spades.log

Thank you for using SPAdes!

Cerraremos la pantalla con **exit** para ver los resultados en la pantalla principal.

Iremos al directorio en donde se encuentras los archivos de salida:

``` bash
cd ../../results/assembly_JC1A
ls -F
```

   **`Output`** 
   
assembly_graph_after_simplification.gfa  corrected/              K55/             scaffolds.fasta
assembly_graph.fastg                     dataset.info            misc/            scaffolds.paths
assembly_graph_with_scaffolds.gfa        first_pe_contigs.fasta  params.txt       spades.log
before_rr.fasta                          input_dataset.yaml      pipeline_state/  strain_graph.gfa
contigs.fasta                            K21/                    run_spades.sh    tmp/
contigs.paths                            K33/                    run_spades.yaml

MetaSPAdes nos dio muchos archivos. Los que contienen el ensamblaje son el contigs.fasta y el scaffolds.fasta. También hay tres **k** capetas: K21, K33 y K55, estas contienen los archivos de resultados indivudales para un ensamblaje con k-mers iguales a esos números. La carpeta **corrected** tienen las lecturas corregidas con el algoritmo SPAdes. El archivo **assembly_grpah_with_scaffolds.gfa** contiene la información necesaria para visualizar nuestro montaje por diferentes medios, como programas como Bandage.

Los contigs se crean a partir de lecturas ensambladas, pero los scaffolds son el resultado de un proceso posterior en el que los contigs se ordenan, orientan y conectan con N.

