# Darkly 🕵️‍♂️🛡️

Si tu idea es entrar a este repo para sacar las flags estas muy equivocado!

# Indice

1. [Path/Directory traversal  📂🔓](#1---pathdirectory-traversal--)

2. [Hidden input 🕵️‍♂️🔍](#2---hidden-input-%EF%B8%8F%EF%B8%8F)

3. [Redirections 🔄🔀](#3---redirections-)

4. [Survey 📊✅](#4---survey-)

5. [Cookie Injection 🍪💉](#5---cookie-injection-)
   
6. [Stored XSS 💬⚠️](#6---stored-xss--%EF%B8%8F)

7. [Scrapping 🤖🕵️‍♂️](#7---scrapping-%EF%B8%8F%EF%B8%8F)

8. [HTTP Header Manipulation 🐦🕵️‍♂️](#8---http-header-manipulation-%EF%B8%8F%EF%B8%8F)

9. [SQL Injection Images 📸💉](#9---sql-injection-images-)

10. [SQL Injection Images 👥💉](#10---sql-injection-members-)

11. [Bruteforce 🔐⚔️](#11---bruteforce-%EF%B8%8F)

12. [Robots 🤖👮‍♂️](#12---robots-admin-%EF%B8%8F)

13. [Data URI Injection 📄🔍](#13---data-uri-injection-)

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

1. El primer paso que debemos seguir, una vez estamos dentro de la pagina web, sera observar como estan estructuradas las URLs de la pagina y navegaremos un poco por la web en busca de patrones que sugieran que la web es vulnerable a la inclusion de rutas de archivos o directorios en las URLS.

![image](https://github.com/gemartin99/Darkly/assets/66915274/d245349a-943f-4ce0-a51b-605bd3abe687)

2. Explorando por la pagina vemos que si le damos al botón de ```SIGN IN``` nos cargará la pagina con un panel de inicio de sesión y veremos que la URL de la pagina no sigue una ruta tradicional (absoluta/relativa).

![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/eb77e860-7778-4ec2-b8a7-578370322173)

Que significa esta URL❓ La parte de la URL antes del ´?´ sigue siuendo la ruta base. Lo que hay después de ´?´ son parámetros de consulta, estos parámetros proporcionan datos adicionales que son relevantes para la solicitud.

Basicamente, lo que sugiere esto es que lo que le estamos diciendo al servidor es que la pagina (page) que queremos cargar es signin (que eso sera una macro o algo por el estilo de un fichero, por ejemplo: signin.html). 

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

1. El primer paso que haremos sera abrir el codigo fuente de la pagina web y examinarlo en busca de formularios ```<form>``` ya que suelen ser lugares comunes donde se encuentran campos ocultos que podrían ser manipulables.

   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/3c6205fe-4d52-4aa9-b678-11a39a54f39f)

2. Navegando por la pagina hemos encontrado algunos formularios pero en los campos ocultos no aparece nada de valor. Hasta que nos topamos con el apartado ```Recover password```.
   
   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/bfeb403e-42fa-45bc-89ca-0fe601d6209b)

3. Si abrimos el codigo fuente podemos observar como hay un campo oculto con un valor de un correo electronico.
   
   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/477d6db6-1610-4173-bdb1-e1ce8c2db81e)
   
4. Sabiendo esto lo que haremos a continuacion sera sustituir dicho valor desde ```inspect``` para que de esta manera en vez de llegar la recuperación de contraseña al correo por defecto nos llegue al correo que nosotros especifiquemos.

  ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/69cc60e7-7e9c-4d95-a878-6a753c41a680)

5. Una vez lo hayamos modificado le haremos click al boton de ```submit``` y ya nos dara la flag del ejercicio.

   ![image](https://github.com/gemartin99/Darkly-Tutorial/assets/66915274/e36e0eee-f7f4-4777-8fbb-dd01df808c80)




### Prevención 🔒🛡️

Verificar que todos los datos recibidos, incluidos los provenientes de campos ocultos, sean válidos y autorizados antes de procesar cualquier acción.

## 3 - Redirections 🔄🔀

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 4 - Survey 📊✅

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 5 - Cookie Injection 🍪💉

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 6 - Stored XSS  💬⚠️

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 7 - Scrapping 🤖🕵️‍♂️

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 8 - HTTP Header Manipulation 🐦🕵️‍♂️

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

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

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 12 - Robots Admin 🤖👮‍♂️

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 13 - Data URI Injection 📄🔍

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

### 14 - File Upload Injection 📤📄 

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

### Prevención 🔒🛡️

