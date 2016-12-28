# Git #

### Recomendaciones de Uso ###
- No es necesario hacer push constantemente pero sin con regularidad, sobre todo cuando se requiere compartir los cambios locales a otro miembro del equipo de desarrollo.

### Operaciones Locales ###
Git tiene tres estados en los que se pueden encontrar tus archivos. 
 - **Confirmado** (committed): Significa que los datos están almacenados de manera segura en tu base de datos local.
 - **Modificado** (modified): Modificado significa que has modificado el archivo pero todavía no lo has confirmado a tu base de datos.
 - **Preparado** (staged): Significa que has marcado un archivo modificado en su versión actual para que vaya en tu próxima confirmación.

![](http://i.imgur.com/0Bqn9C0.png)


### Flujo de Trabajo ###
El directorio de Git es donde Git almacena los metadatos y la base de datos de objetos para tu proyecto. Es lo que se copia cuando clonas un repositorio desde otro ordenador.
El directorio de trabajo es una copia de una versión del proyecto. Estos archivos se sacan de la base de datos comprimida en el directorio de Git, y se colocan en disco para que los puedas usar o modificar.
El área de preparación es un archivo que almacena información acerca de lo que va a ir en tu próxima confirmación. 
El flujo de trabajo básico en Git es algo así:
	1. Modificas una serie de archivos en tu directorio de trabajo.
	2. Preparas los archivos, añadiéndolos a tu área de preparación.
	3. Confirmas los cambios, lo que toma los archivos tal y como están en el área de preparación, y almacena esas instantáneas de manera permanente en tu directorio de Git.

### Configuración ###
En sistemas Windows se crea un archivo .gitconfig en el directorio del perfil del usuario.

    This PC->Local Disk(C:)->Users->desarrollo10

![](http://i.imgur.com/ebQwrc7.png)

Git usa la información de este archivo para incluirla de forma inmutable en cada confirmación de cambio (*commits*).

Existen para Windows herramientas visuales de Diferencias y Merge tales como **AraxisMerge**, **SemanticMerge**, si usas **SourceTree** como cliente de Git estas herramientas se pueden especificar desde *Tools/Option* en el Tab de *Diff*.

##### Agregar una solución al control de versiones (GitLab + SourceTree) #####
Para poner a punto una solución seguir la siguiente secuencia:
- Crear el proyecto en GitLab
- Crear la solución .NET
- En SourceTree usar la opción Create New Repository apuntando a la carpeta de la solución
- Ejecutar GitBash en la carpeta de la solución.

    	git remote add origin git@10.100.11.133:MyGroup/mygitproject.git

- Agregar archivo ignore a la carpeta de la solución.
- Hacer el primer commit
- Crear el gitlfow.
	
##### Agregar DLLs requeridos para una solución, no ignorar #####
**SourceTree** por default en su configuración ignora los archivos .dll, pero en ocaciones las soluciones requiere incluir en los commit algunos archivos dll. Para lograr esto se requiere forzar al git a incluir estos archivos, desde la línea de comandos en el directorio de la solución teclear el comando **add**, por ejemplo:

    git add _ExternalComponents/LLBLGen/*.dll –f

### Comandos ###
Listar todas las propiedades que Git ha configurado.

	git config –list

Listar archivos cambiados de una rama respecto de la rama develop: Si se requiere conocer todos los archivos modificados o agregados en una rama específica se puede usar el comando:

	git diff --name-only develop
   
![](http://i.imgur.com/z1a6j6o.png)

También se puede guardar el resultado de este listado con la instrucción siguiente, el archivo se guarda en la raíz de la carpeta de la solución.

	git diff --name-only develop > salida.diff   

Si se requiere conocer el detalle de un archivo especifico se puede usar el comando:

    git diff develop -- Orion.RutaComplete/Archivo.cs

![](http://i.imgur.com/RfHXwdv.png)


##### Buscar en los mensajes de un commit un texto específico #####

    git log -g --grep=STRING


# Mantenimiento del servicio GitLab #

##### Reniciar el servidor de Ubuntu #####
Ya firmado en la consola de Ubuntu teclear:

    sudo reboot

# GitHub #

### Update a fork ###

- Ingresar al repositorio al que se le hizo fork en tu cuenta de GitHub
- Revisar que se tenga notificaciones de nuevos commit del autor del repo
	- This branch is 3 commits behind ...
	- Posicionado en la rama master
- Click en el boton New pull request
- En la sección de comparing changes
	- Del lado izquierdo debe esta tu fork
	- Del lado derecho debe estar el repositorio original
	- Para esto se requiere hacer click en la opcion Switching the base
- Click en Create pull request
	- Ingresar un titulo por ejemplo Update from original.
	- En la parte inferior hacer click en Create pull request
	- git debera indicar que no hay conflictos para hacer el merge
- Click en Merge pull request
- Click en Confirm merge
- GitHub regresa al repositorio fork
	- El fork queda un commit adelante
	- Ahora el fork y el repositorio original estan sincronizados.
	
![](http://i.imgur.com/7mEcu7J.png)