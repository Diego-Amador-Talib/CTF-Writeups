

# Writeup: Blog

![Banner deB log](images/banner_blog.png)

**Pickle Rick** es un desafío de nivel **Fácil** de la plataforma **TryHackMe**. En este writeup, documento los pasos que seguí para obtener acceso al sistema, escalar privilegios y encontrar los 3 ingredientes.

---

## 📊 Datos Esenciales

* **IP de la Máquina:** `10.10.220.243`
* **Sistema Operativo:** `Linux`
* **Tipo de Máquina:** `Web`
* **Ingredientes:** `mr. meeseek hair`, `1 jerry tear` y `fleeb juice`.
* **Tiempo de Resolución:** `30 min`

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
