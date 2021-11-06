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

```
grep "MW" covid-samples.fasta | sort | cut -c 1-11 | sort -n 
>MW181431.1
>MW186669.1
>MW186829.1
>MW186830.1
```
Con esta orden llego hasta ordenar la primera palabra de las seq pero cuando intento volcar estos datos en la salida al fichero me da error y no sé porqué ¿?

```
grep "MW" covid-samples.fasta | sort | cut -c 1-11 | sort -n > covid.seq.txt
-bash: covid.seq.txt: Permiso denegado
```
No puedo volcar la salida de grep

```
grep "MW" covid-samples.fasta | sort | cut -c 1-11 |tee seq.txt | sort -n > covid.seq.txt
-bash: covid.seq.txt: Permiso denegado
tee: seq.txt: Permiso denegado
```
***jajjja YA LO TENGO !!***  **HE COPIADO A MI DIRECTORIO EL ARCHIVO Y AHORA ME DEJA¡¡¡¡**

```
mcuadrado@cpg3:~$ cp -r /home/gtfs/covid-samples.fasta covid-copy.fasta 
mcuadrado@cpg3:~$ grep "MW" covid-copy.fasta | sort | cut -c 1-11 |tee seq.txt | sort -n > covid.seq.txt
mcuadrado@cpg3:~$ cat covid.seq.txt
>MW181431.1
>MW186669.1
>MW186829.1
>MW186830.1
```





## Ejercicio 4

Encuentra, usando una sola línea, el número de usuarias diferentes que tienen al menos una carpeta a su nombre en el '/home' de CPG3.

### Respuesta ejercicio 4

```
 mcuadrado@cpg3:~$ ls -la /home | cut -f 3 -d " " |sort | uniq -c 
      1 
      2 1
      9 2
      1 3
      2 6
      2 7
      1 8
      2 9
      1 abenito
      1 adominguez
      1 adrianambroa
      1 agarmar
      1 alba
      1 analaura
      1 anaromero
      1 arqcarmel
      1 jenr
      1 laudupri
      1 mcuadrado
      1 negido
      1 nelmarin
      1 nguerrero
      1 pedrojf
      1 rodri
      2 root
      1 smuntion
      1 theron
```
      
   No entiendo porqué aparecen esos números al principio y desaparecen ciertos usuarios como cpg o ccalvo. 

```

mcuadrado@cpg3:~$ ls -l  
total 368
drwxr-x--- 54 abenito             abenito              4096 nov  2 19:04 abenito
drwxr-x---  9 adominguez          adominguez           4096 oct 26 18:32 adominguez
drwxr-x--- 35 adrianambroa        adrianambroa         4096 ago 24 10:43 adrianambroa
drwxr-x--- 12 agarmar             agarmar              4096 jun 14 10:43 agarmar
drwxr-x--- 24 alba                alba                 4096 jun 14 10:43 alba
drwxrwxrwx  8 alejandro           alejandro            4096 nov  2 19:04 alejandro
-rw-r--r--  1 root                root                70880 may 20 17:48 alumnos.tar
drwxr-x--- 14 analaura            analaura             4096 nov  2 19:08 analaura
drwxr-x--- 21 anaromero           anaromero            4096 ago 24 10:43 anaromero
drwxr-x--- 28 arqcarmel           arqcarmel            4096 jun 14 10:43 arqcarmel
drwxr-x---  9 ccalvo              ccalvo               4096 oct 26 18:20 ccalvo
drwxr-x---  2 cpg                 cpg                  4096 sep 28 09:24 cpg
drwxr-xr-x  6 eval_user           eval_user            4096 oct 14 11:22 eval_user
drwxr-x---  2 giselcp             giselcp              4096 sep 15 11:51 giselcp
drwxr-xr-x  2 root                root                 4096 oct 29 10:07 gtfs
drwxr-x---  2 gualtierolugato     gualtierolugato      4096 sep 15 11:24 gualtierolugato
drwxr-x--- 10 jenr                jenr                 4096 oct 13 22:22 jenr
drwxr-x---  3 jibri               jibri                4096 jun 14 10:43 jibri
-rw-------  1 root                root                70880 may 20 17:48 jupyterhub_cookie_secret
-rw-r--r--  1 root                root                70880 may 20 17:48 jupyterhub.sqlite
drwxr-x--- 27 laudupri                           1046  4096 jun 14 10:43 laudupri
drwxr-x---  2 lauraalvarezalvarez lauraalvarezalvarez  4096 jun 14 10:43 lauraalvarezalvarez
drwxr-x---  2 lcorsan             lcorsan              4096 jun 14 10:43 lcorsan
drwxr-xr-x  2 lihuengd            lihuengd             4096 sep 15 11:07 lihuengd
drwxr-xr-x  7 loreto              loreto               4096 oct  1 15:55 loreto
drwx------  2 root                root                16384 jun 14 10:43 lost+found
drwxr-x---  9 madussana           madussana            4096 oct 14 18:44 madussana
drwxr-x--- 14 mcuadrado           mcuadrado            4096 nov  3 11:26 mcuadrado
drwxr-x--- 14 negido              negido               4096 oct 28 19:44 negido
drwxr-xr-x  9 nelmarin            nelmarin             4096 oct 29 12:36 nelmarin
drwxr-x--- 11 nguerrero           nguerrero            4096 nov  3 05:11 nguerrero
drwxr-x---  9 pedrojf             pedrojf              4096 oct 31 21:41 pedrojf
drwxr-x--- 43 rodri               rodri                4096 oct 20 13:22 rodri
drwxr-xr-x  7 seqview_user        seqview_user         4096 jun 14 10:43 seqview_user
drwxr-x---  2 sergiobenito        sergiobenito         4096 sep 28 09:28 sergiobenito
drwxr-x--- 12 smuntion            smuntion             4096 oct 26 18:22 smuntion
drwxr-xr-x  6                1009                1010  4096 sep  7 10:20 testuser
drwxr-x--- 15 theron              theron               4096 jun 14 10:43 theron
```

```
Para que cuente le pongo la orden final de wc -l 

mcuadrado@cpg3:~$ ls -l /home | cut -f 3 -d " " |sort | uniq -c| wc -l
26
```
Sale 26, y bueno, no sé si esto es correcto. Lo mismo sí y los usuarios desaparecidos los ha reemplazado por los números del inicio. No termino de entender que es lo que ha hecho.


_CORRECCIONES ANA Y NOELIA_

**1.** ¡Casi! Los comandos y los flags están bien, pero al haberlos separado por pipes cuentan como órdenes independientes (por eso quedan valores descolocados en la tercera columna, como 6223745), entonces lo correcto hubiera sido:
`sort -nr -k3,3 -k2,2 gene-2-desordenado.bed` 
o 
`sort -r -k3,3n -k2,2n gene-2-desordenado.bed`

**Nota: 1,9/2,5**

**2.** Los features son los correspondientes a la 3ª columna.
Eso lo podemos comprobar de un vistazo con el comando `head -n10 | column -t`. Una vez comprobado, eliminaremos las líneas de cabecera mediante el comando `tail -n+4` o `grep -v "^#"` y seleccionaremos la columna que nos interesa, la que contiene "features", mediante cut -f3. Ordenamos los datos con sort para ejecutar posteriormente uniq -c y conocer así los diferentes tipos de features existentes. Nuestro pipeline quedaría así:

`tail -n+4 Drosophila_melanogaster.BDGP6.28.102.gtf | cut -f3 | sort | uniq -c`
o
`cat Drosophila_melanogaster.BDGP6.28.102.gtf | grep -v "^#" | cut -f3 | sort | uniq -c`

Podríamos ejecutar al final `sort -nr` para ordenar los datos y así tener de un vistazo las frecuencias mayores y menores.
En el segundo caso, con el archivo Homo_sapiens.GRCh38.102.gtf.gz, podemos seguir el mismo camino, pero utilizando `zcat` en lugar de cat y, en el caso de que quisierais utilizar `tail`, tendríais que quitar dos líneas más de comentarios con `tail -n+6`. Quedaría:
`zcat Homo_sapiens.GRCh38.102.gtf.gz | tail -n+6 | cut -f3 | sort | uniq -c | sort -nr`

**Nota: 0,5/2,5**

**3.** ¡Bien! Algunas sugerencias: utilizar `grep  “>”` y `cut -f1 -d " "` para no “jugarosla” con los nombres de las secuencias o con el numero de caracteres (aunque eso lo hicimos nosotras también jaja). Más sencillo aún:
`grep ">" covid-samples.fasta | cut -f1 -d " " | sort > covid-seq-names.txt`

**Nota: 2,4/2,5**

**4.** Justo en este sí que podíais hacer la trampa del ejercicio 3, usando -c para que os saliera. También había que eliminar las primeras líneas con `tail -n+3`.
`ls -al /home | tail -n+3 | cut -c15-33 | sort | uniq | wc -l`.

**Nota: 1,7/2,5**

**Total: 6,5 :)**


