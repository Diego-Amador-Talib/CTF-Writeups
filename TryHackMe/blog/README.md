

# Writeup: Blog

![Banner deB log](images/banner_blog.png)

**Blog** es un desafío de nivel **Medio** de la plataforma **TryHackMe**. En este writeup, documento los pasos que seguí para obtener acceso al sistema, escalar privilegios y encontrar las 2 banderas: **user.txt** y **root.txt**.

---

## 📊 Datos Esenciales

* **IP de la Máquina:** `10.10.232.42`
* **Sistema Operativo:** `Linux`
* **Tipo de Máquina:** `Web`
* **Flags:** `user.txt`, `root.txt`.
* **Tiempo de Resolución:** `75 min`

---

## 🕵️‍♂️ Fase 1: Reconocimiento y Enumeración

El primer paso fue realizar un escaneo de puertos para identificar los servicios activos en la máquina. Utilicé `nmap` con los siguientes parámetros para un escaneo de versiones y scripts:
```bash
nmap -p- --open -sS -sC -sV --min-rate 2000 -n -vvv -Pn 10.10.232.42 -oN escaneo
```

El escaneo inicial con **Nmap** confirmó que la máquina está en línea y reveló los siguientes puertos abiertos:

---

### Puertos Abiertos

* **Puerto 22 (SSH)**
    * **Servicio:** OpenSSH 7.6p1 Ubuntu
* **Puerto 80 (HTTP)**
    * **Servicio:** Apache httpd 2.4.29
    * **Título del sitio:** "Billy Joel's IT Blog – The IT blog"
    * **Detalles:** El sitio corre sobre **WordPress 5.0** y tiene un directorio de administración (`/wp-admin`) que está configurado para no ser rastreado por robots.
* **Puerto 139 (netbios-ssn)**
    * **Servicio:** Samba smbd 3.X - 4.X
* **Puerto 445 (netbios-ssn)**
    * **Servicio:** Samba smbd 4.7.6-Ubuntu
    * **Detalles:** El servicio Samba muestra el nombre de la máquina como **"BLOG"** y pertenece al grupo de trabajo **"WORKGROUP"**. La información de seguridad indica que el inicio de sesión de invitados (`guest`) está habilitado y la firma de mensajes está deshabilitada.

---
**Análisis del Sitio Web (Puerto 80)** Al navegar a la dirección IP (`10.10.232.42`), el sitio web no carga correctamente. Según la descripción del desafío, esto se debe a que el sitio web requiere que se resuelva un nombre de dominio específico.
![1](images/1.png)
Para solucionar este problema, se debe añadir una entrada al archivo **`/etc/hosts`** de la máquina atacante, mapeando el nombre de host `blog.thm` a la dirección IP de la máquina objetivo.

```bash
echo "10.10.232.42 blog.thm" | sudo tee -a /etc/hosts
```

Una vez que se añade esta línea, al navegar a `http://blog.thm` se accede al blog de WordPress, titulado "Billy Joel's IT Blog – The IT blog", como se detectó en el escaneo de Nmap.
![2](images/2.png)
