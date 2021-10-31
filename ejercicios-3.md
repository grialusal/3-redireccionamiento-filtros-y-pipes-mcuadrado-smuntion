# Sesión III - Redireccionamiento, filtros y pipes

Herramientas computacionales para bioinformática: UNIX, expresiones regulares y shell script

Edita esta plantilla en formato markdown [Guía aquí](https://guides.github.com/features/mastering-markdown/) como se pide en el guión. 
Cuando hayas acabado, haz un commit de tus cambios y súbelos al repositorio antes de la fecha de entrega señalada. 

======================================


## Ejercicio 1
`sort -R` (random) desordena las líneas de un fichero. Prueba a desordenar el fichero `gene-2.bed` y crea un nuevo fichero llamado `gene-2-desordenado.bed`.

Trata ahora de ordenar este fichero de acuerdo a los siguientes criterios: 
1. Sin cortar los elementos
2. En orden descendente
3. Usando a la vez la tercera y la segunda columna (en este orden de prioridad). Consulta el manual para ver la opción -k. 

### Respuesta ejercicio 1

```
sort -R gene-2.bed > gene-2-desordenado.bed

1- cat gene-2-desordenado.bed | sort 
DUDA: sin cortar los elementos?. Los datos están separados en columnas, no sé si la pregunta se refiere a quitar las columnas y que aparezca todo seguido ---> Esto no lo sé hacer (SANDRA HELP)

2- cat gene-2-desordenado.bed | sort -nr

3- sort   -k, --key=KEYDEF  ordena según una determinada clave;  KEYDEF  indica  el  tipo  y  localización
KEYDEF  es  F[.C][OPTS][,F[.C][OPTS]]  para  las  posiciones  inicial y final, F es un número de campo y C la posición de un caracter en  dicho
campo.  En  ambos casos empiezan en 1 y, por defecto, terminan al finalde la línea.

RESPUESTA:    cat gene-2-desordenado.bed | sort -k 3,3 -k 2,2 | sort -nr
1	6239952	6240378	GENE00000025907
1	6238262	6238384	GENE00000025907
1	6234229	6234399	GENE00000025907
1	6234229	6234399	GENE00000025907
1	6234229	6234311	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6230133	6230191	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6223599	6223745	GENE00000025907
1	6222341	6228319	GENE00000025907
1	6206227	6206270	GENE00000025907
1	6206197	6206270	GENE00000025907
```


## Ejercicio 2

Cuáles son y cuántos tipos distintos de "features" hay en `Drosophila_melanogaster.BDGP6.28.102.gtf` y en `Homo_sapiens.GRCh38.102.gtf.gz`? Nota: para trabajar con ficheros .gunzip sin descomprimir puedes usar `zcat`.

### Respuesta ejercicio 2

**`Drosophila_melanogaster.BDGP6.28.102.gtf`**  ***10 features*** : son las enumeradas abajo, después de las 3 líneas de #head
```
head -n 4 Drosophila_melanogaster.BDGP6.28.102.gtf | column -t

#!genome-build            BDGP6.28                                                                                                               
#!genome-version          BDGP6.28                                                                                                               
#!genome-build-accession  GCA_000001215.4                                                                                                       
3R   FlyBase  gene  567076  2532932  .  +  .   gene_id  "FBgn0267431";  gene_name  "Myo81F";  gene_source  "FlyBase";  gene_biotype  "protein_coding";
```

**`Homo_sapiens.GRCh38.102.gtf.gz`**  ***11 features*** : son las enumeradas abajo, después de las 5 líneas de #head
```
zcat Homo_sapiens.GRCh38.102.gtf.gz > Homo.gtf  
1- descomprimo el fichero .gz y le digo que lo vuelque en Homo.gtf

head Homo.gtf
2- miro a ver como ha quedado el fichero Homo.gtf (y me doy cuenta de que tiene un número distinto de #heads)

3- head -n6 Homo.gtf | column -t
#!genome-build            GRCh38.p13                                                                                                                               
#!genome-version          GRCh38                                                                                                                                   
#!genome-date             2013-12                                                                                                                                 
#!genome-build-accession  NCBI:GCA_000001405.28                                                                                                                   
#!genebuild-last-updated  2020-09                                                                                                                                 
1    havana    gene  11869  14409   .  +  .  gene_id  "ENSG00000223972";  gene_version  "5";  gene_name  "DDX11L1";  gene_source  "havana";  gene_biotype  "transcribed_unprocessed_pseudogene";
```


## Ejercicio 3

Recuerdas `covid-samples.fasta`? Localízalo en tu HOME, y extrae, usando un pipeline, los nombres de las secuencias contenidas en este fichero. Luego saca la primera palabra de cada una, ordénalas y guárdalas en un fichero `covid-seq-names.txt`.

### Respuesta ejercicio 3


## Ejercicio 4

Encuentra, usando una sola línea, el número de usuarias diferentes que tienen al menos una carpeta a su nombre en el '/home' de CPG3.

### Respuesta ejercicio 4





