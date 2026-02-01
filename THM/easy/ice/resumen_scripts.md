
# Resumen de Scripts de Escaneo Nmap y Generación de HTML

## 1. Script Básico: `script.sh`
Escanea una IP fija y abre el HTML en Firefox.

```bash
#!/bin/bash
sudo nmap 192.168.3.112 -sS -sC -sV --stats-every=5s -oX scan.xml && xsltproc scan.xml -o scan.html && firefox scan.html &
```

- **IP fija**: 192.168.3.112  
- Escaneo SYN, scripts por defecto y detección de versiones  
- Convierte XML a HTML y abre en Firefox automáticamente  
- Muy simple, no permite elegir la IP dinámicamente  

---

## 2. Script Simple con Elección de IP: `script_simple.sh`

```bash
#!/bin/bash
# Pedir la IP al usuario
read -p "Introduce la IP a escanear: " IP

# Escaneo completo y generación de HTML
sudo nmap $IP -sS -sC -sV --stats-every=5s -oX scan.xml && xsltproc scan.xml -o scan.html && firefox scan.html &
```

- Permite **elegir la IP** al ejecutar el script  
- Ejecuta Nmap, genera XML y HTML  
- Abre automáticamente Firefox  

---

## 3. Script Organizado por Carpetas: `autoscan.sh`

```bash
#!/bin/bash
# Pedir la IP al usuario
read -p "Introduce la IP a escanear: " IP

# Crear carpeta para guardar resultados
mkdir -p scans_$IP
cd scans_$IP

# Escaneo completo con Nmap y XML
sudo nmap $IP -sS -sC -sV --stats-every=5s -oX scan.xml

# Convertir XML a HTML
xsltproc scan.xml -o scan.html

# Abrir HTML automáticamente en Firefox
firefox scan.html &
```

- Crea una **carpeta por cada IP** (`scans_<IP>`)  
- Evita sobreescribir archivos si escaneas varias IPs  
- HTML se abre automáticamente  

---

## 4. Script Completo con Archivo de Notas: `autoscan_notas.sh`

```bash
#!/bin/bash
# Pedir la IP al usuario
read -p "Introduce la IP a escanear: " IP

# Crear carpeta para guardar resultados
mkdir -p scans_$IP
cd scans_$IP

# Escaneo completo con Nmap y XML
sudo nmap $IP -sS -sC -sV --stats-every=5s -oX scan.xml

# Convertir XML a HTML
xsltproc scan.xml -o scan.html

# Crear archivo de notas con plantilla
NOTES="notas_$IP.txt"
echo "Notas del escaneo para $IP" > $NOTES
echo "------------------------------------" >> $NOTES
echo "Aquí puedes escribir el desglose, hallazgos y pasos para la explotación." >> $NOTES

# Abrir HTML automáticamente en Firefox
firefox scan.html &

echo "[*] Escaneo completado. Revisa la carpeta scans_$IP/"
echo "[*] Archivo de notas creado: $NOTES"
```

- **Más completo**: además de HTML, crea un **archivo de notas por IP** para tus writeups  
- Todos los resultados se guardan en la carpeta correspondiente  
- HTML se abre automáticamente y notas listas para editar  

---

### 🔹 Resumen

| Script | IP | Carpeta por IP | Archivo de notas | HTML abierto |
|--------|----|----------------|-----------------|--------------|
| script.sh | Fija | ❌ | ❌ | ✅ |
| script_simple.sh | Dinámica | ❌ | ❌ | ✅ |
| autoscan.sh | Dinámica | ✅ | ❌ | ✅ |
| autoscan_notas.sh | Dinámica | ✅ | ✅ | ✅ |

---

**Conclusión:**  
- Para uso rápido, `script_simple.sh` basta.  
- Para organizar resultados y agregar writeups, `autoscan_notas.sh` es la mejor opción.
