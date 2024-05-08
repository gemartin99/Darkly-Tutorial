# Darkly 🕵️‍♂️🛡️

# Indice

Ya lo haré

## 1 - Path/Directory traversal  📂🔓

### Descripción de la vulnerabilidad 💡🔍

Es una vulnerabilidad que ocurre cuando una aplicación web no valida adecuadamente las rutas de archivo proporcionadas por el usuario. Esto puede permitir que un atacante navegue a través de directorios del sistema de archivos del servidor que no deberían ser accesibles desde la aplicación.

### Riesgos asociados ⚠️💥

Acceso no autorizado a archivos sensibles. El atacante puede acceder a archivos que normalmente estarían fuera de su alcance, como archivos de configuración del sistema, archivos de contraseñas, registros de bases de datos, archivos de sesión, etc.

### Videos educacionales sobre la vulnerabilidad 🎥

Videos sobre Path/Directory reversal:

Ejemplo básico muy parecido al que veremos nosotros [AQUÍ](https://www.youtube.com/watch?v=4rv14W1PoXU)

Ejemplo básico y otras variantes sobre path traversal [AQUÍ](https://www.youtube.com/watch?v=64XIkIyCIRo=)

### Ejemplo 🔧👨‍💻

⚠️ Antes de ir con el ejemplo + solución. Si no sabes mucho sobre tecnicas de hacking web te recomiendo antes de ver como resolverlo leer atentantemente el resto de apartados y mirar los videos educacionales que te dejo y de esta manera intentar resolverlo por tu cuenta, aprenderás mucho más! ⚠️

Si has omitido mi advertencia voy explicar cada captura y intentar que aprendas! Presta atención❗👀

1. El primer paso que debemos seguir, una vez estamos dentro de la pagina web, sera observar como estan estructuradas las URLs de la pagina y navegaremos un poco por la web en busca de patrones que sugieran que la web es vulnerable a la inclusion de rutas de archivos o directorios en las URLS.

![image](https://github.com/gemartin99/Darkly/assets/66915274/d245349a-943f-4ce0-a51b-605bd3abe687)

2. 
