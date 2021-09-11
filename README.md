# Descripción del análisis computacional de la producción de dibosón ZZ en el canal de desintegración 4l


## Introducción
El marco hace uso del lenguaje [C++](http://www.cplusplus.com/doc/tutorial/) y está interconectado con [ROOT](https://root.cern.ch/), y está disponible bajo este [Enlace de Github](https://github.com/atlas-outreach-data-tools/atlas-outreach-cpp-framework-13tev). Después de clonar / descargar el repositorio, lo único que necesita para configurar es: necesita tener el marco ROOT (vea [aquí](https://root.cern.ch/building-root#quick-start) para una start on ROOT) y un [compilador gcc](https://gcc.gnu.org/). La versión actual del marco se compiló utilizando gcc v6.20 y ROOT v6.10.04.

Los 13 TeV ATLAS Open Data están **alojados** en el [portal en línea CERN Open Data](http://opendata.atlas.cern/release/2020/documentation/datasets/files.html) y [ATLAS Open Data en línea portal](http://opendata.atlas.cern/). El marco puede acceder a las muestras de dos formas:
+ leerlos en línea directamente;
+ leerlos forman un almacenamiento local (las muestras deben descargarse localmente).

El marco consta de **dos partes principales**:

+ la parte **análisis**, ubicada dentro del directorio "Análisis": realiza la selección del objeto particular y almacena los histogramas de salida;
+ la parte **trazado**, ubicada dentro del directorio "Trazado": hace los diagramas de Datos / Predicción finales.

---
## La parte de análisis

El código de análisis se encuentra en la carpeta **Análisis** . El nombre del análisis a estudiar ZZDiBoson.

Cada subcarpeta de análisis contiene los siguientes archivos:

+ código principal de análisis (**ZZDiBosonAnalysis.C**): realiza toda la selección y almacena los histogramas de salida;
+ encabezado principal de análisis (**ZZDiBosonAnalysis.h**): define los histogramas y da acceso a las variables almacenadas en las muestras de entrada;
+ encabezado del histograma (**ZZDiBosonAnalysisHistograms.h**): define el nombre de los histogramas de salida;
+ código de control principal de análisis (**main_ZZDiBosonAnalysis.C**): controla qué muestras de entrada se van a utilizar y su ubicación;
+ un [script bash](https://www.shellscript.sh/) (**run.sh**), ejecutado a través de un shell de Linux / UNIX llamado [fuente](https://linuxize.com/post/ bash-source-command /): le ayuda a ejecutar el análisis de forma interactiva.
+ *en caso de que haya utilizado el script de welcome*, se creará el directorio de salida (**Output_ZZDiBosonAnalysis**): este es el lugar donde se mostrará la salida del código de análisis (*un archivo con histogramas por cada muestra de entrada*) almacenado. Advertencia: si el directorio de salida no existe, el código fallará, ¡cree siempre uno vacío!

### ¡Análisis práctico!

En el directorio principal, se realiza la primera configuración del código escribiendo en la terminal:
```
./welcome.sh 
```
o en caso de que haya instalado el shell de origen:
```
source welcome.sh 
```
Esto le preguntará si desea crear automáticamente los directorios de salida del análisis, o borrar su contenido en caso de que sea necesario.

Después de esto, se cambia a la carpeta de análisis y se abre con el editor de texto preferido el código de control principal de análisis (**main_ZZDiBosonAnalysis.C**):controla **la ubicación de las muestras de entrada**, por favor encuentra la línea:
``
// ruta a su directorio local * o * URL, ¡cambie la predeterminada!
TString path = "";
```
y se adapta adecuadamente al caso específico.

```
Después de eso, se ejecuta el código a través de la línea de comando usando:
```
./run.sh
```
or
```
source run.sh
```
El script le pedirá interactivamente **dos opciones** que puede escribir directamente (0, 1, ..) en la terminal y presionar "ENTER":

+ La **primera opción** le preguntará: ¿desea ejecutar *todas las muestras* una por una, o ejecutar *solo datos* o *solo muestras simuladas*? Las últimas opciones pueden ayudarlo a acelerar el análisis, ya que puede ejecutar varias muestras en varios terminales.
+ La **segunda opción** le preguntará: ¿desea utilizar [Parallel ROOT Facility](https://root.cern.ch/proof) (PROOF), una herramienta integrada en ROOT que permite el análisis de las muestras de entrada en *paralelo en una máquina de muchos núcleos*? Si su versión ROOT tiene PROOF integrado, acelerará el análisis en un *factor de aproximadamente 5*.

Después de elegir las opciones, el código compilará y creará las bibliotecas compartidas ROOT necesarias, y comenzará la selección del análisis: se ejecutará sobre cada muestra de entrada definida en **main_ZZDiBosonAnalysis.C**.

Si todo fue exitoso, el código creará en el directorio de salida (**Output_ZZDiBosonAnalysis**) un nuevo archivo con el nombre de la muestra correspondiente (data, ttbar, ...).


Para limpiar todas las bibliotecas compartidas y vinculadas después de la ejecución, se puede usar un script llamado * clean.sh * ubicado en el directorio principal.
---

## La parte de trazado
El código de trazado se encuentra en la carpeta **Trazado** y contiene los siguientes archivos:

+ trazado del código principal (**Plotting.cxx**): hace que toda la trama sea mágica y controla automáticamente qué hacer para el análisis;
+ Encabezado principal de trazado (**Plotting.h**): define todos los bits y piezas necesarios para el código de trazado;
+ directorio auxiliar (**list_histos**): dentro de el se encuentra el archivo de control con nombre **HistoList_ANALYSISNAME.txt**, cada uno de estos controles cuyos histogramas se van a utilizar y graficar para el análisis;
+ directorio auxiliar (**inputfiles**): dentro de se encuentra el archivo de control con nombre **Files_ANALYSISNAME.txt**, cuyas muestras de entrada exactamente se usarán para este análisis en particular, sus cruces sección y suma de pesos. ¡¡¡NO CAMBIES!!!
+ script bash (**plotme.sh**): ayuda a ejecutar el código de trazado de forma interactiva, ¡utilícelo!
+ un archivo MAKE (**Makefile**): un conjunto de directivas utilizadas por una herramienta de automatización de compilación [make] (https://www.gnu.org/software/make/manual/html_node/Introduction.html) para generar el salida ejecutable;
+ *en caso de que haya utilizado el script de welcome*, el directorio de salida (**histogramas**): ¡contendrá los gráficos de salida!

### Ploteo práctico!

En el directorio principal de Plotting, ejecute en la terminal:
```
./plotme.sh 
```
o en caso de que haya instalado el shell de origen:
```
source plotme.sh 
```

El script le pedirá interactivamente **dos opciones** que puede escribir directamente (0, 1, ..) en la terminal y presionar "ENTER":

+ La **primera opción** será: análisis a graficar
+ La **segunda opción** ubicación del directorio **Output_ZZDiBosonAnalysis** que se creó al ejecutar el código de Analysis.

Después de elegir las opciones, el código se compilará y creará las bibliotecas compartidas ROOT necesarias y comenzará el trazado. Si todo fue exitoso, el código creará en el directorio de salida (**histogramas**) los gráficos correspondientes definidos en **HistoList_ANALYSISNAME.txt**.

Para limpiar todas las bibliotecas compartidas y vinculadas después de la ejecución, puede usar un script llamado *clean.sh* ubicado en el directorio principal.

---
