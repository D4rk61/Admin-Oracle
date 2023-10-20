## Instalacion de WebLogic 12 en Servidor Oracle Linux 8

Actualmente me costo encontrar documentacion 
y mas en español sobre la instalacion de **Oracle
Web-logic** ademas esta instalacion seguira una serie
de documentaciones que hice anteriormente y espero
que sirvan de ayuden a alguien.

**Instalacion: Se hara una instalacion de "Oracle 
Web-logic" en un servidor Oracle linux 8, administrado
por ssh sin interfaz grafica**

<br>

##### Requisitos: 

1. Tener instalado Oracle linux 8, no he probado en oracle linux 9
2. Haber hecho la instalacion de este repositorio: [repo](https://github.com/D4rk61/sshX11)
3. Conocer de temas basicos como ssh, uso del jdk de java
4. (Opcional) tener instalado Oracle database, link: [repo2](https://github.com/D4rk61/Oracle-database)

##### Instalacion:

###### 1. Instalacion del jdk adecuado para web-logic:

El link estara aqui: [jdk-necesario](https://drive.google.com/file/d/1nPmUy0XEPSohDLVxVwUp2JAXGorsiBvv/view?usp=sharing) este lo he subido en drive!

Entonces para instalarlo tienen 2 opciones:

```shell
# Primera opcion - Instalarlo directamente en el server

# Esto directamente en el servidor
wget {link-del-jdk}


# Segunda opcion - Instalarlo en tu maquina local

scp {archivo.rpm} {usuario}@{ip-servidor}:{ruta}
```

En la primera descargaremos el archivo directamente en nuestro
servidor con wget y el link del drive, en la segunda primero 
instalaremos el archivo en nuestra maquina y luego con **scp** 
lo enviaremos al servidor remoto

<br>

Posterior, en nuestro servidor ejecutaremos para instalar el jdk:
`sudo rpm -ivh jdk-8u191-linux-x64.rpm`

Iremos a esta direccion: `/usr/java` y enviaremos el jdk al directorio 
correcto con: `sudo cp -r jdk1.8.0_191-amd64 /usr/lib/jvm/`

Y en nuestro **.zshrc o .bashrc** agregamos lo siguiente:

```shell
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_191-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

<br>

###### 2. Instalacion del servidor

Recordemos que estamos en un servidor sin GUI asi que dare 2 maneras
para hacer esta instalacion:

**Link del archivo weblogic12.zip [archivo](https://drive.google.com/file/d/1m_WpBQmlzQeWgFSQfsddOFmBbWEvy3vy/view?usp=sharing)**

<br>

1. Primera forma, haciendo uso de **ssh -Y**

Esta forma nos permite hacer la instalacion con el asistente grafico 
sin que nuestro servidor tenga interfaz grafica, para eso 
debes de cumplir los requisitos escritos arriba de la instalacion

```shell
# Primero descargamos el archivo como antes del drive:

# Segundo una vez teniendo el .zip del weblogic, lo descomprimimos
unzip {archivo-web-logic}.zip

# Esto nos dara un archivo .jar, comprobamos el jdk:
[oracle@localhost]~% echo $JAVA_HOME
/usr/lib/jvm/jdk1.8.0_191-amd64

# Si no da algo asi, simplemente ejecutamos
java -jar {archivo-extension-jar}.jar
```

2. Segunda forma, sin servidor grafico **ssh** normal

Si no tenemos una interfaz grafica, podemos hacer una 
instalacion silenciosa, esta requiere mas configuracion pero es 
fiable


```shell
# Creamos la carpeta principal, por ejemplo, con cualquier nombre
mkdir weblogic
cd weblogic

# Posicionarnos en el archivo donde este el .jar
cd {carpeta}
touch response_file

# Dentro del archivo agregar lo que esta en "response_file" del repo
vim response_file

# Esta variable: (dentro del archivo response_file)
# La debemmos de configurar segun hayamos creado 
# la carpeta raiz anteriormente
ORACLE_HOME=/home/oracle/weblogic

# Ejecutamos la instalacion de forma silenciosa

java -jar fmw_12.2.1.4.0_wls_lite_generic.jar -silent -responseFile {ruta-archiv-response_file}

# Ejemplo: (no ejecutar)
java -jar fmw_12.2.1.4.0_wls_lite_generic.jar -silent -responseFile /home/oracle/web-server/response_file
```