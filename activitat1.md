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

Per descomprimir-lo i col·locar-lo al directori **/var/www/html** i fer-lo accessible via web executem la següent ordre:
