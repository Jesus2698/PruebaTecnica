El siguiente proceso fue realizado de manerda didactica unicamente para probar y aprender sobre las librerias de python.

---

# Prueba Técnica 
En el siguiente informe presento la documentación sobre el desarrollo de la prueba técnica de recolección automática, se expondrá los nombres de los recursos instalados, la manera de instalación y el desarrollo del código, cabe resaltar que cada línea importante de código esta comentada sobre lo que esta realizando, de igual manera explicare que sucede en cada parte del mismo
<h1 align="center"> PRIMERA PRUEBA</h1>

---
# ACTIVIDAD 1
---
El desarrollo de la actividad 1 se basó en la recolección de datos del archivo data 1-3.pdf, el cual contenía tablas con distintos campos. Los campos a recopilar eran.

-Permit type.

-Workclass.

-Contractor

-Permit

-Issued

-Address

-Project description

-Value

Para ello se utilizó un entorno virtual, librerías y expresiones regulares. Estos datos luego serían pasados a una tabla en el orden que especifica la extracción y luego enviados a un archivo de excel.

---
# REQUERIMIENTOS
---

Lo primero que necesitamos fue la instalación de python en Windows 8, esto se simplifico descargando el programa Anaconda que con su última actualización trae python incluido, asi como el entorno de Jupyter notebook que fue el elegido  para realizar el código. Luego de ello procedemos a instalar los paquetes necesarios para la extracción de datos como lo es la librería de pandas, lector de expresiones regulares y lector de pdf.

---
# INSTALACION
---

Para descargar Anaconda fuimos a la web oficial de anaconda y buscamos el botón download. Que nos lleva al siguiente link

https://repo.anaconda.com/archive/Anaconda3-2022.10-Windows-x86_64.exe

Una vez presionemos el link se inicia la descarga. Al completar la descarga se inicia el programa de instalación, el cual contiene una serie de pasos a seguir. Una vez completada la misma, podemos iniciar Anaconda el cual nos va a llevar a un entorno que contiene varios entornos virtuales, seleccionamos Jupyter notebook.

Una vez dentro del entorno virtual, procedemos a descargar los paquetes necesarios con el sistema de gestión de paquetes pip, este es utilizado para instalar y administrar pquetes de software escritos en python.

Instalamos los paquetes que vamos a utilizar para la primera actividad, para ello vamos a colocar el nombre del sistema de gestión de paquetes (pip) seguido de (install) seguido de e paquete que vamos a descargar, todo esto lo podemos hacer en Jupyter, una vez colocado esto, presionamos enter e iniciara la descarga e instalación de los mismos.

pip install re

pip install pandas

pip install pdfplumber

El primer paquete habilita las expresiones regulares en python, el segundo es la biblioteca de pandas y el tercero nos permite hacer lectura del pdf y sacar el texto.

---

# Ejecución

---
Importamos las librerías que se van a utilizar

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura.PNG"/></p> 

Llamamos al archivo que contiene la tabla

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura2.PNG"/></p> 

Creamos las listas resultado de la toma de datos de las tablas linea por linea

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura3.PNG"/></p> 

Para iniciar creamos un bucle for en un rango de 16, luego de esto extraemos el texto del pdf, la variable i sera el número de páginas que va a revisar una vez se haya completado el bucle, es decir, la variable i = 0, al final del bucle i++, al comenzar el la próxima lectura estaría validada en 1, así hasta llegar hasta 15.

Se hizo una búsqueda con expresiones regulares línea por línea. Para sacar un dato especifico se duplico la línea, ejemplo( la línea de código 1 y 2 son exactamente iguales, de la linea 1 saque PermitNumber, en la linea 2 lo elimino y saco Address, esto me permite crear dos listas aparte.
Este mismo proceso sucedió con las distintas líneas. 

También se elaboró un dataframe para posteriormente ser conectado al resto.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura4.PNG"/></p> 

Se realizó la conexión de todos las tablas en una sola

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura5.PNG"/></p> 


CODIGO COMPLETO

---

import pandas as pd #importamos libreria de pandas
import re  #importamos libreria para expresiones regulares
import pdfplumber #importamos el lector de pdf


#archivo pdf
file = "data.pdf"


#iterador
i=0
#listas finales
resultado=[]
resultado2=[]
resultado3=[]
resultado4=[]
resultado5=[]
resultado6=[]
resultado7=[]

#bucle en un rango de 16 paginas
for i in range(16):
    
    #abrimos el archivo pdf   
    with pdfplumber.open(file) as pdf:
        
        #extraemos el texto
        page =pdf.pages[i]
        text = page.extract_text()
        
        #realizamos la busqueda de la primera linea, posteriormente es enviada a una lista, para luego crear una tabla con ella
        for item in re.finditer("(?P<PermitNumber>\w+\-\d+\-\d+)\s(\d{12})\s(\d+\s\w+\s\w+|\d+\s\w+\s\w+\s\w{2})", text):
            resultado.append(item.groupdict())
            df1 = pd.DataFrame(resultado)
            
        #realizamos la busqueda de la segunda linea, posteriormente es enviada a una lista, para luego crear una tabla con ella   
        for item in re.finditer("(\w+\-\d+\-\d+)\s(\d{12})\s(?P<Address>\d+\s\w+\s\w+|\d+\s\w+\s\w+\s\w{2})", text):
            resultado6.append(item.groupdict())
            df11 = pd.DataFrame(resultado6)
            
         #realizamos la busqueda de la tercera linea, posteriormente es enviada a una lista, para luego crear una tabla con ella   
        for item in re.finditer("(?P<Contractor>[a-z]\s[A-Z]+\s[A-Z]+|[I]\s[A-Z]+\s[A-Z]+|[a-z]\s[A-Z]+\s\&+\s[A-Z]+|\s+\s\d)", text):
            resultado2.append(item.groupdict())
            df2 = pd.DataFrame(resultado2) 
            
         #realizamos la busqueda de la cuarta linea, posteriormente es enviada a una lista, para luego crear una tabla con ella   
        for item in re.finditer("(?P<Permitype>\w+\s\(\w\))\s\-\s(?P<Workclass>\w+\/\w+\s\(\w+\)|\w+\/\w+|\w+\s\w+)", text):
            resultado3.append(item.groupdict())
            df3 = pd.DataFrame(resultado3)    
            
         #realizamos la busqueda de la quinta linea, posteriormente es enviada a una lista, para luego crear una tabla con ella   
        for item in re.finditer("(?P<Issued>\d{2}\/\d+\/\d{2})\s(\$.*)", text):
            resultado4.append(item.groupdict())
            df4 = pd.DataFrame(resultado4)
            
         #realizamos la busqueda de la sexta linea, posteriormente es enviada a una lista, para luego crear una tabla con ella   
        for item in re.finditer("(\d{2}\/\d+\/\d{2})\s(?P<Value>\$.*)", text):
            resultado7.append(item.groupdict())
            df44 = pd.DataFrame(resultado7)     
            
         #realizamos la busqueda de la septima linea, posteriormente es enviada a una lista, para luego crear una tabla con ella   
        for item in re.finditer("\w+\,\s\w{2}\s\d+ (?P<ProjectDescription>.*)", text):
            resultado5.append(item.groupdict())
            df5 = pd.DataFrame(resultado5)    
    i=i+1          


#se realizo la conexion de todas las tablas obtenidas
df_1=pd.merge(df3, df2, right_index=True, left_index=True)
df_2=pd.merge(df_1, df1, right_index=True, left_index=True)
df_3=pd.merge(df_2, df4, right_index=True, left_index=True)
df_4=pd.merge(df_3, df11, right_index=True, left_index=True)
df_5=pd.merge(df_4, df5, right_index=True, left_index=True)
dfinal=pd.merge(df_5, df44, right_index=True, left_index=True)


#guardamos la tabla en un pdf
dfinal.to_csv('tb1.csv')

---

---
# ACTIVIDAD 2
---

El desarrollo de la actividad 1 se basó en la recolección de datos del archivo "data 2-2.pdf", el cual contenía tablas con distintos campos. Los campos a recopilar eran.

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

Para ello se utilizó un entorno virtual, librerías y expresiones regulares. Estos datos luego serian pasados a una tabla en el orden que especifica la extracción y luego enviados a un archivo de excel.

---
# REQUERIMIENTOS
---

Anaconda, python, Jupyter notebook

re, pandas, pdfplumber

Los procesos de instalación ya fueron realizados en la primera actividad

---
# Ejecución
---

Importamos las librerías que se van a utilizar

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Capturas.PNG"/></p>

Llamamos al archivo que contiene la tabla. Creamos las listas resultado de la toma de datos de las tablas línea por línea

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura11.PNG"/></p> 


Para iniciar creamos un bucle for en un rango de 42, luego de esto extraemos el texto del pdf, la variable i será el número de páginas que va a revisar una vez se haya completado el bucle, es decir, la variable i = 0, al final del bucle i++, al comenzar el la próxima lectura estaría validada en 1, asi hasta llegar hasta 15.

Se hizo una búsqueda con expresiones regulares línea por línea. Para sacar un dato especifico se duplico la línea, ejemplo( la línea de código 1 y 2 son exactamente iguales, de la linea 1 saque PermitNumber, en la línea 2 lo elimino y saco Address, esto me permite crear dos listas aparte.
Este mismo proceso sucedió con las distintas líneas. 

También se elaboró un dataframe para posteriormente ser conectado al resto.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura22.PNG"/></p> 

Las búsquedas de las lineas 3 y 4 obtenían duplicados, fueron eliminadas

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura33.PNG"/></p> 

se realizo la conexion de todas las tablas

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura44.PNG"/></p> 

---

CODIGO

---
import pandas as pd #importamos libreria de pandas
import re  #importamos libreria para expresiones regulares
import pdfplumber #importamos el lector de pdf



#archivo pdf
file = "data2.pdf"



#listas finales
resultado=[]
resultado1=[]
resultado12=[]
resultado13=[]
resultado2=[]
resultado3=[]
resultado33=[]
resultado4=[]
resultado44=[]
resultado5=[]
resultado55=[]
resultado56=[]
resultado6=[]
#iterador
i=0




#bucle en un rango de 43 paginas
for i in range(43):
    
    #abrimos el archivo pdf
    with pdfplumber.open(file) as pdf:
    #extraemos el texto
            page =pdf.pages[i]
            text = page.extract_text()
            
            #realizamos la busqueda de la primera linea, posteriormente es enviada a una lista, para luego crear una tabla con ella
            for item in re.finditer("(?P<PermitNumber>[A-Z]{2}\d{2}\-\d{4})\s([A-Z]{2}\d+\s\d{4}|[A-Z]{2}\d+\s\d{4}[A-Z])\s(\d{5}\s\w+\s\w+\s\w+)\s(.*)", text):
                resultado.append(item.groupdict())
                df1 = pd.DataFrame(resultado)
                
            #localizamos uno de los datos obtenidos en la primera linea y se convierte en tabla
            for item in re.finditer("([A-Z]{2}\d{2}\-\d{4})\s([A-Z]{2}\d+\s\d{4}|[A-Z]{2}\d+\s\d{4}[A-Z])\s(?P<Address>\d{5}\s\w+\s\w+\s\w+)\s(.*)", text):
                resultado1.append(item.groupdict())
                df11 = pd.DataFrame(resultado1) 
                
            #localizamos uno de los datos obtenidos en la primera linea y se convierte en tabla
            for item in re.finditer("([A-Z]{2}\d{2}\-\d{4})\s([A-Z]{2}\d+\s\d{4}|[A-Z]{2}\d+\s\d{4}[A-Z])\s(\d{5}\s\w+\s\w+\s\w+)\s(?P<IssuedDate>.*)", text):
                resultado13.append(item.groupdict())
                df13 = pd.DataFrame(resultado13)    
            #localizamos uno de los datos obtenidos en la primera linea y se convierte en tabla
            for item in re.finditer("([A-Z]{2}\d{2}\-\d{4})\s(?P<Parcel>[A-Z]{2}\d+\s\d{4}|[A-Z]{2}\d+\s\d{4}[A-Z])\s(\d{5}\s\w+\s\w+\s\w+)\s(.*)", text):
                resultado12.append(item.groupdict())
                df12 = pd.DataFrame(resultado12)       
                
            #realizamos la busqueda de la segunda linea, posteriormente es enviada a una lista, para luego crear una tabla con ella
            for item in re.finditer("(?P<Description>(.*)) ([I]ssued)", text):
                resultado2.append(item.groupdict()) 
                df2 = pd.DataFrame(resultado2)

            #realizamos la busqueda de la tercera linea, posteriormente es enviada a una lista, para luego crear una tabla con ella     
            for item in re.finditer("([P]hone\n)((.*)) (?P<PhoneContractor>\(\d{3}\)\s\d{3}\-\d{4}|\s) ((.*) (\(\d{3}\)\s\d{3}\-\d{4})|(.*))", text):
                resultado3.append(item.groupdict()) 

            #localizamos uno de los datos obtenidos en la tercera linea y se convierte en tabla
            for item in re.finditer("(?P<contractor>(.*)) (\(\d{3}\)\s\d{3}\-\d{4}) ((.*) (\(\d{3}\)\s\d{3}\-\d{4})|(.*))", text):
                resultado33.append(item.groupdict()) 
                df33 = pd.DataFrame(resultado33)
            #realizamos la busqueda de la cuarta linea, posteriormente es enviada a una lista, para luego crear una tabla con ella     
            for item in re.finditer("([P]hone\n)((.*)) (?P<PhoneOwner>\(\d{3}\)\s\d{3}\-\d{4}) ((.*) (\(\d{3}\)\s\d{3}\-\d{4})|(.*))", text):
                resultado4.append(item.groupdict()) 

            #localizamos uno de los datos obtenidos en la cuarta linea y se convierte en tabla
            for item in re.finditer("(?P<owner>(.*)) (\(\d{3}\)\s\d{3}\-\d{4}) ((.*) (\(\d{3}\)\s\d{3}\-\d{4})|(.*))", text):
                resultado44.append(item.groupdict()) 
                df44 = pd.DataFrame(resultado44)
                
            #realizamos la busqueda de la quinta linea, posteriormente es enviada a una lista, para luego crear una tabla con ella     
            for item in re.finditer("(\$\d+\,\d+\.\d{2}|\$\d+\,\d+\,\d+\.\d{2}) (?P<TotalFee>\$\d+\,\d+\.\d{2}|d+\.\d{2}) (\$\d+\,\d+\.\d{2}) (\$\d+\.\d+) (\d+\.\d+)", text):
                resultado5.append(item.groupdict())
                df5 = pd.DataFrame(resultado5)
            #localizamos uno de los datos obtenidos en la quinta linea y se convierte en tabla
            for item in re.finditer("(?P<valuation>\$\d+\,\d+\.\d{2}|\$\d+\,\d+\,\d+\.\d{2}|d+\.\d{2}) (\$\d+\,\d+\.\d{2}) (\$\d+\,\d+\.\d{2}) (\$\d+\.\d+) (\d+\.\d+)", text):
                resultado55.append(item.groupdict())
                df55 = pd.DataFrame(resultado55)   
            #localizamos uno de los datos obtenidos en la quinta linea y se convierte en tabla
            for item in re.finditer("(\$\d+\,\d+\.\d{2}|\$\d+\,\d+\,\d+\.\d{2}) (\$\d+\,\d+\.\d{2}) (\$\d+\,\d+\.\d{2}) (\$\d+\.\d+) (?P<SqFeet>\d+\.\d+)", text):
                resultado56.append(item.groupdict())
                df56 = pd.DataFrame(resultado56)  
            #realizamos la busqueda de un dato en la primera linea, posteriormente es enviada a una lista, para luego crear una tabla con ella
            for item in re.finditer("(?P<type>[B]uilding \-)", text):
                resultado6.append(item.groupdict())    
                df6 = pd.DataFrame(resultado6)


#las busquedas de resultado3 y resultado4 obtenian datos duplicados, se eliminan los duplicados                

resultado3.pop(1)
resultado3.pop(2)
resultado4.pop(0)
resultado4.pop(1)  
#se crean las tablas restantes
df4 = pd.DataFrame(resultado4)
df3 = pd.DataFrame(resultado3)
i=i+1


#se realizo la conexion de todas las tablas obtenidas
df_1=pd.merge(df1, df6, right_index=True, left_index=True)
df_2=pd.merge(df_1, df11, right_index=True, left_index=True)
df_3=pd.merge(df_2, df12, right_index=True, left_index=True)
df_4=pd.merge(df_3, df13, right_index=True, left_index=True)
df_5=pd.merge(df_4, df5, right_index=True, left_index=True)
df_6=pd.merge(df_5, df55, right_index=True, left_index=True)
df_7=pd.merge(df_6, df2, right_index=True, left_index=True)
df_8=pd.merge(df_7, df33, right_index=True, left_index=True)
df_9=pd.merge(df_8, df3, right_index=True, left_index=True)
df_10=pd.merge(df_9, df44, right_index=True, left_index=True)
df_11=pd.merge(df_10, df4, right_index=True, left_index=True)
dfinal=pd.merge(df_11, df56, right_index=True, left_index=True)



#guardamos la tabla en un pdf

dfinal.to_csv('tb2.csv')

---


<h1 align="center"> SEGUNDA PRUEBA</h1>
---

# SCRAPY yellowpages

# Documentación

Este scraping web se llevó acabo en el mismo entorno virtual Jupyter notebook, se realizó una recolección de datos de cada número posible en la lista de números.

# Requerimientos

Para este script se utilizó Anaconda, Python, Jupyter notebook, chromedriver y los paquetes necesarios para la extracción de información.

# Ejecucion

Se importaron las librerías necesarias, luego de esto se llamó al archivo que contiene los números, para pasar a ser extraidos.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/a1.PNG"/></p> 

Se crearon las listas y un iterados para bucle. Con webdriver inicializamos chrome

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/a2.PNG"/></p> 

Inicializamos en la página, al momento de cargar se busca el campo de search para ingresar el primer número telefónico, una vez el programa coloca el número, encuentra el botón buscar.

Si el programa encuentra un resultado que coincide, lo selecciona. Al abrir la página de este match, extrae los datos deseados.

Al finalizar la extracción, se procede a iniciar de nuevo la página y agrega el siguiente número. Los resultados de estas búsquedas son guardadas en una lista.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/a3.PNG"/></p> 

Para evitar campos vacios se igualan las tablas, Luego de esto se procede a crear un dataframe y guardar los datos en excel.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/a4.PNG"/></p>

CODIGO
---
from selenium import webdriver #webdriver
import os #libreria os
import time #libreria para tiempo
from selenium.webdriver.common.keys import Keys #keys para introducir data
import pandas as pd #libreria de pandas
import xlrd #lector de excel


#archivo que contiene los números telefónicos
archivo = 'phones.xlsx'


#abrir el archivo para extraer datos
wb = xlrd.open_workbook(archivo)



#abrimos la primera página del archivo
hoja = wb.sheet_by_index(0)
#iterador
i=1
#listas
lista_nombre=[]
lista_telefono=[]
lista_direccion=[]



#Inicializamos webdriver para abrir chrome
ruta = os.getcwd()
driver_path = '{}\chromedriver.exe'.format(ruta)
driver = webdriver.Chrome(driver_path)


#bucle for en un rango de 1000 repeticiones para tomar los 1000 numeros
for i in range(1000):
    
    #tomamos el archivo con los numeros y los metemos en una variable numeros, cabe resaltar que el indice va a ser i, que aumentara de 1 en 1
    numeros=hoja.cell_value(i,0)
    
    #abrimos la pagina 
    driver.get('https://www.yellowpages.com/')
    
    #nos posicionamos en la casilla de buscador e introducimos el numero de telefono i
    numero = driver.find_element("xpath",'//div[@class="search-bar"]//div[@class="on-focus search-container"]//input[@name="search_terms"]')
    numero.send_keys(int(numeros)) 
    time.sleep(10)
    
    #presionamos el boton buscar
    enviar = driver.find_element("xpath",'//button[@type="submit"]')
    enviar.click()
    
    #si encuentra una coincidencia tomar los datos 
    try:
        enviar2 = driver.find_element("xpath",'//div[@class="result"][1]//div[@class="srp-listing clickable-area mdm"]//div[@class="info"]')
        enviar2.click()   
        time.sleep(10)
        
        nombres = driver.find_elements("xpath",'//article[@class="business-card non-paid-listing"]//div[@class="sales-info"]//h1[@class="dockable business-name"]')
        nombres = [i.text for i in nombres]
        
        telefono = driver.find_elements("xpath",'//section[@id="details-card"]//p[@class="phone"]')
        telefono = [i.text for i in telefono]
        
        direccion=[]
        
        
        direccion = driver.find_elements("xpath",'//a[@class="directions small-btn"]//span[@class="address"]')
        direccion = [i.text for i in direccion]
        if (len(direccion)) == 1:
            continue
        else:
            direccion = 0
        
        
        #una vez tenga los datos, enviarlos a una lista
        lista_nombre.extend(nombres)
        lista_telefono.extend(telefono)
        lista_direccion.extend(direccion)
        
    #si no encuentra coincidencias continuar con el bucle 
    except:
        pass
    
    
    
    i=i+1


#rellenar espacios vacios si los hay
if len(lista_nombre) != len(lista_direccion):
    lista_direccion += (len(lista_nombre)-len(lista_direccion)) * [0]
    
if len(lista_nombre) != len(lista_telefono):
    lista_telefono += (len(lista_nombre)-len(lista_telefono)) * [0]


#crear una tabla con las listas obtenidas
df = pd.DataFrame({"Nombre": lista_nombre, "Telefono": lista_telefono, "Direccion": lista_direccion})



#guardamos la tabla en un pdf
df.to_csv('scraping3.csv')


# SCRAPY yelp

# Documentación

Este scraping web se llevó acabo en el mismo entorno virtual Jupyter notebook, se realizó una recolección de datos de cada nombre posible en la lista nombres.

# Requerimientos

Para este scrip se utilizó Anaconda, Python, upyter notebook, chromedriver y los paquetes necesarios para la extracción de información.

# Ejecución

Se importaron las librerías necesarias, luego de esto se llamó al archivo que contiene los nombres, para pasar a ser extraídos.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/b1.PNG"/></p> 

Se crearon las listas y un iterador para bucle. Con webdriver inicializamos chrome

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/b2.PNG"/></p> 

Inicializamos en la página, al momento de cargar se busca el campo de search para ingresar el primer nombre, una vez el programa coloca el número, encuentra el botón buscar.

Si el programa encuentra un resultado que coincide, lo selecciona. Al abrir la página de este match, extrae los datos deseados. A esto se le agrega un código extra ya que al momento de abrir la página del match se abre en una nueva pestaña, esto quiere decir que debemos pasar a esa pestaña para trabajar.

Al finalizar la extracción, se cierra la pestaña, se procede a iniciar de nuevo la página y agrega el siguiente nombre. Los resultados de estas búsquedas son guardadas en una lista.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/b3.PNG"/></p> 

Para evitar campos vacíos se igualan las tablas, Luego de esto se procede a crear un dataframe y guardar los datos en excel.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/b4.PNG"/></p>

CODIGO
---
from selenium import webdriver #webdriver
import os #libreria os
import time #libreria para tiempo
from selenium.webdriver.common.keys import Keys #keys para introducir data
import pandas as pd #libreria de pandas
import xlrd #lector de excel


#archivo que contiene los numeros telefonicos
archivo = 'nombres.xlsx'



#abrir el archivo para extraer datos
wb = xlrd.open_workbook(archivo)



#abrimos la primera pagina del archivo
hoja = wb.sheet_by_index(0)
#iterador
i=0
#listas
lista_nombre=[]
lista_telefono=[]
lista_direccion=[]


#Inicializamos webdriver para abrir chrome
ruta = os.getcwd()
driver_path = '{}\chromedriver.exe'.format(ruta)
driver = webdriver.Chrome(driver_path)



#bucle for en un rango de 1000 repeticiones para tomar los 1000 numeros
for i in range(1):
    
    #tomamos el archivo con los numeros y los metemos en una variable numeros, cabe resaltar que el indice va a ser i, que aumentara de 1 en 1
    numeros=hoja.cell_value(i,0)
    
    #abrimos la pagina 
    driver.get('https://www.yelp.com/')
    
    #nos posicionamos en la casilla de buscador e introducimos el numero i
    numero = driver.find_element("xpath",'//div[@class=" find-near-arrange-unit__09f24__hvHOu arrange-unit__09f24__rqHTg border-color--default__09f24__NPAKY"]//input[@placeholder="tacos, cheap dinner, Max’s"]')
    numero.send_keys(numeros) 
    time.sleep(10)
    
    #presionamos el boton buscar
    enviar = driver.find_element("xpath",'//div[@class=" buttons-arrange-unit__09f24___9Q7O arrange-unit__09f24__rqHTg border-color--default__09f24__NPAKY"]//button[@type="submit"]')
    enviar.click()
    

    
    #Al hacer click en el boton esta redirige a otra pagina
    enviar = driver.find_element("xpath",'//li[@class=" border-color--default__09f24__NPAKY"]//div[@class=" arrange-unit__09f24__rqHTg arrange-unit-fill__09f24__CUubG border-color--default__09f24__NPAKY"]//a[@class="css-1m051bw"]')
    enviar.click()
    #pasamos a trabajar con la pagina que se abrio
    driver.switch_to.window(driver.window_handles[1])
    
    #creamos un try para saltar errores
    try:   
        
        #buscamos los nombres y los guardamos en una variable
        nombres = driver.find_elements("xpath",'//div[@class=" margin-b1__09f24__vaLrm border-color--default__09f24__NPAKY"]//h1[@class="css-1se8maq"]')
        nombres = [i.text for i in nombres]
        #buscamos los telefonos y los guardamos en una variable
        telefono = driver.find_elements("xpath",'/html/body/yelp-react-root/div[1]/div[2]/div/div/div[2]/div/div[2]/div/aside/section[2]/div/div[2]/div/div[1]/p[2]')
        telefono = [i.text for i in telefono]

        #por lo general los telefonos son mas complicados de conseguir asi que creamos un condicional por si no encuentra datos
        direccion=[]
        direccion = driver.find_elements("xpath",'//*[@id="location-and-hours"]/section/div[2]/div[1]/div/div/div/div/p')
        direccion = [i.text for i in direccion]
        if (len(direccion)) == 1:
            continue
        else:
            direccion = 0
        

        #una vez tenga los datos, enviarlos a una lista
        lista_nombre.extend(nombres)
        lista_telefono.extend(telefono)
        lista_direccion.extend(direccion)
        
        #si no encuentra coincidencias continuar con el bucle 
    except:
        pass
    #cerramos la pagina 
    driver.close()
    
i=i+1


#crear una tabla con las listas obtenidas
df = pd.DataFrame({"Nombre": lista_nombre, "Telefono": lista_telefono, "Direccion": lista_direccion})


#guardamos la tabla en un pdf
df.to_csv('scraping2.csv')

# SCRAPY bbb.org

# Documentación

Este scraping web se llevó a cabo en el mismo entorno virtual Jupyter notebook, se realizó una recolección de datos de cada número posible en la lista números.

# Requerimientos

Para este scrip se utilizó Anaconda, Python, upyter notebook, chromedriver y los paquetes necesarios para la extracción de información.

# Ejecución

Se importaron las librerías necesarias, luego de esto se llamó al archivo que contiene los números, para pasar a ser extraídos.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/c1.PNG"/></p> 

Se crearon las listas y un iterados para bucle. Con webdriver inicializamos chrome

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/c2.PNG"/></p> 

Inicializamos en la página, al momento de cargar se busca el campo de search para ingresar el primer número, una vez el programa coloca el número, encuentra el botón buscar.

Si el programa encuentra un resultado que coincide, lo selecciona. Al abrir la página de este match, extrae los datos deseados. 

Al finalizar la extracción, se cierra la pestaña, se procede a iniciar de nuevo la pagina y agrega el siguiente número. Los resultados de estas búsquedas son guardadas en una lista.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/c3.PNG"/></p> 

Luego de esto se procede a crear un dataframe y guardar los datos en excel.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/c4.PNG"/></p>

CODIGO

---
from selenium import webdriver #webdriver
import os #libreria os
import time #libreria para tiempo
from selenium.webdriver.common.keys import Keys #keys para introducir data
import pandas as pd #libreria de pandas
import xlrd #lector de excel



#archivo que contiene los numeros telefonicos
archivo = 'phones.xlsx'




#abrir el archivo para extraer datos
wb = xlrd.open_workbook(archivo)



#abrimos la primera pagina del archivo
hoja = wb.sheet_by_index(0)
#iterador
i=0
#listas
lista_nombre=[]
lista_telefono=[]
lista_direccion=[]



#Inicializamos webdriver para abrir chrome
ruta = os.getcwd()
driver_path = '{}\chromedriver.exe'.format(ruta)
driver = webdriver.Chrome(driver_path)



#bucle for en un rango de 1000 repeticiones para tomar los 1000 números
for i in range(1000):
    
    #tomamos el archivo con los números y los metemos en una variable números, cabe resaltar que el indice va a ser i, que aumentara de 1 en 1
    numeros=hoja.cell_value(i,0)
    
    #abrimos la pagina 
    driver.get('https://www.bbb.org/')
    
    #nos posicionamos en la casilla de buscador e introducimos el numero i
    numero = driver.find_element("xpath",'//div[@class="dtm-header-find-typeahead e164phwk0 css-9pv0b2 e19i3r7f0"]//div[@class="css-1vhtc4i e139l4qs0"]//input[@type="search"]')
    numero.send_keys(int(numeros)) 
    time.sleep(10)
    
    #presionamos el boton buscar
    enviar = driver.find_element("xpath",'//div[@class="css-1xdgpde epsahlr0"]//button[@class="bds-button dtm-header-search-submit css-83ihwb erb0q3a0"]')
    enviar.click()
    
    #si encuentra una coincidencia tomar los datos 
    try:
        nombres = driver.find_elements("xpath",'//div[@class="stack"]//a[@class="css-1rni3ap eou9tt70"]')
        nombres = [i.text for i in nombres]
    
        telefono = driver.find_elements("xpath",'//p[@class="MuiTypography-root MuiTypography-body1 e230xlr0 css-xry8fx"]')
        telefono = [i.text for i in telefono] 
    
        direccion = driver.find_elements("xpath",'//p[@class="MuiTypography-root MuiTypography-body1 text-size-5 text-gray-70 css-a4lmh5"]')
        direccion = [i.text for i in direccion]
        
        #una vez tenga los datos, enviarlos a una lista
        lista_nombre.extend(nombres)
        lista_telefono.extend(telefono)
        lista_direccion.extend(direccion)
    
    #si no encuentra coincidencias continuar con el bucle
    except:
        pass
i=i+1



#crear una tabla con las listas obtenidas
df = pd.DataFrame({"Nombre": lista_nombre, "Telefono": lista_telefono, "Direccion": lista_direccion})


#guardamos la tabla en un pdf
df.to_csv('scraping3.csv')

---
Muchas gracias por la oportunidad de presentar esta prueba, he aprendido bastante durante el desarrollo de la misma.

Jesús Ángel López Rojas
---
