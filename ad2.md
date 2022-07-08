
# Actividad dirigida 2

Esta *actividad dirigida* es un ejercicio de programación literaria destinado a presentar todo el
procedimiento realizado a fin de cambiar el archivo **README.md** por la actividad dirigida 1 **(ad1.md)**.

A su vez, este escrito se ha desarrollado a través del editor de texto **Nano** en GitBash.

*El proceso a continuación:*

## Primera parte

Debimos convertir nuestro repositorio en una web servida desde Github. Para activar esta configuración seguimos estos pasos:
	1. Ingresar a la sección de *pages* de nuestro repositorio. En mi caso: https://github.com/nebrijas/MadelaineGuevara-web/settings/pages
        2. En el apartado de *Source* encontramos la opción de *Branch*, aquí escogemos **main** y en el folder seleccionamos **root**

Al activar estas opciones ya convertimos nuestro archivo en un HTML.


## Segunda Parte

1. Desde la web de GitHub creamos un nuevo archivo. En **Add file** seleccionamos *Create New File* y lo nombramos como **ad2.md**

2. De aquí en adelante todos los cambios se realizan en la herramienta **GitBash**. Por lo que es necesario descargarla y acceder a la terminal.

3. En la terminal, primero escribimos el comando **pwd** y ejecutamos para conocer desde qué directorio estamos trabajando.

4. Para copiar el directorio del repositorio en nuestro ordenador, ejecutamos **git clone https://github.com/nebrijas/MadelaineGuevara-web/**

5. Escribimos el comando **ls** y **enter** para verificar la ubicación de nuestro archivo clonado.

6. Luego, colocamos **cd** seguido del nombre de nuestra carpeta clonada **MadelaineGuevara-web** y **enter** para cambiar a este archivo.

7. A continuación ejecutamos **git config user.name** seguido de nuestro nombre de usuario **Madelaine Guevara** .Después, colocamos **git config user.email** seguido de nuestro correo
**madeg_12@hotmail.com** 

8. *Github* en vez de usar contraseña utiliza los "tokens". Así que es necesario ingresar a [GitHub-token](https://github.com/settings/tokens) para configurarlo. 
Aquí seleccionamos **generar nuevo token**, lo nombramos *PD2* y en expiración ponemos 60 días.

9. Copiamos el token y regresamos a *GitBash*, escribimos **echo + "(el token personal)"> ../.token** para guardar este token en un archivo 
oculto que se mantendrá en la carpeta superior del árbol.

10. A continuación, escribimos **README.md ad1.md** y ejecutamos con **enter** para copiar el contenido del *README.md* en el nuevo archivo *ad1.md*.

11. Abrimos **nano README.md** para editar el contenido de esta carpeta. Aquí pudimos modificar el titular y agregar dos enlaces; 
el primero para la *actividad dirigida 1* y el segundo para la *actividad dirigida 2*

12. Como hemos modificado y creado nuevas carpetas, al aplicar **git status** más **enter** aparecen cambios no rastreados. Por lo que escribimos **git add README.md ad1.md** 

13. Agregamos **git commit -m** para colocar el comentario de las modificaciones. 

14. Para finalizar aplicamos **git push** más **enter**. Este comando nos pide nuestro user name y el token personal. Con esto se actualiza el contenido en nuestro repositorio de GitHub
Solo queda comprobar.


   



































^G Help          ^O Write Out     ^W Where Is      ^K Cut           ^T Execute       ^C Location      M-U Undo         M-A Set Mark     M-] To Bracket   M-Q Previous     ^B Back
^X Exit          ^R Read File     ^\ Replace       ^U Paste         ^J Justify       ^/ Go To Line    M-E Redo         M-6 Copy         ^Q Where Was     M-W Next         ^F Forward
 
 
