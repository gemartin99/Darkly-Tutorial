# Darkly 🕵️‍♂️🛡️

Si tu idea es entrar a este repo para sacar las flags estas muy equivocado! La gracia de este repositorio es poder tener una guia en caso de ir muy perdido pero la idea principal es que intentes conseguir todo por ti mismo. Si buscas la solucion y no te rompes un poco la cabeza no aprenderas nada con los CTF's ya que en una maquina real no te daran nada hecho. 

# Indice

1. [Path/Directory traversal  📂🔓](#1---pathdirectory-traversal--)

2. [Hidden input 🕵️‍♂️🔍](#2---hidden-input-%EF%B8%8F%EF%B8%8F)

3. [Open Redirection 🔄🔀](#3---open-redirection-)

4. [Survey 📊✅](#4---survey-)

5. [Cookie Tampering 🍪🛠️](#5---cookie-tampering-%EF%B8%8F)
   
6. [Stored XSS 💬⚠️](#6---stored-xss--%EF%B8%8F)

7. [Scrapping 🤖🕵️‍♂️](#7---scrapping-%EF%B8%8F%EF%B8%8F)

8. [HTTP Header Manipulation 🐦🕵️‍♂️](#8---http-header-manipulation-%EF%B8%8F%EF%B8%8F)

9. [SQL Injection Images 📸💉](#9---sql-injection-images-)

10. [SQL Injection Members 👥💉](#10---sql-injection-members-)

11. [Bruteforce 🔐⚔️](#11---bruteforce-%EF%B8%8F)

12. [Robots 🤖👮‍♂️](#12---robots-admin-%EF%B8%8F)

13. [XSS Reflected 🪟💥](#13---xss-reflected-)

14. [File Upload Injection 📤📄 ](#14---file-upload-injection-)

## 1 - Path/Directory traversal  📂🔓

### Descripción de la vulnerabilidad 💡🔍

Es una vulnerabilidad que ocurre cuando una aplicación web no valida adecuadamente las rutas de archivo proporcionadas por el usuario. Esto puede permitir que un atacante navegue a través de directorios del sistema de archivos del servidor que no deberían ser accesibles desde la aplicación.

### Riesgos asociados ⚠️💥

Acceso no autorizado a archivos sensibles. El atacante puede acceder a archivos que normalmente estarían fuera de su alcance, como archivos de configuración del sistema, archivos de contraseñas, registros de bases de datos, archivos de sesión, etc.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre Path/Directory traversal:

Ejemplo básico muy parecido al que veremos nosotros [AQUÍ](https://www.youtube.com/watch?v=4rv14W1PoXU)

Ejemplo básico y otras variantes sobre path traversal [AQUÍ](https://www.youtube.com/watch?v=64XIkIyCIRo=)

### Ejemplo 🔧👨‍💻

⚠️ Antes de ir con el ejemplo + solución. Si no sabes mucho sobre tecnicas de hacking web te recomiendo antes de ver como resolverlo leer atentantemente el resto de apartados y mirar los videos educacionales que te dejo y de esta manera intentar resolverlo por tu cuenta, aprenderás mucho más! ⚠️

Si has omitido mi advertencia voy explicar cada captura e intentar que aprendas! Presta atención❗👀

1. El primer paso que debemos seguir, una vez estamos dentro de la página web, sera observar como estan estructuradas las URLs de la página y navegaremos un poco por la web en busca de patrones que sugieran que la web es vulnerable a la inclusion de rutas de archivos o directorios en las URLS.

![image](https://github.com/gemartin99/Darkly/assets/66915274/d245349a-943f-4ce0-a51b-605bd3abe687)

2. Explorando por la página vemos que si le damos al botón de ```SIGN IN``` nos cargará la página con un panel de inicio de sesión y veremos que la URL de la página no sigue una ruta tradicional (absoluta/relativa).

![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/eb77e860-7778-4ec2-b8a7-578370322173)

Que significa esta URL❓ La parte de la URL antes del ´?´ sigue siuendo la ruta base. Lo que hay después de ´?´ son parámetros de consulta, estos parámetros proporcionan datos adicionales que son relevantes para la solicitud.

Basicamente, lo que sugiere esto es que lo que le estamos diciendo al servidor es que la página (page) que queremos cargar es signin (que eso sera una macro o algo por el estilo de un fichero, por ejemplo: signin.html). 

3. Una vez conocemos como funciona la estructura de URL y sabiendo que el parametro ```page``` puede ser manipulado sustituiremos la ruta de ```signin``` por la ruta a un fichero que nos interese. Lo primero que haremos sera poner varias veces ```../``` ya que eso significa que retrocederemos en los directorios del servidor , de esta manera llegaremos al directorio raiz y una vez ubicados en el directorio raiz le pediremos que cargue el fichero ```/etc/passwd``` ya que es un fichero muy valioso en el cual podemos ver nombres de usuarios, etc.

![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/51f53cf8-c5f2-4a2e-91f2-296e67cafc07)

Una vez hemos hecho la consulta el servidor, en caso de ser vulnerable, nos devolveria el contenido de ```/etc/passwd```. En este caso al ser un entorno controlado conseguimos la flag del ejercicio. 

### Prevención 🔒🛡️

Los puntos mas importantes para prevenir el ataque sería validar y sanitizar adecuadamente las entradas de usuario, limitar los permisos de acceso a archivos y directorios.

## 2 - Hidden input 🕵️‍♂️🔍

### Descripción de la vulnerabilidad 💡🔍

Esta vulnerabilidad consiste en poder modificar valores en campos ocultos (<input type="hidden">) en formularios web para alterar su comportamiento. Esto podría incluir cambiar privilegios de usuario, enviar datos falsos o manipular el estado de la aplicación.

### Riesgos asociados ⚠️💥

Elevación de privilegios, donde un atacante cambia el estado de un usuario de "user" a "admin", otorgándole control administrativo completo sobre la aplicación. Además, la manipulación de campos ocultos podría facilitar la modificación de datos sensibles como precios de productos, contenido de mensajes u otra información crítica almacenada en la aplicación.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre hidden input/campo oculto manipulable:

Ejemplo básico muy parecido al que veremos nosotros, empezar a ver desde min 4:13 🕦 [AQUÍ](https://www.youtube.com/watch?v=iP3cA6MBjwQ)

### Ejemplo 🔧👨‍💻

1. El primer paso que haremos sera abrir el codigo fuente de la página web y examinarlo en busca de formularios ```<form>``` ya que suelen ser lugares comunes donde se encuentran campos ocultos que podrían ser manipulables.

   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/3c6205fe-4d52-4aa9-b678-11a39a54f39f)

2. Navegando por la página hemos encontrado algunos formularios pero en los campos ocultos no aparece nada de valor. Hasta que nos topamos con el apartado ```Recover password```.
   
   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/bfeb403e-42fa-45bc-89ca-0fe601d6209b)

3. Si abrimos el codigo fuente podemos observar como hay un campo oculto con un valor de un correo electronico.
   
   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/477d6db6-1610-4173-bdb1-e1ce8c2db81e)
   
4. Sabiendo esto lo que haremos a continuacion sera sustituir dicho valor desde ```inspect``` para que de esta manera en vez de llegar la recuperación de contraseña al correo por defecto nos llegue al correo que nosotros especifiquemos.

  ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/69cc60e7-7e9c-4d95-a878-6a753c41a680)

5. Una vez lo hayamos modificado le haremos click al boton de ```submit``` y ya nos dara la flag del ejercicio.

   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/e36e0eee-f7f4-4777-8fbb-dd01df808c80)




### Prevención 🔒🛡️

Verificar que todos los datos recibidos, incluidos los provenientes de campos ocultos, sean válidos y autorizados antes de procesar cualquier acción.

## 3 - Open Redirection 🔄🔀

### Descripción de la vulnerabilidad 💡🔍

Esta vulnerabilidad ocurre cuando una aplicación web permite a los usuarios ser redirigidos a URLs externas sin validar su seguridad. Esto permite a un atacante manipular la URL de redirección, llevando a las víctimas a sitios maliciosos que pueden estar diseñados para el phishing o la distribución de malware.

### Riesgos asociados ⚠️💥

- Phishing: Los usuarios pueden ser engañados para que ingresen información confidencial en sitios fraudulentos.
- Distribución de Malware: Los atacantes pueden redirigir a los usuarios a sitios que descargan software malicioso.
- Pérdida de confianza: La reputación de la aplicación se puede ver gravemente afectada.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre Open Redirect 🔄🔀:

Explicación de **Open Redirect** [AQUÍ](https://www.youtube.com/watch?v=iP3cA6MBjwQ)

Ejemplo de uso [AQUI](https://www.youtube.com/watch?v=UJRytPAvQks&list=PLy8t3TIwSh3tMEAjyldNNbjtTmm5G7kFl&index=5)

### Ejemplo 🔧👨‍💻

   1. Al inspeccionar la página, observamos que la aplicación utiliza la estructura                                 ```index.php?page=redirect&site=whatever``` donde el valor ```site``` no se valida correctamente. Esto permite que un atacante manipule el parámetro ```site``` para redirigir a los usuarios a cualquier URL, incluidas aquellas potencialmente maliciosas.

![image](https://github.com/user-attachments/assets/6eeca6f5-70e8-4b77-b19c-05df160e95d1)

   2. Modificamos el valor de ```site``` por el de otra página.

![image](https://github.com/user-attachments/assets/0179ebe6-805e-4aa7-99e4-af856f502f0a)

   3. Finalmente, al hacer clic en el logo que hemos modificado con la redirección, deberíamos obtener la flag.

![image](https://github.com/user-attachments/assets/85be0a65-e011-4c18-b997-955e31f27780)


### Prevención 🔒🛡️

Validar las URL's, permitiendo solo sitios de confianza o usar whitelists de dominios permitidos.

### 4 - Survey 📊✅

### Descripción de la vulnerabilidad 💡🔍

La aplicación no valida correctamente los valores de entrada enviados a través de un formulario POST. Pudiendo modificar los valores de dicho formulario. Esta vulnerabilidad es conocida como **Client-Side Validation Bypass**.

### Riesgos asociados ⚠️💥

Fraude financiero, acceso no autorizado a servicios o productos y integridad de los datos comprometida.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre **Client-Side Validation Bypass**💰🔧:

Ejemplo de uso similiar [AQUI](https://www.youtube.com/watch?v=EYexI_JikXU)

### Ejemplo 🔧👨‍💻

   1. En este caso, si entramos en la pestaña survey de la página, vemos la siguiente encuesta.

![image](https://github.com/user-attachments/assets/7cb59bbb-64d8-47f5-9a61-a290a213fda7)

   2. Al inspeccionar la página o revisar el código fuente, notamos que la encuesta no valida correctamente los valores de entrada enviados a través de un formulario POST. El hecho de que se pueda enviar el parámetro ```value=``` sin que la aplicación verifique si es un valor válido indica un fallo en la lógica de validación.

![image](https://github.com/user-attachments/assets/c4061e40-45cc-483c-8416-0500d30eccdd)

   3. Modificamos el campo value por el valor que queramos. En este caso, si escogemos la opción 10 de ol, el valor será ```9999999999999```.

![image](https://github.com/user-attachments/assets/b8dc6f65-0fd5-47b7-a94c-fbc273f17041)

   4. Hacemos clic en la opción que hemos modificado.

![image](https://github.com/user-attachments/assets/3bf90c10-1c16-4135-859d-e8773183a013)

   5. Esto nos devuelve la flag del ejercicio.

![image](https://github.com/user-attachments/assets/edb39af3-ccc2-41aa-813a-0832aedeeeb1)

### Prevención 🔒🛡️

Validación de entradas, sanitizando todo correctamente. Sobretodo, implementandolo en el backend ya que el frontend puede ser manipulado fácilmente por un atacante.

### 5 - Cookie tampering 🍪🛠️

### Descripción de la vulnerabilidad 💡🔍

El atacante aprovecha la capacidad de modificar el contenido de las cookies desde el lado del cliente, usualmente mediante herramientas como la consola del navegador, para alterar su valor y engañar al servidor haciéndole creer que tiene ciertos privilegios o estado de autenticación.

### Riesgos asociados ⚠️💥

Suplantación de identidad y por lo tanto acceso no autorizado a recursos.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre Cookie Manipulation 🍪:

Ejemplo básico sobre manipulación de Cookies muy parecido al que veremos nosotros, empezar a ver desde min 6:30 🕦 [AQUI](https://www.youtube.com/watch?v=fbZpsHMgNdk&t=402s) 

### Ejemplo 🔧👨‍💻

   1. Inspeccionamos la página y vemos nuestra cookie de sesión escribiendo el comando ```document.cookie``` en la          consola del navegador. Observamos el ID de sesión y el valor de la cookie. Notamos que contiene ```I_am_admin=```, pero no entendemos el valor asignado, así que buscamos la forma de desencriptarlo.

![image](https://github.com/user-attachments/assets/7f2a5054-f458-4101-b435-33438633016e)

   2. Resulta que el valor es ```false```. Lo que indica que nuestra cookie tiene el valor de ```I_am_admin=false```.  Lo que haremos será cambiar el valor de la cookie a ```true```.

![image](https://github.com/user-attachments/assets/3fbaa5e7-e5b7-46e1-9134-78da2a8b3c1f)

   3. Cogemos la cadena ```true``` y la encriptamos en md5, copiamos ese valor.

![image](https://github.com/user-attachments/assets/7c5c3b26-8441-4027-ac82-5a8d69d7d677)

   4. Modificamos el valor de nuestra cookie de sesión para que sea el ```true``` encriptado.

![image](https://github.com/user-attachments/assets/9bede95b-7a4b-4908-bbd2-fa25ea12ad51)

   5. Al refrescar la página, ya seremos administradores, lo que nos dará acceso a la flag correspondiente.

![image](https://github.com/user-attachments/assets/eea79633-8a9e-4a6f-bd3b-7e1edf070d5b)



### Prevención 🔒🛡️

Tener firmado y cifrado de cookies, monitoreo para detectar cualquier actividad sospechosa, una validación y autenticación adecuada.

### 6 - Stored XSS  💬⚠️

### Descripción de la vulnerabilidad 💡🔍

Permite a un atacante ejecutar scripts maliciosos en el navegador de otros usuarios. Ocurre cuando los datos enviados por un usuario a través de formularios o mensajes se guardan en la base de datos sin sanitizar adecuadamente, y luego se muestran a otros usuarios en alguna parte de la aplicación.

### Riesgos asociados ⚠️💥

Ejecución de código malicioso, robo de cookies, redirección a sitios maliciosos.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre como realizar ataques XSS 💬⚠️:

Qué es XSS❓ **Explicado PASO a PASO** [AQUI](https://www.youtube.com/watch?v=_IO8Sm-tWPs)

Demostración y **RIESGOS** de esta **VULNERABILIDAD** [AQUI](https://www.youtube.com/watch?v=lG2XpAgy0Ns)

### Ejemplo 🔧👨‍💻

   1. En la sección de ```Feedback```, observamos que al publicar un mensaje, este se refleja directamente en la página web. Lo que intentaremos hacer es enviar un script para comprobar si el input está mal sanitizado y si, en lugar de tratarlo como texto plano, el servidor lo ejecuta. En este caso particular, al ser un CTF, si el mensaje contiene la palabra ```script```, automáticamente obtenemos la flag, por lo que no es necesario un payload más complejo.

![image](https://github.com/user-attachments/assets/06ab1bd6-5fc3-4d89-9dca-4bf861cffc51)

   2. Una vez le damos a sign ya nos aparece la flag.

![image](https://github.com/user-attachments/assets/7da119c5-479e-4313-81ad-12415e846fd1)


### Prevención 🔒🛡️

Validar y sanitizar todos los datos de entrada para eliminar o escapar cualquier contenido malicioso y codificar adecuadamente todos los datos antes de mostrarlos en la interfaz de usuario

### 7 - Scrapping 🤖🕵️‍♂️

### Descripción de la vulnerabilidad 💡🔍

El web scraping es la práctica de extraer información de sitios web de manera automatizada. Si la aplicamos al ejemplo que nos da esta maquina veremos que tendremos que hacer una búsqueda exhaustiva a través de directorios y archivos para buscar la flag en uno de ellos.

### Riesgos asociados ⚠️💥

Exposición de información sensible y carga no intencionada del servidor.

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

Utilizar técnicas como CAPTCHA, límites de tasa y protecciones basadas en IP.

### 8 - HTTP Header Manipulation 🐦🕵️‍♂️

### Descripción de la vulnerabilidad 💡🔍

Es una técnica en la que un atacante modifica los encabezados de las solicitudes HTTP para engañar a un servidor web. Esto puede permitir a los atacantes cambiar el comportamiento del servidor, como el manejo de sesiones, la autenticación y las redirecciones.

### Riesgos asociados ⚠️💥

Ejecutar ataques de cross-site scripting (XSS), phishing, y la exposición de información sensible. Los atacantes pueden suplantar a usuarios legítimos, eludir controles de seguridad o redirigir tráfico a sitios maliciosos. 

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre como realizar ataques de HTTP Header Manipulation:

Estos videos educativos son útiles para comprender el concepto general de HTTP Header Manipulation, pero no siempre representan un ejemplo exacto de cómo resolver nuestro ejercicio específico. En el video, se muestra la manipulación del encabezado **Host**, ya que en el entorno del autor, el servidor no valida adecuadamente ese campo. En nuestro caso, la vulnerabilidad radica en la falta de validación de los encabezados **User-Agent** y **Referer**, que son los que necesitamos explotar para lograr la flag.

Explotando vulnerabilidades en la cabecera HTTP Host [AQUÍ](https://www.youtube.com/watch?v=A8hmKWQrp5Q)

### Ejemplo 🔧👨‍💻

   1. Si nos desplazamos hasta el final de la página web, encontraremos una redirección.

![image](https://github.com/user-attachments/assets/9515e083-1500-4735-b33c-af7032182d6c)

   2. Esta nos lleva a una página donde, de inmediato, comienza a sonar la canción de "Albatroz" y aparece un texto extraído de Wikipedia. Es, cuanto menos, una página peculiar. Por lo tanto, procederemos a inspeccionarla y revisar su código fuente.

![image](https://github.com/user-attachments/assets/c0e05b2b-2f88-4627-b46e-a35669f4f11f)

   3. Si examinamos bien en el codigo fuente de la página encontraremos dos comentarios muy interesantes. Uno nos indica que debemos acceder desde la URL ```https://www.nsa.gov/``` y el otro nos sugiere usar el navegador con el agente de usuario ```ft_bornToSec``` 

![image](https://github.com/user-attachments/assets/114e94fb-07e7-4dd9-a4ca-24ff2622c3d1)

![image](https://github.com/user-attachments/assets/7e0cf2f1-a546-4ff3-8943-c57410dc138b)

   4. Genial, sabiendo esto, vamos a hacer una solicitud curl modificando los parámetros que nos pide. Con el flag -e especificaremos un encabezado **Referer**, engañando al sistema para que crea que la solicitud proviene de un lugar de confianza, en este caso, ```https://www.nsa.gov/```. Con el flag -A estamos indicando que el **User-Agent** que se enviará con la solicitud es ```ft_bornToSec```, simulando el navegador. Por último, indicamos sobre qué página queremos hacer esa petición, y el resultado del comando se almacenará en un fichero llamado file.txt.

![image](https://github.com/user-attachments/assets/0f8a807f-0f19-42f0-9ed8-c6edaa8be4cf)

   5. Una vez realizada la solicitud, el contenido HTML se habrá almacenado en el archivo. A continuación, utilizaremos ```grep``` para buscar la flag dentro del archivo.

![image](https://github.com/user-attachments/assets/eb896614-e416-43c6-802b-941cf087c2c0)


### Prevención 🔒🛡️

Validación y sanitización de encabezados. Utilizar conexiones seguras HTTPS que van cifradas.

### 9 - SQL Injection Images 📸💉

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 10 - SQL Injection Members 👥💉

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 11 - Bruteforce 🔐⚔️

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre como realizar ataques de Fuerza Bruta con ```Hydra```🐍:

Como utilizar Hydra paso a paso [AQUÍ](https://www.youtube.com/watch?v=rvme2kE8-jY)

Como usar Hydra en Panel de LOGIN WEB [AQUÍ](https://www.youtube.com/watch?v=3qN-DmxpokM)

### Ejemplo 🔧👨‍💻

![image](https://github.com/user-attachments/assets/c60f66cd-d9f9-4577-a9b9-0fa018d232a7)

![image](https://github.com/user-attachments/assets/4cb9bcab-059f-41b5-bd86-74b56bca05ee)

![image](https://github.com/user-attachments/assets/813a343e-e232-4579-8ad8-bf5f72856b41)

![image](https://github.com/user-attachments/assets/c99cc41b-b6c5-4991-9c6c-c595d80820a5)

![image](https://github.com/user-attachments/assets/ae2bf3ad-96c3-4840-900b-73b8486327a8)


![image](https://github.com/user-attachments/assets/3193fd41-9fb2-490c-81ee-5a94687bcf3c)


### Prevención 🔒🛡️

### 12 - Robots Admin 🤖👮‍♂️

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

   1. Una de las primeras tareas al buscar vulnerabilidades en una página web es identificar directorios ocultos. Para ello, utilizaremos la herramienta gobuster junto con un diccionario de directorios, lo que nos permitirá probar diferentes rutas a partir de la raíz del sitio.

![image](https://github.com/user-attachments/assets/a9ffb846-2f56-4a78-b74c-305a2dc4286b)

   2. Al ingresar en las distintas rutas descubiertas, obtendremos diferentes tipos de información. Por ejemplo, al acceder a ```admin/```, observamos que hay un panel de inicio de sesión.

![image](https://github.com/user-attachments/assets/0e078b86-aba2-451a-bbf7-5bf2c1c655cc)

   3. En la ruta ```whatever/```, encontramos un archivo. Procedemos a descargarlo para analizar su contenido.

![image](https://github.com/user-attachments/assets/8172c081-c7a8-476f-b77c-9cdf08c18b04)

   4. Al visualizar el contenido del archivo descargado, se revela información importante: aparece el usuario ```root``` junto a lo que parece ser una contraseña. Intentamos ingresar estas credenciales en el panel de inicio de sesión de ```admin/```, pero el intento resulta infructuoso. Es posible que la contraseña esté encriptada.

![image](https://github.com/user-attachments/assets/5cd6f1a0-b976-4714-97b5-d23e0adb0f8f)

   5. Seleccionamos el texto encriptado y buscamos una página que permita desencriptar MD5. Introducimos la cadena en la herramienta de desencriptación, la cual nos indica que la contraseña es ```qwerty123@```.

![image](https://github.com/user-attachments/assets/78d5701e-d5e0-4bee-a009-c0dcd1448c4d)

   6.  Ahora, intentamos nuevamente iniciar sesión con las credenciales:
      
         **Usuario**: ```root```
       
         **Contraseña**: ```qwerty123@```     

![image](https://github.com/user-attachments/assets/7c043129-f857-4a1d-8685-70499f22c71c)

   7. Al ingresar correctamente, logramos acceder y conseguimos la flag correspondiente.

![image](https://github.com/user-attachments/assets/97fa5b53-8e9b-43ca-ba4b-5115b26e5a4b)

### Prevención 🔒🛡️

### 13 - XSS Reflected 🪟💥

### Descripción de la vulnerabilidad 💡🔍

Es una vulnerabilidad de seguridad que permite a un atacante inyectar scripts maliciosos en una página web que es ejecutada por el navegador de un usuario. Esto ocurre cuando una aplicación web incluye datos proporcionados por el usuario en su respuesta sin una adecuada validación o escape.

### Riesgos asociados ⚠️💥

Robo de datos, como cookies de sesión o información sensible del usuario, phising, redirección maliciosa, daño a la reputación.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre hidden input/campo oculto manipulable:

Qué es y como funciona **XSS Reclected❓** [AQUI](https://www.youtube.com/watch?v=whazLcsPcy0)

Ejemplo básico parecido al que veremos nosotros [AQUÍ](https://www.youtube.com/watch?v=P3h2lr1D1N8)

### Ejemplo 🔧👨‍💻

   1. Si clickamos sobre la imagen nsa se nos abre la siguiente pagina.

![image](https://github.com/user-attachments/assets/273c8319-6028-4ef0-88c1-1dd42d97823e)

   2. La URL contiene parámetros, en este caso, ```page``` y ```src```. Las aplicaciones web a menudo utilizan parámetros de la URL para generar contenido dinámico. Si el parámetro ```src``` se usa directamente en el HTML sin ser validado o sanitizado, puede ser un punto de inyección. Probamos asignar un payload de XSS directamente a la URL, pero sin éxito.

![image](https://github.com/user-attachments/assets/09fab0a7-b310-43df-b416-c835a6072bbd)

   3. Como no hemos tenido éxito poniendolo a lo bruto, utilizaremos el esquema de URI que permite incrustar datos directamente en una página web o en un archivo, en lugar de enlazarlos desde una ubicación externa. Esto se utiliza comúnmente para incluir imágenes, archivos de texto o cualquier otro tipo de datos directamente en el código HTML o CSS. La estructura básica de una DATA URI es la siguiente: data:[tipo_mime][;base64], [contenido]. Sabiendo esto vamos a codificar el script en base64 para intentar colarlo.

![image](https://github.com/user-attachments/assets/9e69b278-9864-41cf-9ef4-773c0a154f62)

   4. Modificamos la URL siguiendo la estructura básica de una DATA URI para asi incluir el script codificado.
 ```http://10.11.250.221/index.php?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgnZGFtZSBsYSBmbGFnJyk8L3NjcmlwdD4=```. Esta vez si, con éxito.

![image](https://github.com/user-attachments/assets/35a840e2-f201-41e5-97ef-d85e02a47e38)


### Prevención 🔒🛡️

Sanitizar y validar todas las entradas del usuario antes de ser procesadas o mostradas y implementar políticas que limiten los recursos que pueden ser cargados en la aplicación web.

### 14 - File Upload Injection 📤📄 

### Descripción de la vulnerabilidad 💡🔍

La capacidad de un atacante para cargar archivos maliciosos a un servidor a través de un formulario de carga de archivos. Esta vulnerabilidad ocurre cuando una aplicación web no valida adecuadamente los archivos que los usuarios intentan cargar, lo que permite que se suban archivos que pueden ser utilizados para ejecutar código malicioso, obtener acceso no autorizado o comprometer la seguridad del servidor.

### Riesgos asociados ⚠️💥

Ejecución de código malicioso, acceso no autorizado, filtración de datos, daño a la reputación.

### Videos educacionales sobre la vulnerabilidad 🎥

▶️ Videos sobre como realizar **File Upload Injection** 📤📄:

Ejemplo de uso buenísimo. No utilizaremos la herramienta Burpsuite pero nos sirve para entender como funciona. [AQUI](https://www.youtube.com/watch?v=B7L0epI7oYI)

### Ejemplo 🔧👨‍💻

   1. Si nos vamos a la pestaña de ```Add image``` vemos que podemos subir un archivo.

![image](https://github.com/user-attachments/assets/148e412c-9e37-43fd-8edc-efd642d2c242)

   2. Creamos un fichero malicioso, en este caso no hara nada pero la idea es crear un fichero con contenido malicioso en ```.php```

![image](https://github.com/user-attachments/assets/c1e894b1-b97b-41f4-8cd9-dc2ed5536e41)

   3. Falseamos una petición para que el servidor crea que esta recibiendo un fichero ```.jpeg``` cuando realmente estamos subiendo el fichero malicioso ```.php```. La salida de este comando se hara en el fichero tmp.txt. Despues lo que           haremos sera hacer cat al fichero y grep para buscar la flag.

![image](https://github.com/user-attachments/assets/9865b422-1d82-420e-b493-52d96b18fcd8)



### Prevención 🔒🛡️

Implementar validaciones rigurosas para los archivos cargados. Esto incluye verificar la extensión del archivo, el tipo MIME y el contenido real del archivo para asegurarse de que es seguro.


<br>
<br>
<br>

#
# Este tutorial ha llevado mucho trabajo, si crees que te ha sido útil agradeceria mucho starred 🌟 para que así se comparta y pueda ayudar a más estudiantes 👨🏻‍🎓❤️
<br>
<br>
<br>


# Contacto 📥

### Contacta conmigo si crees que puedo mejorar el tutorial! Puede ayudar a futuros estudiantes! 😁

◦ Email: gemartin@student.42barcelona.com

◦ Linkedin: https://www.linkedin.com/in/gemartin99/

# Quizás pueda interesarte!

### - Para ver mi progresion en el common core 42 ↙️

[AQUÍ](https://github.com/gemartin99/42cursus)

### - Mi perfil en la intranet de 42 ↙️
[AQUÍ](https://profile.intra.42.fr/users/gemartin)
