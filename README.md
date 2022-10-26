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
Importamos las librerias que se van a utilizar

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura.PNG"/></p> 

Llamamos al archivo que contiene la tabla

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura2.PNG"/></p> 

Creamos las listas resultado de la toma de datos de las tablas linea por linea

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura3.PNG"/></p> 

Para iniciar creamos un bucle for en un rango de 16, luego de esto extraemos el texto del pdf, la variable i sera el numero de paginas que va a revisar una vez se haya completado el bucle, es decir, la variable i = 0, al final del bucle i++, al comenzar el la proxima lectura estaria validada en 1, asi hasta llegar hasta 15.

Se hizo una busqueda con expresiones regulares linea por linea. Para sacar un dato especifico se duplico la linea, ejemplo( la linea de codigo 1 y 2 son exactamente iguales, de la linea 1 saque PermitNumber, en la linea 2 lo elimino y saco Address, esto me permite crear dos listas aparte.
Este mismo proceso sucedio con las distintas lineas. 

Tambien se elaboro un dataframe para posteriormente ser conectado al resto.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura4.PNG"/></p> 

Se realizo la conexion de todos las tablas en una sola

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura5.PNG"/></p> 

Resultado final para posteriormente guardarlo en excel

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
data1.to_csv('tb1.csv')

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

Importamos las librerias que se van a utilizar

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Capturas.PNG"/></p>

Llamamos al archivo que contiene la tabla. Creamos las listas resultado de la toma de datos de las tablas linea por linea

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura11.PNG"/></p> 


Para iniciar creamos un bucle for en un rango de 16, luego de esto extraemos el texto del pdf, la variable i sera el numero de paginas que va a revisar una vez se haya completado el bucle, es decir, la variable i = 0, al final del bucle i++, al comenzar el la proxima lectura estaria validada en 1, asi hasta llegar hasta 15.

Se hizo una busqueda con expresiones regulares linea por linea. Para sacar un dato especifico se duplico la linea, ejemplo( la linea de codigo 1 y 2 son exactamente iguales, de la linea 1 saque PermitNumber, en la linea 2 lo elimino y saco Address, esto me permite crear dos listas aparte.
Este mismo proceso sucedio con las distintas lineas. 

Tambien se elaboro un dataframe para posteriormente ser conectado al resto.

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura22.PNG"/></p> 

Las busquedas de las lineas 3 y 4 obtenian duplicados, fueron eliminadas

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura33.PNG"/></p> 

se realizo la conexion de todas las tablas

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura44.PNG"/></p> 

Resultado final

<p align="center"><img src="https://github.com/Jesus2698/PruebaTecnica/blob/main/Captura55.PNG"/></p> 

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

df.to_csv('tb2.csv')

---


<h1 align="center"> SEGUNDA PRUEBA</h1>
---

# SCRAPY yellowpages

# Documentacion
