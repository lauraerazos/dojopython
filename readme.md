# README #

# Dojo python pandas

En este repositorio se encuentra el codigo del Dojo python-pandas.
El software esta desarrollado en el lenguaje de programacion Python a traves del framework flask usado para crear 
aplicaciones web.  

## Versiones

* Python -> 3.6.x (La version 3.7.x aún presenta incompatibilidad con varias librerías usadas, e.g. celery)
* flask -> 1.0.x
* celery -> 4.2.x
* redis -> 4.0.x - 5.0.x (Se ha probado en esas versiones y funciona correctamente)
* MySQL -> 5.6.x (En desarrollo se probó con la version 8 y sigue siendo compatible)


## Para empezar
### Ambiente virtual de python
Antes de empezar se recomienda usar un ambiente virtual de python. Para esto se pueden usar las librerias virtualenv 
y virtualenvwrapper. virtualenvwrapper es una libreria que ayuda a gestionar los ambientes creados con virtualenv.
En la documentacion de cada una de ellas se explica como pueden ser instaladas.

Virtualenv:
https://virtualenv.pypa.io/en/latest/

Virtualenvwrapper: https://virtualenvwrapper.readthedocs.io/en/latest/

~~~~
sudo apt-get install virtualenv

sudo apt install python3-pip

pip3 install virtualenvwrapper
~~~~

Editamos el archivo de configuración bashcr y agregarmos las siguientes lineas al final del archivo.

~~~~
nano .bashrc

export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Devel
source /home/ubuntu/.local/bin/virtualenvwrapper.sh
~~~~

Ejecutamos el siguiente comando para aplicar los cambios.

`source .bashrc`

Importante: crear el ambiente con la version 3.6.x de Python. Para instalar las diferentes librerías y paquetes en 
python se debe tener instalador el manejadro de paquetes de python PIP. Normalmente viene instalado en sistemas UNIX. 
Si no se encuentra, en Linux se puede instalar con el respectivo manejador de paquetes del sistma operativo (yum, apt) 
y en MACOS puede ser instalado con brew al instalar la version de Python requerida. 

Para crear el ambiente con virtualenvwrapper se usa el siguiente comando:

`mkvirtualenv -p python3.6 dojo`

La version python3.6 debe estar instalada en el equipo para poder usarla en la creacion del ambiente.

Para trabajar con el ambiente de python creado se usa el comando:

`workon dojo`

Para salir del ambiente y volver a la version local de python se usa el comando:

`deactivate`

Cuando se trabaja con un ambiente virtual se crea una version aislada de Python con su propio manejador de paquetes PIP. 
Las librerias instaladas con PIP estando activado el ambiente virtual, solo quedan disponibles en dicho ambiente. 
Esto sirve para no tener conflictos con versiones y otras librerías.

### Instalación de dependencias
Ya creado y activado el ambiente virtual de python, con la version requerida, se pueden instalar las dependencias de 
la aplicación. Para instalar las dependencias se usa el siguiente comando dentro del ambiente virtual ubicando la 
consola en la carpeta base del proyecto:

`pip install -e .`

Todas las dependencias se encuentran en el archivo setup.py el cual es identificado por el comando anterior para 
poder instalar los paquetes requeridos.

Fuera del ambiente virtual se debe instalar redis para ello ejecutamos los siguientes comandos:

`sudo apt install redis-server`

Luego editamos el achivo de configuración redis.conf cambiando la configuración "supervised" a "systemd" y reiniciamos el servicio

`sudo apt install redis-server`

~~~~
sudo nano /etc/redis/redis.conf

. . .

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd

sudo systemctl restart redis.service
~~~~ 

### Instalación gestor de paquetes
Es requerido administrar las librerias externas por medio de un gestor de paquetes, para ello debemos instalar webpack
mediante npm y nodejs, ejecutamos los diguientes comandos.

~~~~
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install nodejs
~~~~ 

Posterior a la instalación nos ubicamos en la carpeta /app/static/ y ejecutamos los comandos

~~~~
sudo npm install
npm run build
~~~~ 

## Correr en ambiente de desarrollo
Para correr el software en ambiente de desarrollo se puede usar el servidor de aplicaciones que viene por defecto con 
flask. Según la documentación este servidor solo debe ser usado en desarrollo, para ambientes de produccion se deben 
usar servidores optimizados para dicho fin. En este caso se usa gunicorn que es un servidor Python WSGI HTTP.

Antes de iniciar cualquier tipo de servidor se deben especificar las variables de entorno necesarias. 
En sistemas UNIX se hace de la siguiente manera:

~~~~
export FLASK_APP=run.py
export FLASK_ENV=development
~~~~

El siguiente comando sirve para iniciar el servidor de aplicaciones que viene por defecto con flask:

`flask run -h 0.0.0.0`

El parametro -h (host) se establece en 0.0.0.0 si se necesita acceder al servidor a través de una IP diferente a 
localhost.

Si se desea probar el servicio a través del servidor de aplicaciones gunicorn el comando sería el siguiente:

`gunicorn -b localhost:8000 -w 1 run:app --timeout 1200 --threads 4`

Si se quieren imprimir mensajes de debug, se debe agregar al comando el parametro --log-level=debug

Luego pueden ingresar a la url 

`http://localhost:8000/accounting/load_account_files`

## Información adicional
Los siguientes son enlaces que sirvieron para el desarrollo y despliegue de este proyecto:

https://scotch.io/tutorials/build-a-crud-web-app-with-python-and-flask-part-one

https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04

https://blog.miguelgrinberg.com/post/using-celery-with-flask

https://blog.miguelgrinberg.com/post/celery-and-the-flask-application-factory-pattern

https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-18-04

https://linuxize.com/post/how-to-set-or-change-timezone-on-ubuntu-18-04/

Y la documentacion oficial de flask:

http://flask.pocoo.org/docs/1.0/tutorial/