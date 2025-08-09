

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

Con esta información, hemos obtenido un posible nombre de usuario: `RickRu13s`. Este dato será crucial para la siguiente fase de enumeración, especialmente si intentamos acceder vía **SSH**.
      
