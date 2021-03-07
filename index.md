

## Ejercicio 1

### 1. Quality Control (FastQC & Cut Adapt)


Comenzamos el pipeline con la evaluación de la calidad de los datos obtenidos a través de la secuenciación. Para ello, primero hacemos uso de ciertas herramientas que permiten observar la calidad de los fragmentos secuenciados, asi como la posible modificación de los mismos, con el objetivo de preservar únicamente aquella información con la suficiente calidad / veracidad.


#### 1.1 Quality assessment


FastQC es una herramientas que permite generar gran cantidad de gráficos que ilustran la calidad de las lecturas obtenidas, lo que en definitiva transmite información acerca de la veracidad de las mismas. Entre estas gráficas, encontramos: calidad de lectura de los fragmentos por cada base secuenciada, distribución de la calidad media de los fragmentos, contenido GC, número de no lecturas (N), distribución de la longitud de las lecturas, número de lecturas duplicadas o sobrerrepresentadas (i.e adaptadores)… etc.

En nuestro caso utilizamos FastQC, un programa con una GUI o interfaz bastante amigable, que permite realizar dichos tests de forma bastante sencilla.

Ejemplo gráficas:


#### 1.2 Filtering & Trimming


En caso de observar anomalías o problemas de calidad en las lecturas, se puede recurrir a herramientas como CutAdapt o FastQ Trimmer, que posibilitan modificarlas, excluyendo aquellas regiones o lecturas que no interesan. Estas técnicas pueden incluir el descarte de todas aquellas lecturas por debajo de un umbral de calidad determinado, la eliminación de secuencias como los adaptadores (secuencias sobrerrepresentadas que no interesan), o la acortación de estas para retener únicamente las secciones por encima de dicho límite de calidad.

Estas herramientas se podrían ejecutar a través de servidores Galaxy, que permiten hacerlo de forma sencilla, a través de nuevamente una interfaz gráfica. No obstante, ejecutarlas de forma manual a través de comandos permite obtener un poder de configuración de la herramienta significativamente mayor.

Configuramos CutAdapt para:

- Eliminar los adaptadores.
- Eliminar toda aquella secuencia por debajo de una determinada longitud.
- Eliminar todo aquel extremo 3' de las secuencias con una calidad deficiente (dado que es el extremo que suele presentar una calidad menor en Illumina).
- Eliminar toda aquella lectura en la que no se ha encontrado adaptadores.

Para ello lanzamos cutadapt con los siguientes parámetros:

```markdown
cutadapt -a ADAPTER_FWD -A ADAPTER_REV -o out.1.fastq -p out.2.fastq reads.1.fastq reads.2.fastq -m 250 -M 250 -q 28 --discard-untrimmed
```


### 2. Alineamiento

El siguiente paso consiste en el alineamiento de los fragmentos secuenciados con el genoma de referencia. Para ello no podemos utilizar un alineador tradicional, dado que los intrones harian muy dificil el proceso, al introducir sesgos en el genoma. Hay una serie de programas especializados en RNA seq, por ejemplo TopHat2, que parte del alineador Bowtie para posteriormente considerar los intrones en el proceso. 

#### 2.1 Indexar genoma de referencia

Es un proceso necesario para que Bowtie acceda de forma rápida al genoma de referencia. Para realizarlo introducimos el siguiente comenado:

```markdown
bowtie2-build -f Homo_sapiens.GRCh38.fa ihg38
```
Ello produce como salida archivos con extensión .bt2, que contienen las secuencias de referencia contra las que seran alineadas nuestros fragmentos. 

#### Alienamiento

Usamos el software TopHat2, que incluye una herramienta para la búsqueda de transcritos quiméricos ("--fusion-search"). Especificamos el genoma de referencia anotado (.gtf), el directorio donde TopHat2 guardará los resultados ("./aligment_output") y las lecturas de ambos sentidos (R1.fastq y R2.fastq).

```markdown
tophat -G ./hg38.gtf -o ./aligment_output ./ihg38 ./Sample1.R1.fastq ./Sample1.R2.fastq --fusion-search
```
### Filtrado de transcritos

Utilizamos el programa TopHat-fusion-post para filtrar los posibles transcritos quiméricos candidatos. Es aconsejable repetir este proceso con distintos parámetros de filtrado. Lanzamos este comando en el directorio de salida de TopHat2:

```markdown
tophat-fusion-post --num-fusion-reads 1 --num-fusion-pairs 2 --num-fusion-both 5 ../ihg38
```

another test

![image](/Unknown.jpeg)

### 2. Alignment (STAR)

### 3. Transcript filtering (STAR - fusion)



### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block
# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ibienzobas/ibienzobas.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
