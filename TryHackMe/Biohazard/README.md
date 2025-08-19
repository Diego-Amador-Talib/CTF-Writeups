# Writeup: Biohazard
![Banner de Biohazard](images/Banner.png)
**Biohazard** es una máquina de nivel **Medio** en la plataforma **TryHackMe**, que me brindó un excelente desafío de CTF. En este write-up, documento el proceso que seguí para resolver la máquina, enfrentándome a una serie de retos inesperados que requirieron un profundo **análisis de cifrados** y la aplicación de técnicas de **esteganografía** y **decodificación de textos**. A lo largo de este análisis, detallaré cada paso, desde la fase inicial de reconocimiento hasta la obtención del acceso final.

---

## 📊 Datos Esenciales

- **IP de la Máquina:** `10.10.X.X`
- **Sistema Operativo:** `Linux`
- **Temática:** `Survival Horror`
- **Habilidades Clave:** `Análisis de Cifrados`, `Esteganografía`, `Vigenère`
- **Tiempo de Resolución:** `75 min`
---

## 🕵️‍♂️ Fase 1: Reconocimiento y Enumeración

El primer paso fue realizar un escaneo de puertos para identificar los servicios activos en la máquina. Utilicé `nmap` con los siguientes parámetros para un escaneo de versiones y scripts:

```bash
nmap -p- --open -sS -sC -sV --min-rate 2000 -vvv -n -Pn 10.10.248.135 -oN escaneo
```

El escaneo inicial con **Nmap** confirmó que la máquina está en línea y reveló los siguientes puertos abiertos:

---

### Puertos Abiertos

  * **Puerto 21 (FTP)**
      * **Servicio:** vsftpd 3.0.3
  * **Puerto 22 (SSH)**
      * **Servicio:** OpenSSH 7.6p1 Ubuntu
  * **Puerto 80 (HTTP)**
      * **Servicio:** Apache httpd 2.4.29 (Ubuntu)
      * **Título del sitio:** "Beginning of the end"

---
### Análisis del Sitio Web (Puerto 80)
![1](images/1.png)
