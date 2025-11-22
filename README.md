# Semana 13: Análisis metataxonómico utilizando datos de secuenciación de Nanopore

## Logro de la sesión: 

Al finalizar la sesión, el estudiante conoce y utiliza herramientas bioinformáticas para realizar un análisis metataxonómico a partir de los datos de secuenciación Nanopore.

# Estructura de la práctica:

1. Acceso al servidor de cómputo
2. Análisis de calidad del secuenciamiento Nanopore
3. Limpieza de los archivos FASTQ 
4. Identificación del perfil taxonómico a partir de los archivos FASTQ
5. Obtención del archivo BIOM
6. Análisis de diversidad  

## Programas requeridos:

### Programas de acceso al servidor:

PuTTY v0.79 https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
   - **Descripción:** PuTTY es un cliente SSH, Telnet y Rlogin gratuito y de código abierto para Windows y sistemas Unix. Se utiliza principalmente para establecer conexiones seguras de línea de comandos a servidores remotos.

WinSCP v6.1 https://winscp.net/eng/download.php
   - **Descripción:** WinSCP es un cliente SFTP, FTP, WebDAV, Amazon S3 y SCP gratuito y de código abierto para Windows. Permite la transferencia segura de archivos entre un ordenador local y servidores remotos mediante una interfaz gráfica de usuario. 

### Programas bioinformáticos:

Emu v3.5.1 https://github.com/treangenlab/emu
   - **Descripción:** Emu (Evolutionary Microbial Unification) es una herramienta avanzada para la cuantificación precisa de microorganismos, optimizada para datos de secuenciación completa del gen 16S rRNA. Utiliza algoritmos sofisticados para asignar lecturas de forma precisa, incluso con errores, lo que permite una identificación fiable a nivel de especie. Es ideal para desentrañar la composición de comunidades microbianas complejas usando datos de 16S, especialmente de tecnologías de lectura larga.

MultiQC v1.28.0 https://multiqc.info
   - **Descripción:** MultiQC es una herramienta que agrega informes de control de calidad de múltiples herramientas de análisis bioinformático en un único informe HTML interactivo. Es compatible con una amplia gama de herramientas, incluyendo FastQC, NanoPlot, PycoQC y Trim Galore!, facilitando la revisión y comparación de los resultados de control de calidad de múltiples muestras.

Nanofilt v2.8.0 https://github.com/wdecoster/nanofilt
   - **Descripción:** NanoFilt es una herramienta para filtrar datos de secuenciación de Nanopore basándose en la calidad y la longitud de los reads. Permite seleccionar reads de alta calidad para análisis posteriores.

NanoPlot v1.41.6 https://github.com/wdecoster/NanoPlot
   - **Descripción:** NanoPlot es una herramienta para la visualización de datos de secuenciación de Nanopore. Genera varios tipos de gráficos para evaluar la calidad y las características de los reads, como la distribución de longitudes y la calidad a lo largo de los reads.

Phyloseq v1.46.0 https://github.com/joey711/phyloseq
   - **Descripción:** Phyloseq es un paquete esencial de R que centraliza el análisis y visualización de datos de microbiomas de alta complejidad. Permite integrar tablas de OTUs/ASVs, taxonomía, metadatos y árboles filogenéticos en un solo objeto. Ofrece funciones robustas para filtrado, normalización, cálculo de diversidad y ordenación, facilitando el estudio de comunidades microbianas y promoviendo la reproducibilidad en la investigación.

### Herramientas bioinformáticas en línea:

Easy16S https://shiny.migale.inrae.fr/app/easy16S
   - **Descripción:** Easy16S es una plataforma web Shiny intuitiva para el análisis y visualización de datos de microbioma 16S, construida sobre phyloseq. Permite a usuarios de todos los niveles realizar análisis de diversidad alfa y beta, además de explorar la composición taxonómica y mapear covariables de forma sencilla. Su integración con FROGS y la exportación de resultados lo hacen una herramienta versátil. Es ideal para una rápida exploración de datos y formación, simplificando el acceso al estudio del microbioma.
   
MicrobiomeAnalyst v2.0 https://www.microbiomeanalyst.ca/MicrobiomeAnalyst/home.xhtml
   - **Descripción:** MicrobiomeAnalyst es una plataforma web fácil de usar para el análisis estadístico, visual y meta-análisis integral de datos de microbioma. Está diseñada para ser accesible a investigadores y clínicos con diversos niveles de experiencia en bioinformática, proporcionando una interfaz intuitiva para explorar una amplia variedad de métodos establecidos. MicrobiomeAnalyst soporta datos de genes marcadores (por ejemplo, 16S rRNA) y datos de metagenómica de escopeta (shotgun). Sus módulos permiten el perfilado de la comunidad, análisis comparativos, predicción funcional e integración con conjuntos de datos públicos o firmas microbianas conocidas. Las funcionalidades clave incluyen el procesamiento y la normalización de datos, diversos análisis estadísticos (por ejemplo, abundancia diferencial), visualizaciones interactivas (por ejemplo, gráficos PCoA, mapas de calor, redes de correlación) e interpretación funcional (por ejemplo, análisis de redes metabólicas). También ofrece un módulo de procesamiento de datos brutos para datos de amplicones y un módulo para integrar datos de microbioma y metabolómica.

## Metodología:

## 1.	Acceso al servidor de cómputo

### Abrir el programa PuTTY, colocar el hostname ( 10.142.250.66 ) y port ( 22 ), y dar clic en Open:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/92f89dbb-1a21-411d-adb5-38fe486a5567" />



### En la terminal abierta, escribir su usuario y contraseña correspondiente para tener acceso al servidor de cómputo Tensor:
 
<img width="700" alt="image" src="https://github.com/user-attachments/assets/4d246e93-c59c-4749-a2dd-03db25c53654" />


### Abrir el programa WinSCP, colocar el hostname ( 10.142.250.66 ) y port ( 22 ), escribir su usuario y contraseña correspondiente para tener acceso al servidor de cómputo Tensor, y hacer clic en Login:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ef4dc253-ce4a-417d-b761-39692d2a011a" />

<img width="500" alt="image" src="https://github.com/user-attachments/assets/577debca-6085-47c5-9bbd-73688bfa8bb0" />

## 2. Análisis de calidad del secuenciamiento Nanopore

```bash
cd

cd metataxonomic

mkdir nanopore

cd nanopore

mkdir quality

cd quality

# Para ITS

conda activate quality

NanoPlot -t 5 --fastq /data/2025_2/metagenomics/metataxonomic/final/omicas/its/barcode10_its.fastq.gz -p b10_its_ --only-report --maxlength 5000 -o .

multiqc -o rizo_its .

# Para 16S

conda activate quality

NanoPlot -t 5 --fastq /data/2025_2/metagenomics/metataxonomic/final/omicas/16s/barcode10_16s.fastq.gz -p b10_16s_ --only-report --maxlength 5000 -o .

multiqc -o rizo_16s .
```

## 3. Limpieza de los archivos FASTQ 

```bash
cd ~/metataxonomic/nanopore

mkdir trim

cd trim

# Para ITS

porechop -t 2 -i /data/2025_2/metagenomics/metataxonomic/final/omicas/its/barcode10_its.fastq.gz -o b10_its_porechop.fastq.gz

gunzip -c b10_its_porechop.fastq.gz | NanoFilt -q 10 --length 400 --maxlength 2000 | gzip > b10_its_nanofilt.fastq.gz

# Para 16S

porechop -t 2 -i /data/2025_2/metagenomics/metataxonomic/final/omicas/16s/barcode10_16s.fastq.gz -o b10_16s_porechop.fastq.gz

gunzip -c b10_16s_porechop.fastq.gz | NanoFilt -q 10 --length 1000 --maxlength 2000 | gzip > b10_16s_nanofilt.fastq.gz
```

```bash
# Crear el script count.sh con nano:

nano count.sh

# Pegar la siguiente información:

#!/bin/bash

echo "--- Conteo de Lecturas por Muestra (Archivos FASTQ.GZ) ---"
echo "Archivo FASTQ.GZ | Número de Lecturas"
echo "-----------------------------------"

# Loop through all .fastq.gz files in the current directory
for file in *.fastq.gz; do
    if [ -f "$file" ]; then
        # Decompress the file on the fly and count lines
        num_lines=$(zcat "$file" | wc -l | awk '{print $1}')
        # Each read in a FASTQ file (compressed or not) consists of 4 lines
        num_reads=$(echo "$num_lines / 4" | bc)
        printf "%-20s | %s\n" "$file" "$num_reads"
    fi
done

echo "-----------------------------------"

# Ejecutar el script:

sh count.sh
```

## 4. Identificación del perfil taxonómico a partir de los archivos FASTQ

```bash
cd ~/metataxonomic/nanopore

mkdir taxonomy

cd taxonomy

conda activate metataxonomic

# Para ITS

emu abundance --type map-ont --threads 10 --db /data/db/emu/unite/ --output-dir . ~/metataxonomic/nanopore/trim/b10_its_nanofilt.fastq.gz

Copiar los archivos *_its_nanofilt.fastq_rel-abundance.tsv a la carpeta /data/2025_2/its_alumnos

Copiar en su carpeta los archivos *_its_nanofilt.fastq_rel-abundance.tsv necesarios

emu combine-outputs . tax_id

# Para 16S

emu abundance --type map-ont --threads 10 --db /data/db/emu/emu/ --output-dir . ~/metataxonomic/nanopore/trim/b10_16s_nanofilt.fastq.gz

Copiar los archivos *_16s_nanofilt.fastq_rel-abundance.tsv a la carpeta /data/2025_2/16s_alumnos

Copiar en su carpeta los archivos *_16s_nanofilt.fastq_rel-abundance.tsv necesarios

emu combine-outputs . tax_id
```

## 5. Obtención del archivo BIOM

```bash
# Para ITS

awk 'BEGIN{ FS = OFS = "\t" } { print "", $0 }' emu-combined-tax_id.tsv > rizo_its_emu.tsv

awk '{if (NR == 1) {print $0} else {print NR-2, $0}}' rizo_its_emu.tsv > tmp && mv tmp rizo_its_emu.tsv

tr '\t' ',' < rizo_its_emu.tsv | sed 's/_nanofilt.fastq//g' > rizo_its_emu.csv

awk 'BEGIN{FS=OFS=","}{sub(/\r$/,"");print $1,$9,$8,$7,$6,$5,$4,$3,$2,$10,$11,$12,$13,$14,$15,$16,$17}' rizo_its_emu.csv > tmp && mv tmp rizo_its_emu.csv

sed 's/,$//' rizo_its_emu.csv | sed 's/superkingdom/Kingdom/g' | sed 's/phylum/Phylum/g' | sed 's/class/Class/g' | sed 's/order/Order/g' | sed 's/family/Family/g' | sed 's/genus/Genus/g' | sed 's/species/Species/g' | sed 's/_its//g' > rizo_its_emu_final.csv

# Para 16S

awk 'BEGIN{ FS = OFS = "\t" } { print "", $0 }' emu-combined-tax_id.tsv > rizo_16s_emu.tsv

awk '{if (NR == 1) {print $0} else {print NR-2, $0}}' rizo_16s_emu.tsv > tmp && mv tmp rizo_16s_emu.tsv

tr '\t' ',' < rizo_16s_emu.tsv | sed 's/_nanofilt.fastq//g' > rizo_16s_emu.csv

awk 'BEGIN{FS=OFS=","}{sub(/\r$/,"");print $1,$9,$8,$7,$6,$5,$4,$3,$2,$10,$11,$12,$13,$14,$15,$16,$17}' rizo_16s_emu.csv > tmp && mv tmp rizo_16s_emu.csv

sed 's/,$//' rizo_16s_emu.csv | sed 's/superkingdom/Kingdom/g' | sed 's/phylum/Phylum/g' | sed 's/class/Class/g' | sed 's/order/Order/g' | sed 's/family/Family/g' | sed 's/genus/Genus/g' | sed 's/species/Species/g' | sed 's/_16s//g' > rizo_16s_emu_final.csv

## Crear en Windows la carpeta metataxonomic, copiar el archivo CSV final y descomprimir el archivo rizo_metataxonomica.zip que está en el aula virtual.

## Abrir RStudio y seguir las indicaciones del docente.

## Cargar el archivo generado en el programa Easy16S (https://shiny.migale.inrae.fr/app/easy16S) y exportarlo en BIOM (junto con su metadata en CSV).
```

## 6. Análisis de diversidad  

Abrir el archivo BIOM en el programa microbiomeanalyst(https://www.microbiomeanalyst.ca/MicrobiomeAnalyst/ModuleView.xhtml) y seguir las indicaciones del docente

# Bitácora de la práctica:

## Resultados

```bash
•	Flujograma de todo el pipeline realizado 
•	Valores de calidad y numero de lecturas en la data cruda y después de la limpieza 
•	Análisis de abundancia en todos los niveles taxonómicos
•	Análisis de diversidad alfa (seleccionar 3 métricas) a nivel de género y familia 
•	Análisis de beta diversidad (PCoA y NMDS) a nivel de género y familia
```













