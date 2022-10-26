# Prueba Tecnica 
En el siguiente informe presento la documentacion sobre el desarrollo de la prueba tecnica de recoleccion automatica, se expondra los nombres de los recursos instalados, la manera de instalacion y el desarrollo del codigo, cabe resaltar que cada linea importante de codigo esta comentada sobre lo que esta realizando, de igual manera explicare que sucede en cada parte del mismo
<h1 align="center"> PRIMERA PRUEBA</h1>

---
# ACTIVIDAD 1
---
El desarrollo de la actividad 1 se baso en la recoleccion de datos del archivo data 1-3.pdf, el cual contenia tablas con distintos campos. Los campos a recopilar eran.

-Permit type.

-Workclass.

-Contractor

-Permit

-Issued

-Address

-Project description

-Value

Para ello se utilizo un entorno virtual, librerias y expresiones regulares. Estos datos luego serian pasados a una tabla en el orden que especifica la extraccion y luego enviados a un archivo de excel.

---
# REQUERIMIENTOS
---

Lo primero que necesitamos fue la instalacion de python en windows 8, esto se simplifico descargando el programa Anaconda que con su ultima actualizacion trae python incluido, asi como el entorno de Jupyter notebook que fue el elegido  para realizar el codigo. Luego de ello procedemos a instalar los paquetes necesarios para la extraccion de datos como lo es la libreria de pandas, lector de expresiones regulares y lector de pdf.

---
# INSTALACION
---

Para descargar Anaconda fuimos a la web oficial de anaconda y buscamos el boton download. Que nos lleva al siguiente link

https://repo.anaconda.com/archive/Anaconda3-2022.10-Windows-x86_64.exe

Una vez presionemos el link se inica la descarga. Al completar la descarga se inicia el programa de instalacion, el cual contiene una serie de pasos a seguir. Una vez completada la misma, podemos iniciar Anaconda el cual nos va a llevar a un entorno que contiene varios entornos virtuales, seleccionamos Jupyter notebook.

Una vez dentro del entorno virtual, procedemos a descargar los paquetes necesarios con el sistema de gestion de paquetes pip, este es utilizado para instalar y administrar pquetes de software escritos en python.

Instalamos los paquetes que vamos a utilizar para la primera actividad, para ello vamos a colocar el nombre del sistema de gestion de paquetes (pip) seguido de (install) seguido de e paquete que vamos a descargar, todo esto lo podemos hacer en Jupyter, una vez colocado esto, presionamos enter e iniciara la descarga e instalacion de los mismos.

pip install re

pip install pandas

pip install pdfplumber

El primer paquete habilita las expresiones regulares en python, el segundo es la bibloteca de pandas y el tercero nos permite hacer lectura del pdf y sacar el texto.

---
# Ejecucion
---

---
# ACTIVIDAD 2
---

El desarrollo de la actividad 1 se baso en la recoleccion de datos del archivo "data 2-2.pdf", el cual contenia tablas con distintos campos. Los campos a recopilar eran.

-Permit Number

-Project type

-Address

-Parcel

-Issued date

-Total fees

-Valuation

-Type class description

-Contractor

-Property owner

-Sq feet

Para ello se utilizo un entorno virtual, librerias y expresiones regulares. Estos datos luego serian pasados a una tabla en el orden que especifica la extraccion y luego enviados a un archivo de excel.

---
# REQUERIMIENTOS
---

Anaconda, python, Jupyter notebook

re, pandas, pdfplumber

Los procesos de instalacion ya fueron realizados en la priemra actividad

---
# Ejecucion
---

<h1 align="center"> SEGUNDA PRUEBA</h1>
---

# SCRAPY yellowpages

# Documentacion
