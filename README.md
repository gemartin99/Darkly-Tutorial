# Darkly 🕵️‍♂️🛡️

Si tu idea es entrar a este repo para sacar las flags estas muy equivocado!

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

10. [SQL Injection Images 👥💉](#10---sql-injection-members-)

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

## 3 - Open Redirection 🔄🔀

### Descripción de la vulnerabilidad 💡🔍

Esta vulnerabilidad ocurre cuando una aplicación web permite a los usuarios ser redirigidos a URLs externas sin validar su seguridad. Esto permite a un atacante manipular la URL de redirección, llevando a las víctimas a sitios maliciosos que pueden estar diseñados para el phishing o la distribución de malware.

### Riesgos asociados ⚠️💥

- Phishing: Los usuarios pueden ser engañados para que ingresen información confidencial en sitios fraudulentos.
- Distribución de Malware: Los atacantes pueden redirigir a los usuarios a sitios que descargan software malicioso.
- Pérdida de confianza: La reputación de la aplicación se puede ver gravemente afectada.

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

Si hacemos un inspeccionar de la pagina vemos que la aplicación utiliza la estructura ```index.php?page=redirect&site=whatever``` donde el valor de ```site``` no se valida adecuadamente. Esto significa que un atacante puede manipular el valor del parámetro site para redirigir a los usuarios a cualquier URL, incluyendo sitios potencialmente maliciosos.

![image](https://github.com/user-attachments/assets/6eeca6f5-70e8-4b77-b19c-05df160e95d1)

Modificamos el valor de site por el de otra pagina.

![image](https://github.com/user-attachments/assets/0179ebe6-805e-4aa7-99e4-af856f502f0a)

Pinchamos sobre el logo que hayamos modificado la redirección y ya nos debe dar la flag.

![image](https://github.com/user-attachments/assets/85be0a65-e011-4c18-b997-955e31f27780)


### Prevención 🔒🛡️

Validar las URL's, permitiendo solo sitios de confianza o usar whitelists de dominios permitidos.

### 4 - Survey 📊✅

### Descripción de la vulnerabilidad 💡🔍

La aplicación no valida correctamente los valores de entrada enviados a través de un formulario POST. Pudiendo modificar los valores de dicho formulario.

### Riesgos asociados ⚠️💥

Acceso no autorizado por parte del atacante a funcionalidades o datos restringidos. El caso mas claro seria poder cambiar el valor a productos en venta.

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

En este caso, si entramos en la pestaña de survey de la pagina vemos una encuesta.

![image](https://github.com/user-attachments/assets/7cb59bbb-64d8-47f5-9a61-a290a213fda7)

Si hacemos un inspeccionar de la pagina y analizamos un poco la encuesta parece que la aplicación no valida correctamente los valores de entrada enviados a través de un formulario POST. El hecho de que se pueda enviar el parámetro ```value=42``` sin que la aplicación verifique si ese valor es válido o no sugiere que hay un fallo en la lógica de validación.

![image](https://github.com/user-attachments/assets/c4061e40-45cc-483c-8416-0500d30eccdd)

Modificamos el campo value por el valor que queramos. En este caso si escogemos la opcion 10 de ol el valor sera 9999999999999.

![image](https://github.com/user-attachments/assets/b8dc6f65-0fd5-47b7-a94c-fbc273f17041)

Seleccionamos esa opción.

![image](https://github.com/user-attachments/assets/3bf90c10-1c16-4135-859d-e8773183a013)

Nos devuelve la flag del ejercicio.

![image](https://github.com/user-attachments/assets/edb39af3-ccc2-41aa-813a-0832aedeeeb1)

### Prevención 🔒🛡️

Validación de entradas, sanitizando todo correctamente. Sobretodo, implementandolo en el backend ya que el frontend puede ser manipulado fácilmente por un atacante.

### 5 - Cookie tampering 🍪🛠️

### Descripción de la vulnerabilidad 💡🔍

El atacante aprovecha la capacidad de modificar el contenido de las cookies desde el lado del cliente, usualmente mediante herramientas como la consola del navegador, para alterar su valor y engañar al servidor haciéndole creer que tiene ciertos privilegios o estado de autenticación.

### Riesgos asociados ⚠️💥

Suplantación de identidad y por lo tanto acceso no autorizado a recursos.

### Videos educacionales sobre la vulnerabilidad 🎥

https://www.youtube.com/watch?v=fbZpsHMgNdk&t=402s

### Ejemplo 🔧👨‍💻

Inspeccionamos la pagina y vemos nuestra cookie de sesion escribiendo el comando ```document.cookie``` en la consola del navegador. Lo que vemos es ver el id de sesion y el valor de la cookie. Lo que podemos entender es que pone ```I_am_admin=``` el valor no entendemos que pone, asi buscamos como desencriptarlo.

![image](https://github.com/user-attachments/assets/7f2a5054-f458-4101-b435-33438633016e)

Resulta que el valor es ```false```. Es decir nuestra cookie vale false y el sesion id es I_am_admin=false. Pues lo que haremos sera cambiar el valor de la cookie y buscar que ponga true.

![image](https://github.com/user-attachments/assets/3fbaa5e7-e5b7-46e1-9134-78da2a8b3c1f)

Cogemos la string ```true``` y la encriptamos en md5, copiamos ese valor.

![image](https://github.com/user-attachments/assets/7c5c3b26-8441-4027-ac82-5a8d69d7d677)

Modificamos el valor de nuestra cookie de sesion al true encriptado.

![image](https://github.com/user-attachments/assets/9bede95b-7a4b-4908-bbd2-fa25ea12ad51)

Si refrescamos la pagina ya seremos admins asique nos da la flag correspondiente.

![image](https://github.com/user-attachments/assets/eea79633-8a9e-4a6f-bd3b-7e1edf070d5b)



### Prevención 🔒🛡️

Tener firmado y cifrado de cookies, monitoreo para detectar cualquier actividad sospechosa, una validación y autenticación adecuada.

### 6 - Stored XSS  💬⚠️

### Descripción de la vulnerabilidad 💡🔍

Permite a un atacante ejecutar scripts maliciosos en el navegador de otros usuarios. Ocurre cuando los datos enviados por un usuario a través de formularios o mensajes se guardan en la base de datos sin sanitizar adecuadamente, y luego se muestran a otros usuarios en alguna parte de la aplicación.

### Riesgos asociados ⚠️💥

Ejecución de código malicioso, robo de cookies, redirección a sitios maliciosos.

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

Lo que vemos en el apartado de feedback es que si pones un mensaje y lo publicas ese mismo texto se ve en la pagina web. Lo que vamos a intentar es pasarle algun script para ver si esta mal sanitizado y no lo interpreta como texto plano. En este caso al ser un CTF si en el mensaje aparece la palabra script ya te da la flag, nos ahorramos el payload.

![image](https://github.com/user-attachments/assets/06ab1bd6-5fc3-4d89-9dca-4bf861cffc51)

Una vez le damos a sign ya nos aparece la flag.

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

Ocurre cuando un atacante modifica o interfiere con los encabezados HTTP de una solicitud o respuesta

### Riesgos asociados ⚠️💥

Falsificación de identidad y exposición de información sensible

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

Si vamos al final de toda la web veremos una redireccion al final.

![image](https://github.com/user-attachments/assets/9515e083-1500-4735-b33c-af7032182d6c)

Nos lleva a esta pagina que ya de primeras nos comienza a sonar la cancion de albatroz y un texto de la wikipedia. Pagina rara cuanto menos. Asique vamos a inspeccionarla y ver el codigo fuente.

![image](https://github.com/user-attachments/assets/c0e05b2b-2f88-4627-b46e-a35669f4f11f)

Si buscamos bien en el codigo fuente de la pagina encontraremos dos comentarios muy interesantes. Que tenimos que venir de ```https://www.nsa.gov/``` y que usemos el navegador ```ft_bornToSec``` 

![image](https://github.com/user-attachments/assets/114e94fb-07e7-4dd9-a4ca-24ff2622c3d1)

![image](https://github.com/user-attachments/assets/7e0cf2f1-a546-4ff3-8943-c57410dc138b)

Genial, pues sabiendo esto vamos a hacer una solicitud curl modificando los parametros que nos pide. Con el flag -e especificaremos un encabezado Referer, engañando al sistema haciendo que crea que la solicitud proviene de un lugar de confianza. Con el flag -A estamos indicando que el User-Agent que se enviará con la solicitud es ```ft_bornToSec```. Simulando el navegador. Por ultimo indicamos sobre que pagina queremos hacer esa peticion y el resultado del comando nos lo almacene en un fichero llamado file.txt

![image](https://github.com/user-attachments/assets/0f8a807f-0f19-42f0-9ed8-c6edaa8be4cf)

Una vez se ha realizado la solicitud se nos habra almacenado el contenido html en el fichero y haremos grep en busca de la flag.

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

![image](https://github.com/user-attachments/assets/a9ffb846-2f56-4a78-b74c-305a2dc4286b)

![image](https://github.com/user-attachments/assets/0e078b86-aba2-451a-bbf7-5bf2c1c655cc)

![image](https://github.com/user-attachments/assets/8172c081-c7a8-476f-b77c-9cdf08c18b04)

![image](https://github.com/user-attachments/assets/5cd6f1a0-b976-4714-97b5-d23e0adb0f8f)

![image](https://github.com/user-attachments/assets/78d5701e-d5e0-4bee-a009-c0dcd1448c4d)

![image](https://github.com/user-attachments/assets/7c043129-f857-4a1d-8685-70499f22c71c)

![image](https://github.com/user-attachments/assets/97fa5b53-8e9b-43ca-ba4b-5115b26e5a4b)

### Prevención 🔒🛡️

### 13 - XSS Reflected 🪟💥

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

![image](https://github.com/user-attachments/assets/273c8319-6028-4ef0-88c1-1dd42d97823e)

![image](https://github.com/user-attachments/assets/9e69b278-9864-41cf-9ef4-773c0a154f62)

```http://10.11.250.221/index.php?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgnZGFtZSBsYSBmbGFnJyk8L3NjcmlwdD4=```

![image](https://github.com/user-attachments/assets/35a840e2-f201-41e5-97ef-d85e02a47e38)


### Prevención 🔒🛡️

### 14 - File Upload Injection 📤📄 

### Descripción de la vulnerabilidad 💡🔍

### Riesgos asociados ⚠️💥

### Videos educacionales sobre la vulnerabilidad 🎥

### Ejemplo 🔧👨‍💻

![image](https://github.com/user-attachments/assets/c1e894b1-b97b-41f4-8cd9-dc2ed5536e41)

![image](https://github.com/user-attachments/assets/9865b422-1d82-420e-b493-52d96b18fcd8)



### Prevención 🔒🛡️





# Este tutorial ha llevado mucho trabajo, si crees que te ha sido útil agradeceria mucho starred 🌟 para que así se comparta y pueda ayudar a más estudiantes 👨🏻‍🎓❤️

# Contacto 📥

### Contacta conmigo si crees que puedo mejorar el tutorial! Puede ayudar a futuros estudiantes! 😁

◦ Email: gemartin@student.42barcelona.com

◦ Linkedin: https://www.linkedin.com/in/gemartin99/

# Quizás pueda interesarte!

### - Para ver mi progresion en el common core 42 ↙️

[AQUÍ](https://github.com/gemartin99/42cursus)

### - Mi perfil en la intranet de 42 ↙️
[AQUÍ](https://profile.intra.42.fr/users/gemartin)
