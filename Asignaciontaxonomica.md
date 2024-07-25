## Asignación Taxonómica 

Es el proceso de asignación de una Unidad Taxonómica Operativa (OTU) a secuencias que pueden ser lecturas o contigs. Las secuencias se comparan con una base de datps construida utilizando genomas completos. Cuando una secuencia encuentra una coincidencia lo suficientemente buena en la base de datos, se asigna a la OTU correspondiente. 

Existen diferentes maneras para realizar la asignación taxonómica así como programas que realizan estos mapeos taxonómicos:
1. BLAST: buscan la coincidencia más probable para cada secuencia dentro de una base de datos de genomas.
2. Marcadores: buscan marcadores de una base de datos realizada a priori en las secuencias a clasificar y les asignan ña taxonomía en función de los hits obtenidos.
3. K-mers: Una base de datos de genomas se divide en fragmentos de longitud k para poder buscar fragmentos únicos por grupo taxonómico, desde el ancestro común más bajo (LCA), pasando por el filo hasta la especie. Luego, el algoritmo divide la secuencia de consulta (lecturas/contigs) en fragmentos de longitud k, busca dónde se ubican dentro del árbol y realiza la clasificación con la posición más probable.

 Uno de nuestros resultados será la abundacia de cada taxón u OTU en la muestra. La abundacia absoluta de un taxón es la cantidad de secuencias (lecturas o contigs) asignadas a él. Y su abundancia relativa es, la proporción de secuencias asignadas a él. Es importante tener en cuenta los sesgos que pueden hacer variar las abundacias.

## Kraken2

  Es un sistema de clasificación taxonómica que utiliza coincidencias exactas de k-mers para lograr una alta precisipib y velocidades de clasificación rápidas.

  Para utilizarlo necesitamos nuestros archivos de netrada y una base de datos para compararlos. La base de datos más pequeña de Kraken, Mninikraken, necesita 8 GB  de RAM libre, por lo tanto no se puede utilizar en cualquier computadora.

  # Asignación taxonómica de lecturas metagenómicas

  Estas asiganaciones se pueden realizar antes del ensamblaje. Utilizaríamos archivos FASTQ como entradas, como  **JP4D_R1.trim.fastq.gz** y **JP4D_R2.trim.fastq.gz**.  Lo que obtendríamos serían dos archivos: el infrome **JP4D.report** y el archivo kraken **JP4D.kraken**.

  Para ejecutar kraken2, utilizaríamos este comando:

```bash
 mkdir TAXONOMY_READS
kraken2 --db kraken-db --threads 8 --paired JP4D_R1.trim.fastq.gz JP4D_R2.trim.fastq.gz --output TAXONOMY_READS/JP4D.kraken --report TAXONOMY_READS/JP4D.report
```

Las salidas precalculasdas fueron: **JP4D-kraken.kraken** y **JP4D.report**.

 ```bash
head ~/dc_workshop/taxonomy/JP4D.kraken
```

**`Output`** 

U	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:19691:2037	0	250|251	0:216 |:| 0:217
U	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:14127:2052	0	250|238	0:216 |:| 0:204
U	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:14766:2063	0	251|251	0:217 |:| 0:217
C	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:15697:2078	2219696	250|120	0:28 350054:5 1224:2 0:1 2:5 0:77 2219696:5 0:93 |:| 379:4 0:82
U	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:15529:2080	0	250|149	0:216 |:| 0:115
U	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:14172:2086	0	251|250	0:217 |:| 0:216
U	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:17552:2088	0	251|249	0:217 |:| 0:215
U	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:14217:2104	0	251|227	0:217 |:| 0:193
C	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:15110:2108	2109625	136|169	0:51 31989:5 2109625:7 0:39 |:| 0:5 74033:2 31989:5 1077935:1 31989:7 0:7 60890:2 0:105 2109625:1
C	MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:19558:2111	119045	251|133	0:18 1224:9 2:5 119045:4 0:181 |:| 0:99

Para entenderlo mejor:

|Ejemplo de columna   |   Descripción    |
|---------------------|----------------- |
|78.13                |  Porcentaje de lecturas cubiertas por el clado enraizado en este taxón|
|587119               |  Número de lecturas cubiertas por el clado enraizado en este taxón|
| 587119              | Número de lecturas asignadas directamente a este taxón |
| U                  | Código de rango que indica (S)inclasificado, (D)ominio, (R)ino, (F)ilo, (C)lase, (O)rden, (F)amilia, (G)eno o (E)specie. Todos los demás rangos son simplemente '-'.|
| 0                    | Identificación de taxonomía del NCBI |
| unclassified         | Nombre científico indentado |

 # Asignación taxonómica de contigs de un MAG

Ahora tenemos la identidad taxonómica de las lecturas de todo el metagenoma, pero necesitamos saber a qué taxón corresponden nuestros MAG. Para ello, tenemos que hacer la asignación taxonómica con sus contigs en lugar de sus lecturas porque no tenemos las lecturas correspondientes a un MAG separado de las lecturas de toda la muestra.

No lo ejecutaremos, los resultados están calculados en el directorio ~/dc_workshop/taxonomy/mags_taxonomy/. Este sería el comando para JP4D.001.fasta MAG:

 ```bash
mkdir TAXONOMY_MAG
kraken2 --db kraken-db --threads 12 -input JP4D.001.fasta --output TAXONOMY_MAG/JP4D.001.kraken --report TAXONOMY_MAG/JP4D.001.report
```

**`Output`**

JP4D.001.kraken
JP4D.001.report

 ```bash
more ~/dc_workshop/taxonomy/mags_taxonomy/JP4D.001.report
```

**`Output`**
 50.96	955	955	U	0	unclassified
 49.04	919	1	R	1	root
 48.83	915	0	R1	131567	  cellular organisms
 48.83	915	16	D	2	    Bacteria
 44.40	832	52	P	1224	      Proteobacteria
 19.37	363	16	C	28216	        Betaproteobacteria
 16.22	304	17	O	80840	          Burkholderiales
  5.66	106	12	F	506	            Alcaligenaceae
  2.72	51	3	G	517	              Bordetella
  1.12	21	21	S	2163011	                Bordetella sp. HZ20
  .
  .
  .

  
