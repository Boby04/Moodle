# Instalar y configurar Moodle en Linux

## Requisits:
- Apache
- MySQL o MariaDB
- PHP (Necessari la versió 8.0 cap amunt)

### Instal·lació d'Apache

El primer que farem és actualitzar el repositori de la nostra distribució per assegurar-nos que instal·lem els darrers paquets de cada programa. Per això introduïm al terminal la següent comanda:

```
sudo apt-get update
```

Quan s'hagin actualitzat els paquets, amb l'ordre anterior, instal·lem el servidor web Apache amb la següent comanda:

```
sudo apt-get install apache2
```
![Selecció_003](https://user-images.githubusercontent.com/114423020/204158007-f267775a-fa99-4902-a1e1-15baea5afb4d.png)

Per comprovar que hem instal·lat correctament Apache a la nostra màquina podem anar al nostre navegador i introduir **localhost** com a adreça a visitar a la barra d'adreces, i ens hauria d'apareixer algo semblant al següent:

![image](https://user-images.githubusercontent.com/114423020/204158033-51a78225-a0a8-41e8-8692-f8a9413f9b24.png)


### Instal·lació de MariaDB

Instal·lem la base de dades amb la comanda següent:

```
sudo apt-get install mariadb-server
```
![Selecció_004](https://user-images.githubusercontent.com/114423020/204158120-7134f745-642f-412c-a89e-10d8ba9d32f6.png)

Durant la instal·lació sol·licitarà la contrasenya per a l'usuari **root** de la base de dades.

Quan hàgiu acabat d'instal·lar haureu d'executar la configuració de seguretat del servidor MariaDB amb la comanda següent:

```
sudo mysql_secure_installation
```

Una vegada introduïda la comanda haurem d'habilitar/deshabilitar el següent:

- Deshabilitar usuaris anònims.
- Desactivar accés remot com a root.
- Eliminar les bases de dades de testeig i accedir-hi.
- Actualitzar les taules de privilegis.

![Selecció_005](https://user-images.githubusercontent.com/114423020/204158232-052c317c-f16d-4765-8d61-045f93911b9b.png)

Finalment reiniciem el servidor MariaDB amb la comanda:

```
sudo service mariadb.service restart
```

### Instal·lació de PHP

Abans de procedir a la instal·lació haurem d'obrir una terminal i actualitzar els paquets del sistema. A més instal·larem algunes dependències amb la comanda següent:

```
sudo apt update; sudo apt upgrade
```
![Selecció_011](https://user-images.githubusercontent.com/114423020/204158528-b391a7ab-a8ca-43c4-a824-a948aa7f256d.png)


```
sudo apt install ca-certificates apt-transport-https programari-properties-common
```

![Selecció_013](https://user-images.githubusercontent.com/114423020/204158531-b3af5339-820b-47c9-8abc-c1c7674a47e7.png)


Finalitzada la instal·lació de les dependències, ja hi podem afegir el **PPA d'Ondrej** . A la mateixa terminal, només necessitarem utilitzar l'ordre:

```
sudo add-apt-repository ppa:ondrej/php
```
![Selecció_012](https://user-images.githubusercontent.com/114423020/204158586-d557fce4-dc79-42e7-a8d7-3433c358cba3.png)

Després d'afegir el **PPA** al nostre equip, s'hauria de produir l'actualització de paquets disponibles des dels dipòsits.

Ara haurem d'instal·lar el PHP 8.0 amb el mòdul Apache. Per fer-ho només caldrà executar la següent comanda:

```
sudo apt install php8.0 libapache2-mod-php8.0
```
![Selecció_014](https://user-images.githubusercontent.com/114423020/204158730-b4ab879d-7da0-46c8-a1f6-7de16e5ac2e3.png)

Un cop finalitzada la instal·lació, haurem de reiniciar el servidor web Apache per habilitar el mòdul amb la comanda:

```
sudo systemctl restart apache2
```
![Selecció_015](https://user-images.githubusercontent.com/114423020/204158769-5c7f66ad-629a-47a5-9057-1df0b0a8c07b.png)

Arribats a aquest punt, ja podem confirmar la versió de PHP predeterminada al servidor:

```
php -v
```
![Selecció_016](https://user-images.githubusercontent.com/114423020/204158825-78afb8bb-67f9-495f-adf5-35d39fb44083.png)

## Instal·lació del Moodle

### Descarregar Moodle

Per poder instal·lar Moodle haurem d'anar amb unaltre ordinador i despres anem a la pàgina oficial de **[Moodle](https://moodle.org)** i des d'allà podrem descarregar l'última versió. 

**IMPORTANT: Com que nosaltres estem utilitzant una màquina sense interfície gràfica haurem de fer-ho des d'un altre ordinador amb interfície gràfica, i una vegada tenim l'arxiu que volem descarregar haurem de còpia l'enllaç del document per poder descarregar-lo al servidor amb la comanda següent:**

```
wget <link de l'arxiu que volem descarregar>
```
![Selecció_018](https://user-images.githubusercontent.com/114423020/204159554-9c6328f9-5f21-46e3-b755-c59a1960a8ed.png)


Per descomprimir-lo i col·locar-lo al directori **/var/www/html** i fer-lo accessible via web executem la següent ordre:

```
sudo unzip <link descarregar> -d /var/www/html/
```
![Selecció_019](https://user-images.githubusercontent.com/114423020/204159562-f7979cd0-56cc-4fd5-baf6-76a5a0204ae1.png)


Si voleu moure a un altre directori cal canviar el paràmetre després de -d

**IMPORTANT:** El directori creat ha de poder ser escrit pel servidor web, haurem de canviar el propietari del mateix a **www-data**, per exemple així:

```
sudo chown www-data:www-data /var/www/html/moodle
```
![Selecció_020](https://user-images.githubusercontent.com/114423020/204159764-7010cedf-0bff-495f-a8e9-a7e0cae7bca8.png)


### Crear el directori de fitxers

Moodle utilitza un directori per desar fitxers dels usuaris, és recomanable que aquest directori no estigui al mateix directori que el servidor web, però ha de ser un directori amb accés per part del navegador.

Jo, per exemple, crearem el directori **moodledata** en **home**.

```
cd /home
sudo mkdir moodledata
sudo chown www-data:www-data moodledata/
```
![Selecció_021](https://user-images.githubusercontent.com/114423020/204159761-f2442543-49b6-4d29-be0f-019547dacb42.png)


### Configurar la base de dades

Ara haurem de crear una base de dades per al Moodle, per accedir a la base de dades MariaDB haurem d'introduir la seguent comanda:

```
sudo mysql -u root -p
```
![Selecció_026](https://user-images.githubusercontent.com/114423020/204159812-c97dc472-4a44-49cc-bba5-59cfc3dd4f80.png)


Creem un usuari per a moodle, per exemple moodlemanager:

```
CREATE USER 'moodlemanager'@'localhost' IDENTIFIED BY 'managermoodle';
```
![Selecció_027](https://user-images.githubusercontent.com/114423020/204159818-61058d8e-57a4-496e-b439-125ad1b7704a.png)

I crearem la base de dades per a l'ús de moodle:

```
CREATE DATABASE moodle;
```
![Selecció_028](https://user-images.githubusercontent.com/114423020/204159837-5bb2b7b0-1986-47f5-9087-8565a69c0b78.png)

Finalment, concedeix control sobre la base de dades moodle a l'usuari moodlemanager que hem creat abans:

```
GRANT ALL PRIVILEGES ON moodle.* TO 'moodlemanager'@'localhost';
FLUSH PRIVILEGES;
```
![Selecció_029](https://user-images.githubusercontent.com/114423020/204159851-f40fa3e8-2b18-444e-b86c-0ae13aae1caa.png)

Amb això ja hem acabat la configuració de la base de dades

Finalment per accedir dins del moodle amb el navegador haurem dintroduir la IP de la nostra màquina i fiquem moodle. I ens hauria d'apareixer el següent:
![Selecció_030](https://user-images.githubusercontent.com/114423020/204159950-76a1f279-8402-4669-b23c-e6053e307e9b.png)
En aquesta pàgina configurem la instalació i direm amb quina llengua voldrem instalar-ho.

Segurament després d'instal·lar-ho ens apareixera aquesta pantalla on ens dira que haurem d'instal·lar dos extensions per continuar.
![Selecció_008](https://user-images.githubusercontent.com/114423020/205707725-545420f9-18a5-4387-b73c-b2d8eea527e7.png)

Per descarregar les dos extensiona haurem de fer les següent comandes:

```
1. sudo apt search php | grep curl
2. sudo apt install php8.0-curl (descarreguem la versio que tenim nosaltres de php)
```
![Selecció_005](https://user-images.githubusercontent.com/114423020/205708895-027506bf-66c2-46b8-bff6-34de93d3d9a8.png)


```
1. sudo apt search php | grep zip
2. sudo apt install php8.0-zip(descarreguem la versio que tenim nosaltres de php)
```
![Selecció_006](https://user-images.githubusercontent.com/114423020/205708955-ae1fe20e-0f2e-4b4e-85d0-a4cbc406642c.png)

Una vegada fet aixo actualitzem el apache amb la comanda 

```
sudo service apache2 reload
```

Seguidament ens apareixera una pantalla on haurem de confirma la ruta de la pàgina web, per defecte esta a **"/var/www/moodledata"** pero ho haurem de canviar a **"/home/moodledata"**.

**Antes:**
![Selecció_009](https://user-images.githubusercontent.com/114423020/205710064-67e78ab8-b4ce-455f-af7b-c01628cea80e.png)

**Després:**
![Selecció_010](https://user-images.githubusercontent.com/114423020/205710100-04129403-8237-4b2e-a0e2-b6d912923863.png)

Després haurem de selecionar el nostre controlador de base de datos.
![Selecció_011](https://user-images.githubusercontent.com/114423020/205710245-41560871-288d-40db-9f6d-442476e741ab.png)

Ens apareixera una pantalla dels ajustes de base de dades on haurem de inserir el nom d'usuari i la contrasenya que hem creat anteriorment amb la base de dades.
![Selecció_012](https://user-images.githubusercontent.com/114423020/205710535-36dc8ffa-7ebd-47ec-8622-9c4d4a7a1d14.png)

Ara ens apareixera un error on ens indicara instal·lar 2 extensions més, que són **"String"** i **"xml"**.
![Selecció_014](https://user-images.githubusercontent.com/114423020/205710904-35190947-8b37-4c19-a580-d749d388d3fb.png)

Per instal·lar les extensions ho haurem de instal·lar amb aquestes comandes:

```
sudo apt install php8.0-xml
```
![Selecció_015](https://user-images.githubusercontent.com/114423020/205711167-508c57a6-c0df-44b4-8fe0-7ffdec928e89.png)

```
sudo apt install php8.0-mbstring
```
![Selecció_016](https://user-images.githubusercontent.com/114423020/205711407-4ad34f64-97fd-4d40-a256-eb5b90d4db58.png)

I tornem a actualitzar apache2
![Selecció_017](https://user-images.githubusercontent.com/114423020/205711538-bf2bd77c-6572-40c3-b891-05b0f8127c4e.png)

Ens apareixera una pantalla on ens ensenyar els terminos i condiciones, seguidament cliquem continuar.
![Selecció_019](https://user-images.githubusercontent.com/114423020/205711837-5ddba2a8-71f1-4bc8-a36e-8fb011c7dd68.png)

Una vegada configurat el moodle ens apareixera una pantalla amb les coses que ens falten per fer la instal·lació de moodle. Primer que tot ens apareixera les extensions que hens faltens:

![Selecció_029](https://user-images.githubusercontent.com/114423020/205712196-34fef141-8b7a-4545-90f8-af45c079fa1e.png)

I baix de tot ens apareixera un error de la configuracio de php.
![Selecció_030](https://user-images.githubusercontent.com/114423020/205712877-2ceed822-0cf2-4309-8eae-f87f45ed42d9.png)

Per resoldre aquests problemes haurem de fer el següent

**Instal·lar extensions necessaies**
![Selecció_031](https://user-images.githubusercontent.com/114423020/205713062-54f2c780-3c7d-4a8f-a1c1-82afb12c2cc5.png)

**Configuració de php**
Haurem d'anar a l'arxiu php.ini, per entra dins d'aquest arxiu haurem d'estar dins de la carpeta **apache2** i després haurem d'inserir la comanda:

```
sudo nano php.ini
```
![Selecció_021](https://user-images.githubusercontent.com/114423020/205713748-295fb26c-4802-4197-a33a-cb419fc3b60f.png)

I una vegada dins hem de clica **Windows+w** i esciure **max_input_vars =**, per pretederminat ficara **1000** pero nosaltres haurem de fica **5000** i descomentar la línia.
![Selecció_032](https://user-images.githubusercontent.com/114423020/205714119-43228a32-89fd-4ad8-919e-ca2a0117a0d4.png)

Finalment una vegada instal·lr tot ens hauria d'apareixer que el nostre entorn compleix els requisits mínims i podem clica **continuar**.
![Selecció_033](https://user-images.githubusercontent.com/114423020/205714431-1a9d79d0-6e7c-4702-bd7e-cc15dbef0275.png)

Ara inserim les dades generals per el moodle amb l'usuari i contrasenyar incluit.
![Selecció_034](https://user-images.githubusercontent.com/114423020/205714627-070ef76d-7a2a-427e-b2f1-2a79633f9301.png)

I també configurem la pàgina inicial.
![Selecció_035](https://user-images.githubusercontent.com/114423020/205714751-bfcc2ecc-1999-4a21-b048-b4606d97ff1a.png)

Un cop fet tot el anterior ja esta tot acabat, ens hauria d'apareixer la pàgina principal de moodle.
![image](https://user-images.githubusercontent.com/114423020/205715047-f3d382d8-bd46-4d4e-b360-a58a7ca42bc8.png)





