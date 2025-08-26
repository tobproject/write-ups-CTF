# THM / Ice Walkthrough

The markdown code below showcases various markdown features. Enjoy!


## TASK 2 : **Recon**
## **Step 1: Escaneo inicial del host**

**Objetivo:** Identificar puertos abiertos y servicios corriendo en la máquina objetivo.

**IP de la máquina:** **10.10.156.23**

**Herramienta utilizada:** Script personalizado con Nmap

**Comando ejecutado:**
```bash
./autoscan.sh
```

Al ejecutar el script, se solicitó la IP a escanear:

```
Introduce la IP a escanear: 10.10.156.23
```

---

### **Descripción del escaneo**

Se realizó un escaneo completo con las siguientes opciones de Nmap:

* `-sS` → Escaneo SYN (half-open scan) para identificar puertos abiertos sin establecer una conexión completa.
* `-sC` → Ejecuta scripts por defecto de Nmap para obtener información adicional sobre los servicios.
* `-sV` → Detecta la versión de los servicios en los puertos abiertos.
* `--stats-every=5s` → Muestra el progreso del escaneo cada 5 segundos.
* `-oX scan.xml` → Guarda los resultados en formato XML para análisis posterior.

El XML generado se transformó en un archivo HTML (`scan.html`) para una revisión más sencilla y se abrió automáticamente en Firefox.

---

### Resultados del escaneo de Nmap







| Puerto | Protocolo | Servicio     | Versión       | Observaciones                    |
| ------ | --------- | ------------ | ------------- | -------------------------------- |
| 22     | tcp       | ssh          | OpenSSH 7.9p1 | Acceso remoto seguro             |
| 80     | tcp       | http         | Apache 2.4.29 | Página web disponible            |
| 139    | tcp       | netbios-ssn  | Samba smbd    | Posible compartición de archivos |
| 445    | tcp       | microsoft-ds | Samba smbd    | Posible vulnerabilidad SMB       |

**Once the scan completes, we'll see a number of interesting ports open on this machine. As you might have guessed, the firewall has been disabled (with the service completely shutdown), leaving very little to protect this machine. One of the more interesting ports that is open is Microsoft Remote Desktop (MSRDP). What port is this open on?:**
```
| ------ | --------- | ------------ | ------------- | -------------------------------- 
| 80     | tcp       | http         | Apache 2.4.29 | Página web disponible            |
| 139    | tcp       | netbios-ssn  | Samba smbd    | Posible compartición de archivos |
|3389    |	tcp      | 	open 	    |tcpwrapped     | 	syn-ack 	        	        |
```


**What service did nmap identify as running on port 8000? (First word of this service)**:
```
Icecast
```

**What does Nmap identify as the hostname of the machine? (All caps for the answer)**:
```
DARK-PC
```




## TASK 3 : **Gain Access**
## **Step 1: Exploit the target vulnerable service to gain a foothold!**

**Objetivo:** Explotar el servicio vulnerable del objetivo para obtener acceso.


**Now that we've identified some interesting services running on our target machine, let's do a little bit of research into one of the weirder services identified: Icecast. Icecast, or well at least this version running on our target, is heavily flawed and has a high level vulnerability with a score of 7.5 (7.4 depending on where you view it). What is the Impact Score for this vulnerability? Use https://www.cvedetails.com for this question and the nex:**
```bash
Check 'Icecast' on: https://www.cvedetails.com 
```

**  What is the CVE number for this vulnerability? This will be in the format: CVE-0000-0000 **:
```
Check 'Icecast' on: https://www.cvedetails.com 
```





