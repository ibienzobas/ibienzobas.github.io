

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

another test

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
