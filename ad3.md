# AD3

Esta **actividad dirigida 3** consiste en un ejercicio de programación literaria aprovechando el código que hemos usado en la materia de programación con Python, dónde realizamos *web scrapping*.

Comenzaremos mencionando las librerías y módulos que utilizaremos en esta actividad:


## Módulos y librerías externas

**Los módulos** son ficheros que contienen definiciones y declaraciones de Python que implementan un conjunto de funciones. Al ser internos del sistema solo deben importarse. Por su parte, las **librerías externas** deben instalarse antes de poder importarse.

### Módulos internos
Estos son los módulos que utilizaremos:

1. [time](https://docs.python.org/es/3/library/time.html): Este módulo proporciona varias funciones relacionadas con el tiempo.
2. [csv](https://docs.python.org/3/library/csv.html): (Valores separados por comas) este módulo implementa clases para leer y escribir datos tabulares en formato CSV (Uno de los formatos favoritos de Excel).
3. [re](https://docs.python.org/3/library/re.html): Este módulo proporciona operaciones de coincidencia de expresiones regulares similares a las que se encuentran en el lenguaje Perl.
4. [os](https://docs.python.org/3/library/os.html): Proporciona una forma portátil de usar la funcionalidad dependiente del sistema operativo.



### Librerías externas 
Para este *web scraping* también implementaremos las siguientes librerías externas:

1. [requests](https://www.w3schools.com/python/module_requests.asp): permite enviar solicitudes HTTP utilizando Python.
2. [bs4](https://cppsecrets.com/users/15711159799104105110115104971071219756515164103109971051084699111109/Python-bs4-module.php#:~:text=Python%20%22bs4%22%20module%20Article%20Creation%20Date%20%3A%2022-Jun-2021,remove%20the%20HTML%20markup%2C%20and%20save%20the%20information):Hace referencia a Beautiful Soup, una biblioteca que facilita extraer información de páginas web. Es una herramienta de web scraping que nos ayuda a limpiar y analizar los documentos que hemos sacado de la web. 
3. [pandas](https://pandas.pydata.org/):es una herramienta rápida, potente y fácil de usar para el análisis y manipulación de datos de código abierto 
4. [termcolor](https://replit.com/talk/learn/How-to-Use-Termcolor-In-Python/24684): es un módulo de Python para el formato ANSII Color para la salida en el terminal.Permite imprimir texto coloreado y fondo.

## Instalación de Librerías

Instalaremos las librerías externas a través del siguiente código:


```python
pip install requests bs4 pandas termcolor
```

    Defaulting to user installation because normal site-packages is not writeable
    Requirement already satisfied: requests in d:\anaconda\lib\site-packages (2.27.1)
    Requirement already satisfied: bs4 in c:\users\madelaine guevara\appdata\roaming\python\python39\site-packages (0.0.1)
    Requirement already satisfied: pandas in d:\anaconda\lib\site-packages (1.4.2)
    Requirement already satisfied: termcolor in c:\users\madelaine guevara\appdata\roaming\python\python39\site-packages (1.1.0)
    Requirement already satisfied: charset-normalizer~=2.0.0 in d:\anaconda\lib\site-packages (from requests) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in d:\anaconda\lib\site-packages (from requests) (2021.10.8)
    Requirement already satisfied: idna<4,>=2.5 in d:\anaconda\lib\site-packages (from requests) (3.3)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in d:\anaconda\lib\site-packages (from requests) (1.26.9)
    Requirement already satisfied: beautifulsoup4 in d:\anaconda\lib\site-packages (from bs4) (4.11.1)
    Requirement already satisfied: python-dateutil>=2.8.1 in d:\anaconda\lib\site-packages (from pandas) (2.8.2)
    Requirement already satisfied: pytz>=2020.1 in d:\anaconda\lib\site-packages (from pandas) (2021.3)
    Requirement already satisfied: numpy>=1.18.5 in d:\anaconda\lib\site-packages (from pandas) (1.21.5)
    Requirement already satisfied: six>=1.5 in d:\anaconda\lib\site-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)
    Requirement already satisfied: soupsieve>1.2 in d:\anaconda\lib\site-packages (from beautifulsoup4->bs4) (2.3.1)
    Note: you may need to restart the kernel to use updated packages.
    

## Importación de módulos y librerías

Luego de la instalación, el siguiente paso es la importación. 

La instrucción `import` es la forma más común de invocar la maquinaria de importación. 

Cuando se quiere **descargar todo el módulo o librería**, se acompaña `import` con el nombre de la librería. 

Si solo se necesita **una sección determinada,** se usa la fórmula `from` (librería) `import` (atributo específico). 

Además, está la opción de hacer una **importación cambiándole el nombre al módulo por el que usaremos para llamarlo.** Lo último se haría de la siguiente manera: `import` (librería) `as` (nombre que usaremos) 

A continuación importamos las librerías y módulos según nuestras necesidades:



```python
import requests
import time
import csv
import re
from bs4 import BeautifulSoup
import os
import pandas as pd
from termcolor import colored 
```

## Web scraping

Luego de importadas las librerías, podremos empezar a *scrapear* la información que buscamos de la fuente.

### Objetos o variables

#### Resultados

Añadiremos un objeto vacío tipo lista para recopilar y organizar los **resultados** del *scraping*.  


```python
resultados = []
```


```python
type (resultados)
```




    list



### Invocar acceso a datos de la página web

Como buscamos hacer *web scrapping* de diferentes secciones del periódico [El País](https://elpais.com/),
comenzaremos con una solicitud para acceder a la información de ese URL mediante el siguiente código: 


```python
req = requests.get ("https://resultados.elpais.com")
```

### Condicional de código de estado

Seguido se emplea un condicional `if` para que la respuesta dependa de los códigos de estado (*status codes*).

El status **200** significa que la solicitud fue exitosa, mientras que el **404** significa que no se encontró el recurso que estaba buscando.

Con este `if` se pide que cuando el status no sea 200, el resultado no pueda leerse y se levante una excepción que muestre el mensaje ***("No se puede hacer Web Scraping en"+ URL)*** 


```python
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
```

### Beautiful Soup

Luego se agrega la librería `BeautifulSoup` con los argumentos:
- `req.text` (para obtener el texto que hemos solicitado)
- `html.parser` (para asegurar que se utilice el *parser* adecuado para el contenido HTML)

El resultado se almacena en una variable llamada `soup`



```python
soup = BeautifulSoup (req.text, 'html.parser')
```

Después se agrega una variable `tag` para encontrar todas las etiquetas `h2` dentro de la página. 

Se le pide además, que imprima estos `h2` y los añada en la lista inicial de `resultados` 


```python
tags = soup.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    Jamie Dimon, el banquero más poderoso: “Los problemas económicos no son pasajeros. Las cosas pueden ir mucho peor”
    Kamala Harris asume que fue un error dar por sentado el derecho al aborto en EE UU
    Ocampo: “Para crecer hay que trabajar mano a mano con los ricos en Colombia”
    Luis Echeverría, el gran represor de México muere impune a los 100 años
    Remite el riesgo de estanflación
    Los frentes abiertos de Castillo agudizan la inestabilidad de Perú
    El “momento Churchill”
    Operación salida… de los cuerpos amantes y gozosos
    Putin intentará estrangular a la Unión Europea
    Mi bebé ya entiende de política
    Mar Castellanos, neuróloga: “El ictus ya no es una enfermedad de gente mayor”
    Los próximos pasos de la revolución conservadora del Supremo de EE UU
    La Columna Armada: una historia de narcos, autodefensas y caciques en Tamaulipas
    Más de 4.000 dólares para compensar una negligencia médica que costó perder el útero y la amputación de las piernas
    Menores que migran solos: una tragedia que crece en las fronteras colombianas
    Las protestas masivas por la crisis económica fuerzan la dimisión del presidente y del primer ministro en Sri Lanka
    Las activistas que dejaron sin anuncios a la web de Steve Bannon van ahora a por Fox News
    Mônica Benício: “Quiero que Lula sea presidente, pero quiero que haga políticas feministas y para la población LGTBI”
    Los militares de Canadá se abren a un código más inclusivo: tatuajes en el rostro y uniformes sin distinción de género 
    La muerte a tiros de Shinzo Abe conmociona a un país poco acostumbrado al uso de armamento
    Un magnicidio inusual que hace sentir a los japoneses vulnerables 
    Vídeo | ¿Por qué el atentado contra el ex primer ministro es algo excepcional en Japón?
    El detenido por el asesinato: desempleado y antiguo miembro de las fuerzas armadas
    El exministro de Economía Rishi Sunak lanza su candidatura a liderar el Partido Conservador
    José Eduardo dos Santos, de la guerrilla comunista al clan de los gobernantes millonarios
    Especial | Secuestro y asesinato de Miguel Ángel Blanco: cuando ETA perdió la calle
    Sudáfrica se rebela contra el alcoholismo juvenil tras la muerte de 21 adolescentes en un bar
    Cuatro agentes de la Patrulla Fronteriza enfrentarán procesos disciplinarios tras el maltrato a inmigrantes haitianos en Texas
    Duchas cortas y límite a las calefacciones: Alemania se prepara para posibles cortes de energía
    La fuga de Siberia de Trotsky, la revelación de Marta D. Riezu y otros libros de la semana
    Músicas ocultas
    Jardines secretos en la jungla de asfalto: cómo resistir a la especulación plantando un árbol
    El desvío experimental de Perfume Genius, los salmos de Nick Cave y otros discos 
    La banalidad del drama: la literatura que surge de los grandes juicios históricos
    Muere a los 79 años Tony Sirico, Paulie en ‘Los Soprano’
    Un ‘chéjov’ turbulento dirigido por un disidente de Putin
    Menos mal que quedan los libros
    Róisín Murphy: “Cuando salgo al escenario, lo hago como sale una bala”
    10 veces que las ‘celebrities’ se vistieron de novia en la alfombra roja
    Elliot Page: cuando el insulto es un ‘necronombre’
    La demanda femenina de zapatillas que solo encuentra oferta masculina
    Recorra Madrid de la mano de ‘La casa de papel’ 
    ¿Quién es quién en la familia real de Mónaco? Un repaso a los Grimaldi
    Rybakina triunfa y cuela a Rusia en Wimbledon
    Verstappen no da opción a Ferrari y se lleva la ‘pole’ en Austria  
    El Tour de Francia de Van Aert y Pogacar
    El 9 de julio el Tour de Francia celebra San Federico Martín Bahamontes 
    ¿Por qué es infinito?
    EL PAÍS América gana el Premio de la Asociación Mundial de Editores al mejor sitio de noticias de Latinoamérica
    Vender a una niña por comida: “Hubo pretendientes e intenté escoger al más joven”
    Francisco de Roux: “Si no se acaba con el narco, Colombia no tendrá paz”
    La economía mundial se desinfla: así es la crisis que viene
    Japan’s ex-PM Shinzo Abe dies in hospital after being shot by gunman
    An ex-Marine processes war trauma, uncovers memories of abuse at a Catholic school in Spain
    ‘Meraxes gigas’: New species of giant dinosaur discovered in Argentina 
    Fàtima Crispi: “It is assumed that the pregnant woman is a happy woman… but the stress of pregnancy is closely related to the fear of the unknown.”
    ‘One has to go’: How Chris Pratt became the least liked of all of Hollywood’s actors named Chris
    Letras Americanas: la actualidad literaria de un continente vista por el escritor Emiliano Monge
    Americanas: Reportajes y noticias sobre feminismo e historias con enfoque de género de la región
    Toda la actualidad científica en el boletín de Materia
    Ideas: reportajes y entrevistas para entender el mundo
    El agente que mató a George Floyd, condenado a otros 21 años de cárcel por violar sus derechos civiles
    Haití, sin presidente y sin Estado un año después del asesinato de Jovenel Moïse
    Petro nombra a Alejandro Gaviria en Educación y ahonda en su perfil de moderado
    Inflación, energía, reservas y tipo de cambio: los desafíos que lastran a la economía Argentina en el segundo semestre
    Los argentinos se lanzan a las tiendas ante una nueva crisis del peso: “Si ves algo, compra”
    Uniper, la mayor gasista alemana, pide el rescate asfixiada por los recortes del gas ruso
    Marcos López Hoyos, inmunólogo: “Deberíamos usar mascarillas en interiores y comer en terrazas”
    La falta de presupuesto ahoga a los refugios para mujeres víctimas de violencia en México
    ¿Existe brecha generacional en el colectivo LGTBI? De la lucha por los derechos al debate identitario
    Alivio para el arte conceptual: la justicia francesa considera que el autor de una obra es el artista que la concibe y no el que la ejecuta
    ‘Mali Twist’: Y el rock también llegó a Malí
    Muere el actor James Caan, célebre por encarnar a Sonny Corleone en ‘El padrino’, a los 82 años
    ¿Es la consciencia una ilusión intrascendente?
    Hallada en Atapuerca la cara del humano más antiguo de Europa
    Rosaly Lopes, la persona que más volcanes ha descubierto: “Puede haber vida en Titán” 
    España debuta con cabeza en la Eurocopa
    Hugo Koblet, el James Dean del ciclismo
    Annemiek Van Vleuten, número uno del ciclismo femenino mundial: “Es difícil decir adiós”
    La sobrexplotación amenaza a las especies silvestres de las que dependen miles de millones de personas en el mundo
    ‘Mi familia de ardillas’, por Jacinto Antón
    Adiós al Aquarama del zoo de Barcelona, el hogar de la orca ‘Ulises’
    El misterio de la carta del soldado alemán
    ‘Por qué los proyectos ‘cripto’ son tan difíciles de entender’, por Jordi Pérez Colomé
    Jorge Stolfi: “Soy catedrático de informática. Como mis colegas, sé que la tecnología de bitcoin es basura”
    Laura Borràs exhibe apoyos para continuar en un homenaje sin representantes del Govern
    Las discrepancias entre PP y PSOE enturbian el homenaje a Miguel Ángel Blanco 
    Pablo Iglesias, sobre los audios de Villarejo: “Toda esta ‘basura’ ha alimentado el relato mediático sobre Podemos durante años”
    ‘Encerrado con el diablo’: Dennis Lehane busca en el fondo del alma
    ‘Sanfermines: maltrato animal en riguroso directo’, por Eva Güimil
    ‘La noche más larga’ y ‘La lista final’: palomitas sin sabor para el verano
    El espectáculo (teatral) debe continuar en la alta costura de París
    Por qué es importante conocer tu personalidad erótica para tener una vida sexual más satisfactoria
    Elon Musk tuvo gemelos en secreto con una ejecutiva de una de sus empresas
    El Panamá indígena busca empoderar a sus mujeres
    Volver a la escuela tras dos años de pandemia
    ¿Dónde está exactamente el origen del mal?
    El hombre que persigue la gaita más antigua del mundo
    La tiranía de la vida eficiente: ¿alguien es capaz de no hacer nada? 
    Carlos Nobre: “La Amazonia  ya muestra síntomas  de muerte”
    Manual para sobrevivir al calentón del euríbor
    La carrera por las oposiciones ha empezado: en juego hay 45.000 trabajos para toda la vida
    Ayn Rand, ‘rednecks’ e inclusividad forzada: el mundo virtual como escenario político
    Hay un río de oro negro bajo A Coruña
    Un continente donde los sueños tienen menos de 30 años
    Toca devolver lo prestado: la crisis de deuda asfixia el desarrollo en África
    Siete calas del Mediterráneo dignas de culto
    Una noche en pleno delta del Okavango
    De Lady Gaga a Meghan Markle: 10 veces que las ‘celebrities’ se vistieron de novia porque sí en una alfombra roja
    El ‘influencer’ más varguardista del momento crea parodias de la moda desde una de las islas más pobres del mundo
    A-ha: cómo tres tipos que no se soportan sobrevivieron a la canción pop más grande de los ochenta 
    Una granja llena de armas y dos denuncias por secuestro: ¿cómo ha llegado Ezra Miller hasta aquí?
    La receta de Ladilla Rusa: ensaladilla rusa
    Aló Comidista: “¿Tomar gazpacho es un pecado nutricional?”
    Las oficinas del futuro: espacios híbridos con tecnología integrada para rendir más (y mejor)
    Abre la primera casa sin cocina para los enfermos que no pueden parar de comer: así es el síndrome de Prader Willi
    Ponte a prueba con nuestros crucigramas, sopas de letras, sudokus y juegos arcade
    Toni Cantó, directo al ‘trending topic’ por lo que ha dicho al hablar de su trabajo
    Scariolo: “Es el momento más difícil de la Selección en los últimos 15 años”
    Después del Homo Sapiens: ¿dioses o tiranos?
    

## Repetición del proceso para secciones del periódico.

El proceso anterior se repite para obtener información de las siguientes secciones:

- [El País - Internacional](https://elpais.com/internacional)
- [El País - Opinión](https://elpais.com/opinion)
- [El País - España](https://elpais.com/espana)
- [El País - Economía](https://elpais.com/economia)
- [El País - Sociedad](https://elpais.com/sociedad)
- [El País - Educación](https://elpais.com/educacion)
- [El País - Clima y Medio Ambiente](https://elpais.com/clima-y-medio-ambiente)
- [El País - Ciencia](https://elpais.com/ciencia)
- [El País - Cultura](https://elpais.com/cultura)
- [El País - Babelia](https://elpais.com/babelia)
- [El País - Deportes](https://elpais.com/deportes)
- [El País - Tecnología](https://elpais.com/tecnologia)
- [El País - Gente](https://elpais.com/gente)
- [El País - Televisión](https://elpais.com/televisión)
- [El País - Eps](https://elpais.com/eps)

**Lo único que se modifica es que se enumeran las variables `req` siguiendo con `req2` y así sucesivamente**


```python
req2 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup2 = BeautifulSoup(req2.text, 'html.parser')
 
tags = soup2.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req3 = requests.get("https://elpais.com/opinion")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup3 = BeautifulSoup(req3.text, 'html.parser')
 
tags = soup3.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req4 = requests.get("https://elpais.com/espana/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup4 = BeautifulSoup(req4.text, 'html.parser')
 
tags = soup4.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req5 = requests.get("https://elpais.com/economia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup5 = BeautifulSoup(req5.text, 'html.parser')
 
tags = soup5.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req6 = requests.get("https://elpais.com/sociedad/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup6 = BeautifulSoup(req6.text, 'html.parser')
 
tags = soup6.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req7 = requests.get("https://elpais.com/educacion/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup7 = BeautifulSoup(req7.text, 'html.parser')
 
tags = soup7.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req8 = requests.get("https://elpais.com/clima-y-medio-ambiente/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup8 = BeautifulSoup(req8.text, 'html.parser')
 
tags = soup8.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req9 = requests.get("https://elpais.com/ciencia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup9 = BeautifulSoup(req9.text, 'html.parser')
 
tags = soup9.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req10 = requests.get("https://elpais.com/cultura/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup10 = BeautifulSoup(req10.text, 'html.parser')
 
tags = soup10.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req11 = requests.get("https://elpais.com/babelia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup11 = BeautifulSoup(req11.text, 'html.parser')
 
tags = soup11.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req12 = requests.get("https://elpais.com/deportes/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup12 = BeautifulSoup(req12.text, 'html.parser')
 
tags = soup12.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req13 = requests.get("https://elpais.com/tecnologia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup13 = BeautifulSoup(req13.text, 'html.parser')
 
tags = soup13.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req14 = requests.get("https://elpais.com/tecnologia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup14 = BeautifulSoup(req14.text, 'html.parser')
 
tags = soup14.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req15 = requests.get("https://elpais.com/gente/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup15 = BeautifulSoup(req15.text, 'html.parser')
 
tags = soup15.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req16 = requests.get("https://elpais.com/television/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup16 = BeautifulSoup(req16.text, 'html.parser')
 
tags = soup16.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req17 = requests.get("https://elpais.com/eps/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup17 = BeautifulSoup(req17.text, 'html.parser')
 
tags = soup17.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    El exministro de Economía Rishi Sunak lanza su candidatura a liderar el Partido Conservador británico
    La policía japonesa admite fallos en la seguridad de Shinzo Abe
    Las protestas masivas por la crisis económica fuerzan la dimisión del presidente y del primer ministro en Sri Lanka
    Un magnicidio inusual que hace sentir a los japoneses vulnerables 
    Populistas occidentales por el desagüe de la historia
    Boris Johnson dimite
    La ultraderecha mundial y el control de los cuerpos
    Las otras fronteras de Rusia: entre el miedo (al imperio) y los intereses nacionales
    Duchas cortas y límite a las calefacciones: Alemania se prepara para posibles cortes de energía este invierno
    La muerte a tiros de Shinzo Abe conmociona a un país poco acostumbrado al uso de armamento
    Shinzo Abe, un primer ministro carismático que marcó el rumbo de la política en Japón
    Un magnicidio inusual que hace sentir a los japoneses vulnerables 
    Shinzo Abe no es el primero: historia reciente de los magnicidios
    El detenido por la muerte de Abe: desempleado y antiguo miembro de las fuerzas armadas
    Ucrania rinde homenaje a Boris Johnson
    La revolución conservadora del Supremo de Estados Unidos está lista para su segundo asalto
    Las voces de los niños de la guerra: “Los colegios se volvieron salas fúnebres”
    “Hay una parte de la verdad de Colombia que solo se puede conocer fuera de Colombia”
    Detenido en Brasil el presunto autor intelectual del asesinato de Dom Philips y Bruno Pereira en la Amazonia
    Kirchner acusa al ministro de Economía saliente de “desestabilización institucional”
    Detenido en Brasil el presunto autor intelectual del asesinato de Dom Philips y Bruno Pereira en la Amazonia
    Nadie frena la novena reelección de un eterno sindicalista argentino
    “Hay una parte de la verdad de Colombia que solo se puede conocer fuera de Colombia”
    Trump y Biden miden ya sus fuerzas para las elecciones de 2024
    Orgullo y resiliencia
    Guerra sucia contra Podemos
    Financiación autonómica: el avestruz y los populismos
    En el camino a Santiago
    ‘Mi casa, mi árbol’
    Los escritores, la Europa de los derechos y el mercado único digital
    Operación salida… de los cuerpos amantes y gozosos
    Putin intentará estrangular a la UE
    De sumar, de restar y de Yolanda Díaz
    La vida sigue después del Bataclan
    Mi bebé ya entiende de política
    Un historiador
    El legado de Boris Johnson
    El Roto
    Peridis
    Flavita Banana
    Riki Blanco
    Sciammarella
    Planeta de Plástico, por Malagón
    Envía tu carta
    Médicos en España
    Lo raro es vivir
    Defender lo público
    En la calle circulan rumores
    Opinar sin insultar
    Contra el conflicto de intereses, transparencia
    El defensor del lector contesta
    Yolanda Díaz presenta Sumar como movimiento ciudadano que busca “un nuevo contrato social”
    Una jubilada, una sindicalista, un trabajador joven y otra de mayor edad: razones para Sumar
    Feijóo, en el homenaje a Miguel Ángel Blanco: “Prometo derogar la ley de memoria democrática”
    Ante la crisis, ¿gestionas o lideras?
    El almirante Cervera y el honor de España
    El Gran Retroceso
    Santa Bárbara y la cultura de la defensa
    ¿Es posible disentir de esta retórica belicista?
    Irene Montero muestra el apoyo de Podemos a Sumar: “Yolanda es nuestra candidata” 
    La entrada de agentes marroquíes en Melilla despierta dudas legales entre los expertos
    El Gobierno y la Generalitat  se comprometen a retomar el diálogo y “superar la judicialización”
    El juez cita como imputados a tres exjefes de ETA por el asesinato de Miguel Ángel Blanco
    Interior estudia incentivos a los guardias civiles de zonas rurales para evitar el cierre de cuarteles
    Marlaska elogia a Marruecos y su trabajo de “contención” de la inmigración tras la muerte de 23 subsaharianos en Melilla 
    El Defensor del Pueblo abre una investigación sobre el incendio en la sierra de la Culebra
    Feijóo considera “discutible” dedicar dinero público a becas para familias de rentas altas como hace Ayuso
    Un juez archiva la causa contra el exmagistrado del Constitucional Fernando Valdés por supuesto maltrato 
    La Audiencia justifica la salida del magistrado Delgado del juicio a Camps “para alejar cualquier sospecha de falta de imparcialidad”
    La Audiencia Provincial de Palma decide continuar con el juicio a Cursach tras desestimar buena parte de las peticiones de las defensas
    Un paraíso en el que avistar más de 20 especies de cetáceos
    El secreto de Fuerteventura se hace viral: la ‘playa de las palomitas’
    Un vino para triunfar entre los mileniales
    Artesanía y cultura que conquista a las ‘influencers’
    Más allá de la capital. El triunfo de la vida rural madrileña
    Jamie Dimon, el banquero más poderoso del mundo: “Los problemas económicos no son pasajeros. Las cosas pueden ir mucho peor”
    Elon Musk retira la oferta de compra por Twitter
    EE UU se consolida como primer proveedor de gas a España, en detrimento de Argelia
    Vecinos al borde de un ataque de nervios: morosos que usan la piscina, trasteros que son despensas y ruidos diarios
    Remite el riesgo de estanflación
    Bancos centrales, entre líneas
    Estado autonómico: Sobre las reformas pendientes
    Carlos Jiménez Villarejo: un hombre contra la desesperanza
    IPC: de la negación al riesgo de sobrerreacción
    Los españoles fueron los europeos que más pagaron por la luz entre enero y marzo
    José Antonio Ocampo: “Para crecer tenemos que trabajar mano a mano con las personas de altos ingresos, con los más ricos”
    CaixaBank advierte de que la subida de los intereses “aumenta el riesgo” ante burbujas inmobiliarias
    El ‘efecto Draghi’ pierde parte de su magia
    Así está la negociación sobre el pacto de rentas: un diagnóstico común, soluciones enfrentadas
    Remite el riesgo de estanflación
    El BCE cifra en 70.000 millones las pérdidas de la gran banca ante una crisis climática
    Ya se puede pedir el cheque ‘antinflación’ de 200 euros: ¿quién tiene derecho? ¿Dónde solicitarlo?
    Las constructoras acusan a la CNMC de “falta de rigor” por la macromulta de 204 millones
    Francisco González pide volver a declarar como imputado en el ‘caso Villarejo’
    Uniper, la mayor gasista alemana, pide el rescate asfixiada por los recortes del gas ruso
    La carrera por las oposiciones ha empezado: en juego hay 45.000 trabajos para toda la vida
    Los jueces se ponen de parte de las víctimas de ‘phishing’ bancario
    Texas es la nueva China para  Elon Musk
    IPC: de la negación al riesgo de sobrerreacción
    A Unilever se le indigesta el helado 
    El mercado de la vivienda ante la subida de tipos
    Los hogares frenan su gasto en transporte y ocio y aumentan el de los alimentos
    
    Arnaud Ribault (Citroën): “Los concesionarios de la marca no están enfadados con el cambio de contrato”
    ¿Qué pasa con las obras de arte que nadie quiere comprar?
    No se puede desheredar a un familiar con el que no se tiene relación, según el Supremo
    Líos en la piscina comunitaria: normas y sentencias para un chapuzón seguro
    Parir en casa, educarlo por mi cuenta… ¿Qué puedo decidir sobre mi hijo y qué no?
    Música que transforma vidas y esboza futuros
    El futuro probable de una sociedad insostenible
    Francisco Javier Blasco: “Llevamos años sin mejorar la eficacia de las políticas activas de empleo”
    La hora del bienestar corporativo
    Cancerberos del bienestar de la empresa
    Cómo garantizar la seguridad en un mundo de amenazas híbridas
    ¿Cuáles son los dilemas éticos del uso de la inteligencia artificial?
    Las aventuras de un par de calcetines que dan empleo a todo un pueblo
    Cultura financiera como punto de partida para volver a empezar
    ¿Puede convertirse España en una de las potencias logísticas del futuro?
    Por qué digitalizar una pyme aporta más empleo
    ‘Coopetir’: la actitud DFactory
    Así serán las ciudades de la (nueva) última milla
    Cinco errores habituales al digitalizar un negocio 
    PERTE Agroalimentario: ¿cómo acceder a los fondos europeos?
    Fondos soberanos: ¿Cómo funcionan las cajas fuertes de los estados más ricos?
    ¿Crisis de deuda mundial a la vista?
    El método Muñoz para triunfar en cada reto que se propone
    Gloria Ramos, cuando el afán de superación acaba en la alfombra roja 
    Ocho ‘startups’ innovadoras con nombre propio
    Hotmart y su plataforma 360 para creadores de contenidos
    Ángel Gabilondo, sobre su investigación de la pederastia: “Hay gente trabajando activamente para que esto no salga adelante”
    Radiografía de la séptima ola de covid: el virus comienza a perder impulso
    Mônica Benício: “Quiero que Lula sea presidente, pero quiero que haga políticas feministas y para la población LGTBI”
    El control del cuerpo de las mujeres
    No se gobierna con el catecismo romano
    Un día muy triste para todas las mujeres
    El Supremo de Estados Unidos destruye un derecho fundamental de las mujeres
    Un paso atrás, dos adelante: el avance de los derechos LGTBI en la antigua Yugoslavia
    El responsable LGTBI del PSOE celebra que Sánchez apartase a Calvo por su oposición a la ‘ley trans’
    Biden reacciona a las críticas sobre su inacción con un decreto para proteger el aborto
    El Papa asegura que el camino que ha seguido la Iglesia contra la pederastia “es irreversible”
    La sobrexplotación amenaza a las especies silvestres de las que dependen miles de millones de personas en el mundo
    Los ricos esperan menos para operarse, también en los hospitales públicos
    El 60% de los universitarios no se ve preparado para un mercado laboral que no encuentra los perfiles necesarios  
    El Constitucional declara por primera vez que toda discriminación de las personas trans es ilegal
    La ministra de Ciencia e Innovación: “El gran peligro es el negacionismo climático”
    Las emisiones de efecto invernadero volvieron a crecer en 2021 en España aunque sin sobrepasar los niveles previos a la pandemia
    ¿Existe brecha generacional en el colectivo LGTBI? De la lucha por los derechos al debate identitario
    Se buscan profesionales en economía circular
    La economía circular llega a la ciudad. Te contamos dónde se encuentra 
    ¿Por qué hablar de VIH en el Orgullo y no en los sanfermines?
    Mitos, falsedades, bulos y realidades sobre el VIH 40 años después
    Qué son las evidencias científicas y por qué están revolucionando la educación
    Munic, el joven redimido por el trap
    El tremendo peso de la obesidad en España
    Interactivo | Los 14 cambios en los aeropuertos españoles que no ves y que ya están ocurriendo
    Encontrar empleo pese a los obstáculos. Por dónde empezar la búsqueda
    La fórmula de Carboneros para evitar el éxodo rural: trabajar cuidando a los vecinos 
    Consejos y cuidados para el primer verano con un gatito o un perrito
    ¿Qué tienen en común perros y gatos cuando son cachorros?
    La guía definitiva para alimentarnos mejor (y cuidar nuestra salud y la del planeta)
    El metro como refugio. De los andenes de Madrid en 1936 a los de Kiev en 2022
    La salud mental de los refugiados: cómo abrirse para cerrar las heridas
    Yogures con menos azúcar, paso firme hacia la alimentación infantil del futuro
    Conectada con el mercado laboral y tecnológica: así es la universidad del presente
    Así es la formación más abierta y flexible, a prueba de crisis
    El 60% del dinero destinado a becas educativas en Madrid será para los centros privados
    El 60% de los universitarios no se ve preparado para un mercado laboral que no encuentra los perfiles necesarios  
    ¿Cómo se cierra un colegio público?
    Los sindicatos educativos catalanes convocan huelga el 7 de septiembre, primer día de curso en los institutos, y el 28
    ¿Hay que obligar a los niños a hacer deberes en verano? Los expertos recomiendan actividades que no se parezcan a las tareas escolares
    El caso de Georgia, en EE UU: becar sin importar la renta agranda la desigualdad
    Madrid, el paraíso de la educación privada: en la capital son minoría los alumnos de la pública
    Trabajar en el ‘big data’: “Nunca he tenido que echar currículum, las ofertas me han llegado solas”
    El Gobierno lanza una ofensiva contra las becas para rentas altas de Ayuso que Feijóo respalda
    José Manuel Bar, secretario de Estado de Educación: “Los docentes de secundaria deben empezar a estudiar pedagogía en su carrera”
    Ayuso se apunta ahora el tanto de la bajada de tasas universitarias, pese a que las llevó a los tribunales para no reducirlas
    Sin currículos
    La escuela concertada ante las desigualdades: el debate pendiente
    La equidad frente a las políticas educativas de privatización en Andalucía
    No hay lunes al sol en la Universidad
    El nuevo sistema para evaluar los conocimientos digitales de los profesores valdrá en toda España
    Ofrecer comedor gratis en todos los colegios públicos es “alcanzable y urgente”: costaría 1.664 millones al año, según la ONG Educo  
    Una fórmula para que la escuela pública compita mejor con la concertada
    La pérdida de alumnos por el descenso de la natalidad está afectando con más fuerza a la escuela pública que a la concertada
    El caso de Georgia, en EE UU: becar sin importar la renta agranda la desigualdad
    El techo de cristal del grado medio de FP: candidatos demasiado preparados se quedan con los puestos
    La implantación del nuevo Bachillerato general fracasa pese a su demanda potencial: “Creímos que lo pedirían seis alumnos y lo han hecho 27”
    Toni Solano, director de instituto: “Veo mal a los niños, necesitan muchísima ayuda”
    Niños, Administraciones y redes sociales: prohibir su uso con una mano y enseñar a crear contenidos con la otra
    Una historia en la que caben Alaska, García Lorca, Suárez y Popper: la Universidad Internacional Menéndez Pelayo cumple 90 años 
    Subirats reinterpreta la ley de Universidades: freno a la precariedad, facilidades para los alumnos extranjeros y ciencia abierta
    Las universitarias desertan del grado de Matemáticas ahora que tiene pleno empleo. ¿Por qué?
    Golpe a la temporalidad en las universidades: 25.000 profesores asociados serán indefinidos a tiempo parcial
    “No hay pensamiento sin tiempo para pensar”
    “En el hospital, la enseñanza es una medicina más”
    Un alegato por la paz y la cultura
    La sobrexplotación amenaza a las especies silvestres de las que dependen miles de millones de personas en el mundo
    Segunda ola calor del verano: al menos cuatro días con máximas de hasta 46° y noches tórridas a más de 25°
    La emergencia climática llama a la puerta del mundo
    Las emisiones de efecto invernadero volvieron a crecer en 2021 en España aunque sin sobrepasar los niveles previos a la pandemia
    Jornada del foro Ecosistema Ahora, organizado por EL PAÍS, en imágenes
    La oceanógrafa y buceadora Gádor Muntaner: “Lo que más miedo me da es nadar en un mar sin tiburones”
    Los rinocerontes regresan a Mozambique 40 años después de su desaparición
    Carlos Nobre: “La Amazonia  ya muestra síntomas  de muerte”
    El Parlamento Europeo respalda el sello verde de la UE al gas y energía nuclear
    Un alud letal y una histórica sequía: el cambio climático castiga al norte de Italia
    Un año de espera por la bicicleta: el estallido de la demanda coincide con la escasez de suministro
    Un plan de retirada  
    Crisis energética y precios del carbono
    La lección del martín pescador para afrontar la crisis ecológica
    Estocolmo+50: mirar atrás para tomar impulso
    Ríos imposibles: las 171.000 barreras que rompen el curso de agua en España
    La UE abraza las renovables para romper la dependencia de Rusia
    La lucha en un pueblo de Teruel para salvar su última montaña
    ¿Qué aire respiran los niños de Madrid y Barcelona? En el 46% de los colegios se supera la contaminación permitida
    “No hay pensamiento sin tiempo para pensar”
    “En el hospital, la enseñanza es una medicina más”
    Un alegato por la paz y la cultura
    Mar Castellanos, neuróloga: “El ictus afecta cada vez más a personas en edad laboral. Las enfermedades cardiovasculares son una epidemia”
    ¿Es la consciencia una ilusión intrascendente?
    Óvulos que se agotan y declive del esperma: todo lo que ignoramos sobre fertilidad hasta el momento de querer hijos
    Hallada en Atapuerca la cara del humano más antiguo de Europa
    El humano más antiguo de Europa: prehistoria que hace historia
    Rosaly Lopes, la persona que más volcanes ha descubierto: “Puede haber vida en Titán” 
    Una cena conflictiva
    ‘Meraxes gigas’: descubierta en la Patagonia argentina una nueva especie de dinosaurio carnívoro gigante
    La cola vestigial, el apéndice oculto del hombre que venció a Napoleón en Waterloo
    La agenda de la NASA para volver a la Luna (y quedarse)
    El sector biotecnológico supera las cifras de 2020 y capta 180 millones de euros de inversión privada
    “La identidad de género no se elige, como prueba el fracaso de las terapias de conversión”
    Un novedoso estudio sobre estrés adolescente propone un nuevo método para afrontarlo: optimizarlo en lugar de evitarlo
    Las mentes humanas detrás de las mentes artificiales
    El cerebro está más caliente de lo que se pensaba 
    Cangrejos ermitaños: el arte de seleccionar la mejor concha
    ¿Dónde hay civilizaciones extraterrestres?
    Las mentes humanas detrás de las mentes artificiales
    Matemáticas para describir los remolinos, los taxis del océano
    Reaparece la tesis de María Wonenburger, la pionera matemática española que permaneció décadas en el olvido
    Técnicas criptográficas que se fundamentan en lo impredecible
    Alrededor de la mesa
    Fracciones egipcias
    El problema del huerto
    La cola vestigial, el apéndice oculto del hombre que venció a Napoleón en Waterloo
    Molinos y gigantes, adelantos tecnológicos y el síndrome bicicleta
    El síndrome del hombre lobo y la realidad lógica de su fábula
    La locura como juego; más allá del Quijote y de las novelas de Pérez Galdós
    El andar del borracho, las huellas del azar y el juego de dados en algunos libros malditos
    ¿Son mejores las hormonas bioidénticas?
    ¿Qué surgió antes, el ARN o el ADN?
    ¿Por qué se sabe que los núcleos de la Tierra rotan?
    ¿Qué son los mini reactores nucleares?
    Invitados indeseables por Navidad: el muérdago y otras plagas que evitar durante las fiestas
    Las ‘apps’ nutricionales o cómo comer bien no debería depender de uno mismo
    Malnutrición invisible: el impacto de la pobreza en la salud infantil
    El óxido de etileno, la sustancia cancerígena que ha obligado a retirar miles de alimentos en la UE
    Que no te líen con los ingredientes: aceites y grasas de mala calidad nutricional
    Muse monta un circo en Mad Cool y Uber irrita con sus precios 
    The Killers y Stromae triunfan en una noche marcada por la música electrónica y bailable en el Bilbao BBK Live
    Muere a los 79 años Tony Sirico, Paulie en ‘Los Soprano’
    Cartografía de las desventuras húmedas
    Lo que debemos a Patxo Unzueta
    La huida
    Un siglo de canciones, 1.500 artistas
    La juerga veraniega de Rigoberta Bandini en el festival Cruïlla
    El festival de Aviñón arranca con un ‘chéjov’ turbulento dirigido por un disidente de Putin
    La banalidad del drama: la literatura que surge de los grandes juicios históricos
    Alivio para el arte conceptual: la justicia francesa considera que el autor de una obra es el artista que la concibe y no el que la ejecuta
    ¿Cómo es un festival intergeneracional en 2022?
    ‘Thor: Love and Thunder’, cuarta aventura individual del superhéroe de Marvel, y otros estrenos de este fin de semana
    El cine de propaganda nunca ha desaparecido: ahora se lanzan a él chinos y rusos
    Apoteosis de The Killers en Mad Cool ante 70.000 personas
    #Brahms125 aporta pasión y consuelo en las cálidas noches granadinas
    La Comunidad de Madrid retira una obra de Paco Bezerra prevista por los Teatros del Canal alegando razones económicas
    Estopa, Creu de Sant Jordi desde la periferia de la periferia
    ‘Mali Twist’: Y el rock también llegó a Malí
    Muere el actor James Caan, célebre por encarnar a Sonny Corleone en ‘El padrino’, a los 82 años
    Cuándo un gallego te dice que sí, aunque esté pensando que no. La retranca gallega y otros misterios que descubrir en el Camino Inglés
    El último atardecer de Europa se ve solo desde este lugar. Un viaje hacia el infinito por el Camino de Fisterra-Muxía
    Lorca, en la ciudad al margen
    El Pirineo oscense, para entrar a vivir
    Toda la lectura del verano, en el bolsillo
    Longsellers: los libros más vendidos a lo largo de la historia
    Libros para descubrir el mundo recomendados por Benjamín Prado
    Encuentro virtual con la cantante Carla Morrison
    Ciclo MET VERANO con Cine Yelmo
    'KLIMT. La experiencia inmersiva'
    'Padre no hay más que uno 3'
    Tan guapos como imposibles
    El tercer encierro de San Fermín 2022, en imágenes
    Dos heridos por cornada en un multitudinario y vistoso tercer encierro de San Fermín con los toros de Escolar
    Más allá del frenesí de los sanfermines
    Tristes, drogados, periféricos: la nueva literatura que refleja el dolor de los polígonos
    La fuga de Siberia de Trotsky, la revelación de Marta D. Riezu y otros libros de la semana
    El desvío experimental de Perfume Genius, los salmos de Nick Cave y otros discos del mes
    Jardines secretos en la jungla de asfalto: cómo resistir a la especulación plantando un árbol
    Los nuevos diaristas son vendedores de vidas
    La ‘Safo’ de Christina Rosenvinge no convence
    La gran música eleva a una compañía de danza inmadura
    Trampantojo: De veraneo
    ‘Agua y jabón’: más que un libro, una constelación de genialidades
    Ayn Rand, ‘rednecks’ e inclusividad forzada: el mundo virtual como escenario político
    Hay un río de oro negro bajo A Coruña
    Un clavo quita otro, quizás
    Músicas ocultas
    Lo que siento (no se olviden de Ucrania) 
    No eran marcianos, eran humanos
    ‘La máquina de proyectar sueños’, un retorno a la infancia delicioso y temible
    ‘Veinte años de Sol’ o cómo olvidar el pasado con un implante en el cerebro
    ‘La fuga de Siberia en un trineo de renos’, de Trotsky: aventuras de un comunista en plena huida 
    ‘Magallanes & Co.’, los preparativos y aventuras de la primera vuelta al mundo
    Silvia Agüero: “De no ser escritora habría sido vendedora ambulante”
    Antoni Muntadas: “Supe que me dedicaría al arte cuando dejé de pintar”
    Manuel Estrada: “Me agrada que una portada guste más después de leer el libro”
    Óscar Esquivias: “No hace falta ser creyente para leer la Biblia”
    Amelia Castilla: “Los ritos nos ayudan a hacer viable el trance de la muerte”
    Rybakina triunfa y cuela a Rusia en Wimbledon
    Serena a un lado, la rueda gira y gira
    El Tour de Francia de Van Aert y Pogacar
    ¿Por qué es infinito?
    La Superliga, una oportunidad única para la Unión Europea
    ¿Por qué me gusta el fútbol? 
    Rafael no concibe abandonar
    Verstappen no da opción a Ferrari y se lleva la ‘pole’ en Austria  
    Rybakina, el rastro ‘ruso’ en Wimbledon
    Rumbo a Kyrgios, un Djokovic de récord
    Sandra Sánchez, el oro y las lágrimas del adiós al kárate
    El 9 de julio el Tour de Francia celebra San Federico Martín Bahamontes 
    España debuta con cabeza en la Eurocopa
    Hugo Koblet, el James Dean del ciclismo
    Annemiek Van Vleuten, número uno del ciclismo femenino mundial: “Es difícil decir adiós”
    Guélfand, de 54 años, noquea a Yesipenko, de 20, con brío y brillo en el Magistral de León
    La lesión de Alexia Putellas: las roturas del ligamento cruzado anterior son tres veces más frecuentes en mujeres 
    Cielo e infierno de Nadal, entre dos mundos
    Mapi León: “Se demostró que el fútbol femenino solo había que saber venderlo”
    Así tuvo que cambiar Nadal su saque para aguantar el dolor en Wimbledon
    “Si no tenemos en quién mirarnos es muy difícil saber qué queremos ser”
    “Nunca acepto un no por respuesta”
    La nadadora que dejó de competir para hacer campeones de básquet
    Crónica de dos ciudades moldeadas por la misma pasión 
    Así tuvo que cambiar Nadal su saque para aguantar el dolor en Wimbledon
    La lesión de Alexia Putellas: las roturas del ligamento cruzado anterior son tres veces más frecuentes en mujeres 
    Joseph Blatter y Michel Platini, absueltos de los cargos de corrupción por un tribunal suizo
    Dos glorias activas frente a dos jóvenes pujantes en el XXXV Magistral Ciudad de León
    ¿Por qué me gusta el fútbol? 
    La Superliga, una oportunidad única para la Unión Europea
    Rafael no concibe abandonar
    El deporte se complica para las mujeres trans
    Las activistas que dejaron sin anuncios a la web de Steve Bannon van ahora a por Fox News
    “¿Quieres cobrar tu salario en streaming?” Por qué los proyectos cripto son tan difíciles de entender
    El misterio de la carta del soldado alemán
    El porqué de los desafíos criptográficos: conocerte a ti mismo, conocer a tu enemigo
    Con el relax del verano se acentúan los peligros cibernéticos: así puede protegerse durante las vacaciones
    Meta presenta un traductor capaz de operar en tiempo real con 200 idiomas
    “Soy catedrático de informática. Como mis colegas, sé que la tecnología de bitcoin es basura”
    El sector biotecnológico supera las cifras de 2020 y capta 180 millones de euros de inversión privada
    Las mentes humanas detrás de las mentes artificiales
    El juego de la técnica
    Daniel Innerarity: “Los algoritmos son conservadores y nuestra libertad depende de que nos dejen ser imprevisibles”
    Regina Barzilay: “Tenemos que aprender a vivir en un mundo en el que la tecnología toma decisiones que no podemos supervisar”
    Herramientas y trucos para recordar la ubicación del coche gracias al móvil
    Las tres amigas que quieren revolucionar los materiales de construcción con fibras de hongos
    El Constitucional afianza el derecho al olvido en internet
    En defensa de la procrastinación. Elogio del tiempo perdido (frente al que las redes te roban)
    Instagram, el mejor de los mundos posibles
    Del terrorismo youtuber al influencer rabioso: el odio inunda la red
    Cronodiversidad. ¿Por qué el tiempo cada vez va más rápido?
    El impacto de la tecnología en la hostelería: de la comanda digital al pago con código QR
    Herramientas que ayudan a crecer a las empresas (más allá de los pagos)
    Cinco razones por las que este ‘smartphone’ es sencillamente tentador
    La cámara de este ‘smartphone’ es pura magia
    Por qué los nuevos coches ya no los diseñarán las personas
    Una catapulta para el talento de ‘kilómetro cero’
    Las activistas que dejaron sin anuncios a la web de Steve Bannon van ahora a por Fox News
    “¿Quieres cobrar tu salario en streaming?” Por qué los proyectos cripto son tan difíciles de entender
    El misterio de la carta del soldado alemán
    El porqué de los desafíos criptográficos: conocerte a ti mismo, conocer a tu enemigo
    Con el relax del verano se acentúan los peligros cibernéticos: así puede protegerse durante las vacaciones
    Meta presenta un traductor capaz de operar en tiempo real con 200 idiomas
    “Soy catedrático de informática. Como mis colegas, sé que la tecnología de bitcoin es basura”
    El sector biotecnológico supera las cifras de 2020 y capta 180 millones de euros de inversión privada
    Las mentes humanas detrás de las mentes artificiales
    El juego de la técnica
    Daniel Innerarity: “Los algoritmos son conservadores y nuestra libertad depende de que nos dejen ser imprevisibles”
    Regina Barzilay: “Tenemos que aprender a vivir en un mundo en el que la tecnología toma decisiones que no podemos supervisar”
    Herramientas y trucos para recordar la ubicación del coche gracias al móvil
    Las tres amigas que quieren revolucionar los materiales de construcción con fibras de hongos
    El Constitucional afianza el derecho al olvido en internet
    En defensa de la procrastinación. Elogio del tiempo perdido (frente al que las redes te roban)
    Instagram, el mejor de los mundos posibles
    Del terrorismo youtuber al influencer rabioso: el odio inunda la red
    Cronodiversidad. ¿Por qué el tiempo cada vez va más rápido?
    El impacto de la tecnología en la hostelería: de la comanda digital al pago con código QR
    Herramientas que ayudan a crecer a las empresas (más allá de los pagos)
    Cinco razones por las que este ‘smartphone’ es sencillamente tentador
    La cámara de este ‘smartphone’ es pura magia
    Por qué los nuevos coches ya no los diseñarán las personas
    Una catapulta para el talento de ‘kilómetro cero’
    El Baile de la Rosa vuelve a llenar Mónaco de glamur y famosos tras dos años de parón
    ¿Quién es quién en la familia real de Mónaco? Un repaso a los Grimaldi por el Baile de la Rosa
    La cumbre de la alpargata
    El arbusto sauzgatillo, anafrodisiaco en la Antigua Grecia y regulador hormonal en la actualidad 
    Steven Meisel será el segundo fotógrafo al que Marta Ortega homenajea en A Coruña
    Elon Musk, sobre sus 10 hijos: “Estoy haciendo todo lo posible para ayudar con la crisis de despoblación” 
    Juana Martín, primera mujer española “y gitana” en la alta costura de París, tras su desfile: “Me he dejado la piel”
    En verano, el mejor queso del mundo se hace helado
    El espectáculo (teatral) debe continuar en la alta costura de París
    Elon Musk tuvo gemelos en secreto con una ejecutiva de una de sus empresas
    Por qué es importante conocer tu personalidad erótica para tener una vida sexual más satisfactoria
    Realismo, entretenimiento y democracia: la alta costura se deshace por fin de sus estereotipos en París
    Matthew Bronfman, el ‘príncipe’ de Wall Street y heredero de un imperio licorero que se ha reunido con el rey Felipe VI
    Realismo, entretenimiento y democracia: la alta costura se deshace por fin de sus estereotipos en París
    Nostalgia ‘retro’, punto multicolor y otras pistas para vestir este verano
    La dictadura de la perfección, misoginia y Jeffrey Epstein: los ángeles y demonios de Victoria’s Secret
    Dios y diamantes: lo que Kendrick Lamar nos quiso decir a través de su corona de espinas
    Entre Sevilla, Ascot, el pasado y el futuro: Mans condensa su universo en una colección sin miedo a los contrastes
    En verano, el mejor queso del mundo se hace helado
    El placer de lo rancio y otros gustos y aromas que recuerdan la niñez
    Trece aperitivos
    La rebelión de las cocineras de España: “Hay mujeres, falta reconocimiento”
    Carlos Fernández: “Netflix quiere ser de mayor una televisión en abierto, pero de pago” 
    Muere a los 79 años Tony Sirico, Paulie en ‘Los Soprano’
    ‘Encerrado con el diablo’: Dennis Lehane busca en el fondo del alma humana
    Menos mal que quedan los libros
    Stephen King adora ‘Stranger Things’ y viceversa
    Sanfermines: maltrato animal en riguroso directo
    ‘La ciudad es nuestra’ o la corrupción policial
    ‘La noche más larga’ y ‘La lista final’: palomitas sin sabor para el verano
    Los dilemas de ‘La firma de Dios’: ¿y si el virus detrás de una pandemia tuviera conciencia y voluntad propia?
    Arturo Valls pasa de presentador a recluso por rebasar los límites del humor en ‘Dos años y un día’
    Entre novedades y apuestas de bajo riesgo: así es el verano en la televisión en abierto
    Vivir y morir en Colombia, ciencia desmitificada, un nuevo lugar del crimen y adictos a la cultura pop, entre los ‘podcasts’ de julio
    Promover el turismo en tiempos de nueva normalidad, pero con vieja publicidad
    Podium Podcast estrena web con más de 400 contenidos y nuevo diseño
    EGM 2022: La SER cierra temporada de nuevo como la radio más escuchada
    ¿Quién demonios votó a Podemos en La Moraleja?
    La televisión también quiere ser verde: cómo ‘Servir y proteger’ disminuyó la contaminación en su rodaje
    ¿Qué ver hoy en TV? Sábado 9 de julio de 2022
    Nueve capítulos para recordar ‘The Wire’ en su 20º aniversario
    Harry Palmer: el tercer vértice del mágico triángulo de espías británicos
    Las series de junio de 2022: ‘The Boys’ en Amazon Prime Video; ‘Peaky Blinders’ en Netflix y otras
    La fuerza acompaña a ‘Obi-Wan Kenobi’, una serie deslumbrante
    Orgullo con todas las letras
    Pietro Beccari: “El desfile de Sevilla es uno de los mejores, si no el mejor, que ha hecho Maria Grazia Chiuri en Dior”
    Las heridas abiertas de la infancia en Irak
    La cocinera que trasladó las flores del jarrón al plato
    ¿Dónde está exactamente el origen del mal?
    El hombre que persigue la gaita más antigua del mundo
    El golazo literario de Roberto Santiago con ‘Los futbolísimos’
    La uva más denostada de  Castilla-La Mancha resurge de sus cenizas
    La rebelión de las cocineras de España: “Hay mujeres, falta reconocimiento”
    En busca de la huella de Camarón de la Isla, leyenda viva del flamenco
    Oliviero Toscani: “El fascismo resulta muy cómodo porque no te exige razonar”
    República Dominicana, donde la vida fluye por el río infinito
    El placer de lo rancio y otros gustos y aromas que recuerdan la niñez
    Los 10 coches que menos consumen en 2022
    El engaño a los ojos
    Reír a lágrima viva
    El cuerpo
    Cuento de Catherine del Biombo 5
    Harina enriquecida para acabar con el “hambre oculta”
    Lugares de esperanza para salvar los océanos
    Los guardianes del clima que llevan medio siglo leyendo las advertencias de los glaciares
    Epidemia de violencia: claves del negocio de las armas en Estados Unidos
    Los ejecutivos voladores y la ética medioambiental
    Ibiza, entre la noche desenfrenada y el turismo tranquilo 
    Luz Casal: “Tengo el alma rockera. Nada ha doblegado mi rebeldía”
    Rashid Johnson: “Los pensadores y creadores negros estamos cansados de que nos nieguen un espacio autónomo”
    Sergio Hernández: “En el mundo, la gente ya no quiere verdades, quiere mentiras”
    Elvira Sastre: “No hay que perdonarlo todo”
    La trinidad del taco perfecto: “Buena tortilla, delicioso relleno y una salsa sabrosa”
    La casa de Nina Urgell Cloquell, un piso en el que las paredes hablan
    Garbanzos verdes y puchero
    Bikôkô y Julio Peña, dos estrellas en ebullición  
    Cuando Chufy encontró a Rossy: dos amigas, una isla y una colección de moda
    Retrato Robot 
    Ilusiones hipnóticas
    El poder del hormigón
    Maulévrier, Japón a la francesa
    ‘Fantasías en el Prado’, por Alberto García-Alix
    

## Clear

Después de obtener toda esta información, utlizamos `os.system(clear)` para limpiar la pantalla de Python.

En este cuaderno de Jupyter tendremos que cambiar el código a `!cls`


```python
!cls
```

    
    

## Print 

Utilizaremos `print (colored)` para hacer un enunciado en color azul con negrita 


```python
print(colored("A continuación se muestran los titulares de las páginas principales del diario El País que contienen las siguientes palabras:", 'blue', attrs=['bold']))
print(colored("Feminismo", 'green', attrs=['bold']))
```

    [1m[34mA continuación se muestran los titulares de las páginas principales del diario El País que contienen las siguientes palabras:[0m
    [1m[32mFeminismo[0m
    


```python
print(colored("Feminismo", 'green', attrs=['bold']))
```

    [1m[32mFeminismo[0m
    

## Str_match

Se usa una variable de búsqueda de cadena que identifique los resultados que contengan una palabra determinada. En este primer caso la palabra "feminismo".


```python
str_match = [s for s in resultados if "feminismo" in s]
print("\n".join(str_match))
```

    Americanas: Reportajes y noticias sobre feminismo e historias con enfoque de género de la región
    

Se repite este proceso para las palabras: **igualdad, mujeres, mujer, brecha salarial, machismo, violencia, maltrato, homicidio, género, asesinato y sexo.**


```python
print(colored("Igualdad", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "igualdad" in s]
print("\n".join(str_match))
 
print(colored("Mujeres", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "mujeres" in s]
print("\n".join(str_match))
 
print(colored("Mujer", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "mujer" in s]
print("\n".join(str_match))
 
print(colored("Brecha salarial", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "brecha salarial" in s]
print("\n".join(str_match))
 
print(colored("Machismo", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "machismo" in s]
print("\n".join(str_match))
 
print(colored("Violencia", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "violencia" in s]
print("\n".join(str_match))
 
print(colored("Maltrato", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "maltrato" in s]
print("\n".join(str_match))
 
print(colored("Homicidio", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "homicidio" in s]
print("\n".join(str_match))
 
print(colored("Género", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "género" in s]
print("\n".join(str_match))
 
print(colored("Asesinato", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "asesinato" in s]
print("\n".join(str_match))
 
print(colored("Sexo", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "sexo" in s]
print("\n".join(str_match))

```

    [1m[32mIgualdad[0m
    El caso de Georgia, en EE UU: becar sin importar la renta agranda la desigualdad
    La escuela concertada ante las desigualdades: el debate pendiente
    El caso de Georgia, en EE UU: becar sin importar la renta agranda la desigualdad
    [1m[32mMujeres[0m
    La falta de presupuesto ahoga a los refugios para mujeres víctimas de violencia en México
    El Panamá indígena busca empoderar a sus mujeres
    El control del cuerpo de las mujeres
    Un día muy triste para todas las mujeres
    El Supremo de Estados Unidos destruye un derecho fundamental de las mujeres
    La lesión de Alexia Putellas: las roturas del ligamento cruzado anterior son tres veces más frecuentes en mujeres 
    La lesión de Alexia Putellas: las roturas del ligamento cruzado anterior son tres veces más frecuentes en mujeres 
    El deporte se complica para las mujeres trans
    La rebelión de las cocineras de España: “Hay mujeres, falta reconocimiento”
    La rebelión de las cocineras de España: “Hay mujeres, falta reconocimiento”
    [1m[32mMujer[0m
    La falta de presupuesto ahoga a los refugios para mujeres víctimas de violencia en México
    El Panamá indígena busca empoderar a sus mujeres
    El control del cuerpo de las mujeres
    Un día muy triste para todas las mujeres
    El Supremo de Estados Unidos destruye un derecho fundamental de las mujeres
    La lesión de Alexia Putellas: las roturas del ligamento cruzado anterior son tres veces más frecuentes en mujeres 
    La lesión de Alexia Putellas: las roturas del ligamento cruzado anterior son tres veces más frecuentes en mujeres 
    El deporte se complica para las mujeres trans
    Juana Martín, primera mujer española “y gitana” en la alta costura de París, tras su desfile: “Me he dejado la piel”
    La rebelión de las cocineras de España: “Hay mujeres, falta reconocimiento”
    La rebelión de las cocineras de España: “Hay mujeres, falta reconocimiento”
    [1m[32mBrecha salarial[0m
    
    [1m[32mMachismo[0m
    
    [1m[32mViolencia[0m
    La falta de presupuesto ahoga a los refugios para mujeres víctimas de violencia en México
    Epidemia de violencia: claves del negocio de las armas en Estados Unidos
    [1m[32mMaltrato[0m
    Cuatro agentes de la Patrulla Fronteriza enfrentarán procesos disciplinarios tras el maltrato a inmigrantes haitianos en Texas
    ‘Sanfermines: maltrato animal en riguroso directo’, por Eva Güimil
    Un juez archiva la causa contra el exmagistrado del Constitucional Fernando Valdés por supuesto maltrato 
    Sanfermines: maltrato animal en riguroso directo
    [1m[32mHomicidio[0m
    
    [1m[32mGénero[0m
    Los militares de Canadá se abren a un código más inclusivo: tatuajes en el rostro y uniformes sin distinción de género 
    Americanas: Reportajes y noticias sobre feminismo e historias con enfoque de género de la región
    “La identidad de género no se elige, como prueba el fracaso de las terapias de conversión”
    [1m[32mAsesinato[0m
    El detenido por el asesinato: desempleado y antiguo miembro de las fuerzas armadas
    Especial | Secuestro y asesinato de Miguel Ángel Blanco: cuando ETA perdió la calle
    Haití, sin presidente y sin Estado un año después del asesinato de Jovenel Moïse
    Detenido en Brasil el presunto autor intelectual del asesinato de Dom Philips y Bruno Pereira en la Amazonia
    Detenido en Brasil el presunto autor intelectual del asesinato de Dom Philips y Bruno Pereira en la Amazonia
    El juez cita como imputados a tres exjefes de ETA por el asesinato de Miguel Ángel Blanco
    [1m[32mSexo[0m
    
    

**De esta manera terminamos el ejercicio de programación literaria desglosando cada argumento del código fuente.**

A continuación comparto el codigo fuente completo en caso de que se quiera reutilizar. 

## Código fuente completo


```python
import requests
import time
import csv
import re
from bs4 import BeautifulSoup
import os
import pandas as pd
from termcolor import colored
 
resultados = []
 
req = requests.get("https://resultados.elpais.com")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup = BeautifulSoup(req.text, 'html.parser')
 
tags = soup.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req2 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup2 = BeautifulSoup(req2.text, 'html.parser')
 
tags = soup2.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req3 = requests.get("https://elpais.com/opinion")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup3 = BeautifulSoup(req3.text, 'html.parser')
 
tags = soup3.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req4 = requests.get("https://elpais.com/espana/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup4 = BeautifulSoup(req4.text, 'html.parser')
 
tags = soup4.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req5 = requests.get("https://elpais.com/economia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup5 = BeautifulSoup(req5.text, 'html.parser')
 
tags = soup5.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req6 = requests.get("https://elpais.com/sociedad/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup6 = BeautifulSoup(req6.text, 'html.parser')
 
tags = soup6.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req7 = requests.get("https://elpais.com/educacion/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup7 = BeautifulSoup(req7.text, 'html.parser')
 
tags = soup7.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req8 = requests.get("https://elpais.com/clima-y-medio-ambiente/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup8 = BeautifulSoup(req8.text, 'html.parser')
 
tags = soup8.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req9 = requests.get("https://elpais.com/ciencia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup9 = BeautifulSoup(req9.text, 'html.parser')
 
tags = soup9.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req10 = requests.get("https://elpais.com/cultura/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup10 = BeautifulSoup(req10.text, 'html.parser')
 
tags = soup10.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req11 = requests.get("https://elpais.com/babelia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup11 = BeautifulSoup(req11.text, 'html.parser')
 
tags = soup11.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req12 = requests.get("https://elpais.com/deportes/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup12 = BeautifulSoup(req12.text, 'html.parser')
 
tags = soup12.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req13 = requests.get("https://elpais.com/tecnologia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup13 = BeautifulSoup(req13.text, 'html.parser')
 
tags = soup13.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req14 = requests.get("https://elpais.com/tecnologia/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup14 = BeautifulSoup(req14.text, 'html.parser')
 
tags = soup14.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req15 = requests.get("https://elpais.com/gente/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup15 = BeautifulSoup(req15.text, 'html.parser')
 
tags = soup15.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req16 = requests.get("https://elpais.com/television/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup16 = BeautifulSoup(req16.text, 'html.parser')
 
tags = soup16.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
req17 = requests.get("https://elpais.com/eps/")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup17 = BeautifulSoup(req17.text, 'html.parser')
 
tags = soup17.findAll("h2")
 
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
 
 
os.system("clear")
 
print(colored("A continuación se muestran los titulares de las páginas principales del diario El País que contienen las siguientes palabras:", 'blue', attrs=['bold']))
print(colored("Feminismo", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "feminismo" in s]
print("\n".join(str_match))
 
print(colored("Igualdad", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "igualdad" in s]
print("\n".join(str_match))
 
print(colored("Mujeres", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "mujeres" in s]
print("\n".join(str_match))
 
print(colored("Mujer", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "mujer" in s]
print("\n".join(str_match))
 
print(colored("Brecha salarial", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "brecha salarial" in s]
print("\n".join(str_match))
 
print(colored("Machismo", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "machismo" in s]
print("\n".join(str_match))
 
print(colored("Violencia", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "violencia" in s]
print("\n".join(str_match))
 
print(colored("Maltrato", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "maltrato" in s]
print("\n".join(str_match))
 
print(colored("Homicidio", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "homicidio" in s]
print("\n".join(str_match))
 
print(colored("Género", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "género" in s]
print("\n".join(str_match))
 
print(colored("Asesinato", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "asesinato" in s]
print("\n".join(str_match))
 
print(colored("Sexo", 'green', attrs=['bold']))
 
str_match = [s for s in resultados if "sexo" in s]
print("\n".join(str_match))



```


      Input In [6]
        ```
        ^
    SyntaxError: invalid syntax
    



```python

```
