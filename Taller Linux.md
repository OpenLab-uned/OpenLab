
# Historia
- Software Libre
- FSF
- GNU
- Distros
- Desarrollo posterior (estadísticas actuales)
Richard Stallman y la impresora son clave en la historia del software libre.  En la década de 1970, mientras trabajaba en el Laboratorio de Inteligencia Artificial del MIT, Stallman enfrentó problemas frecuentes con una impresora que se atascaba con papel. Para resolverlo, había creado un sistema que notificaba a los usuarios cuando la impresora estaba bloqueada, pero esta solución dependía del acceso al código fuente del software. 

En 1980, cuando el laboratorio recibió una nueva impresora láser Xerox 9700, Stallman intentó replicar su solución.  Sin embargo, le negaron el código fuente, argumentando que era información confidencial.  Esta negativa, junto con el hecho de que un colega también se negó a compartir el código bajo un acuerdo de no divulgación (NDA), marcó un punto de inflexión. 

Este episodio fue el detonante que llevó a Stallman a fundar el movimiento del software libre.  En 1983, anunció el proyecto GNU, con el objetivo de crear un sistema operativo completamente libre. En 1985, creó la Licencia Pública General (GPL), que garantiza a los usuarios la libertad de usar, estudiar, modificar y redistribuir el software, siempre que se comparta el código fuente.

**Software** **Libre**



**FSF**


**GNU**


# Instalación de Linux
## Panorama general (la idea clave)

Lo que estás haciendo es esto:

> **Modificar el proceso normal de arranque del ordenador** para que, en lugar de cargar el sistema operativo del disco duro, cargue **Ventoy desde un pendrive**, y desde Ventoy arrancar una **ISO de Ubuntu** (u otras).

Eso implica **interactuar con el firmware del ordenador (BIOS/UEFI)**, entender **el orden de arranque**, y saber **qué límites y riesgos existen**.

Vamos por partes.

---

## 1️⃣ ¿Qué es Ventoy y qué implica usarlo?

![https://www.ventoy.net/static/img/secondary_menu1.png](https://www.ventoy.net/static/img/secondary_menu1.png)

![https://www.ventoy.net/static/img/screen/screen_uefi_en.png?v=4](https://www.ventoy.net/static/img/screen/screen_uefi_en.png?v=4)

![https://linuxmint-user-guide.readthedocs.io/en/latest/_images/ventoy_boot.jpg](https://linuxmint-user-guide.readthedocs.io/en/latest/_images/ventoy_boot.jpg)

4

### 🔧 ¿Qué es Ventoy?

**Ventoy** es un **gestor de arranque** que se instala **una sola vez** en un pendrive y permite arrancar **ISOs directamente**, sin tener que “quemarlas” o copiarlas de forma especial.

👉 Copias la ISO como si fuera un archivo normal.  
👉 Ventoy se encarga de arrancarla.

### 🧩 ¿Qué hace Ventoy a nivel técnico?

Cuando instalas Ventoy en un USB:

1. **Reestructura el pendrive**
    
    - Crea **una pequeña partición de arranque** (con su propio cargador).
        
    - El resto del USB queda como una **partición de datos normal** (FAT32/exFAT/NTFS).
        
2. **Instala su propio bootloader**
    
    - Compatible con **BIOS Legacy y UEFI**.
        
    - Puede usar **Secure Boot** (con confirmación).
        
3. **Al arrancar**
    
    - El firmware (BIOS/UEFI) ejecuta Ventoy.
        
    - Ventoy **escanea el USB**, detecta ISOs.
        
    - Muestra un menú.
        
    - Al elegir una ISO, hace **chainloading** (salta al cargador de esa ISO).
        

💡 Ventoy **no modifica el disco duro del ordenador**, solo se ejecuta en RAM.

---

## 2️⃣ ¿Qué es una ISO de Ubuntu y qué significa arrancarla?

![https://ubuntucommunity.s3.dualstack.us-east-2.amazonaws.com/original/2X/4/49a92ce6373041a7f8f50ddf6495f8ac539ad275.jpeg](https://ubuntucommunity.s3.dualstack.us-east-2.amazonaws.com/original/2X/4/49a92ce6373041a7f8f50ddf6495f8ac539ad275.jpeg)

![https://i.sstatic.net/Sicgj.png](https://images.openai.com/static-rsc-1/nHhNp_dOX1lkcQDOOaQKPDlERLJ8b9mTtqRCPmu2BaqZjALa--0WBz37Mu0lQOzS8VxOhE1FhZNZgBtUY8otFZHtXpsImW20nwsDQCuor6W2WS32_8JuopUPvkk7XSzkQykk7Wm3dutzP2F_PqfhAg)

![https://ubuntucommunity.s3.us-east-2.amazonaws.com/original/2X/4/49a92ce6373041a7f8f50ddf6495f8ac539ad275.jpeg](https://ubuntucommunity.s3.us-east-2.amazonaws.com/original/2X/4/49a92ce6373041a7f8f50ddf6495f8ac539ad275.jpeg)

4

**Ubuntu** es una distribución Linux.  
Su ISO suele permitir dos modos:

### 🟢 Modo “Live”

- Ubuntu se carga **en memoria RAM**.
    
- No toca el disco duro.
    
- Ideal para talleres.
    
- Todo se pierde al apagar (salvo persistencia).
    

### 🔴 Modo instalación

- Modifica el disco duro.
    
- **Aquí sí hay riesgo real** si alguien se equivoca.
    

👉 En un taller, **deja claro que solo se usa el modo Live**.

---

## 3️⃣ ¿Qué peligros tiene Ventoy y el arranque por USB?

### ⚠️ Riesgos reales (pero controlables)

#### 1. **Borrar el pendrive**

- Instalar Ventoy **elimina todo lo que haya en el USB**.
- Solución: avisar y usar pendrives dedicados.

#### 2. **Arranque accidental del instalador**

- Un alumno puede darle a “Instalar Ubuntu”.
    
- Solución:
    
    - Explicarlo explícitamente.
        
    - Supervisar.
        
    - Idealmente usar ISOs que prioricen Live.
        

#### 3. **Secure Boot**

- Algunos equipos lo bloquean.
    
- Ventoy es compatible, pero puede pedir confirmación.
    

#### 4. **Ordenadores bloqueados**

- Equipos corporativos o educativos:
    
    - USB deshabilitado.
        
    - BIOS protegida por contraseña.
        
- Aquí no hay magia: **no se puede forzar**.
    

❗ **Ventoy NO daña el ordenador por sí mismo**.

---

## 4️⃣ ¿Qué es la BIOS / UEFI y por qué importa?

![https://docs.oracle.com/cd/E19269-01/820-5830-13/figures/app_bios-5.jpg](https://docs.oracle.com/cd/E19269-01/820-5830-13/figures/app_bios-5.jpg)

![https://www.partitionwizard.com/images/uploads/articles/2020/01/uefi-firmware-settings-missing-windows-10/uefi-firmware-settings-missing-windows-10-1.jpg](https://www.partitionwizard.com/images/uploads/articles/2020/01/uefi-firmware-settings-missing-windows-10/uefi-firmware-settings-missing-windows-10-1.jpg)

![https://www.lifewire.com/thmb/VuhXHxvvZfrEKNKe4pmK49EqpoU%3D/1500x0/filters%3Ano_upscale%28%29%3Amax_bytes%28150000%29%3Astrip_icc%28%29/phoenixbios-setup-main-screen-59959547c41244001039e3b1.PNG](https://www.lifewire.com/thmb/VuhXHxvvZfrEKNKe4pmK49EqpoU%3D/1500x0/filters%3Ano_upscale%28%29%3Amax_bytes%28150000%29%3Astrip_icc%28%29/phoenixbios-setup-main-screen-59959547c41244001039e3b1.PNG)

4

### 🧠 Definición clara

La **BIOS** (o su versión moderna, **UEFI**) es el **firmware** que:

1. Se ejecuta al encender el ordenador.
    
2. Inicializa el hardware.
    
3. Decide **desde dónde arrancar**.
    

### BIOS vs UEFI (resumen para explicar)

|BIOS (antigua)|UEFI (moderna)|
|---|---|
|Interfaz simple|Interfaz gráfica|
|Legacy|Estándar actual|
|Sin Secure Boot|Secure Boot|
|Limitada|Más flexible|

Ventoy funciona **en ambos**.

---

## 5️⃣ ¿Cómo se entra en la BIOS / menú de arranque?

### ⌨️ Teclas habituales (esto es CLAVE en un taller)

| Marca  | Menú de arranque | BIOS    |
| ------ | ---------------- | ------- |
| HP     | F9               | F10     |
| Dell   | F12              | F2      |
| Lenovo | F12 / Fn+F12     | F1 / F2 |
| ASUS   | ESC              | F2      |
| Acer   | F12              | F2      |
| MSI    | F11              | DEL     |

💡 **Menú de arranque (Boot Menu)** es preferible:

- No cambia configuraciones.
    
- Solo eliges el USB **una vez**.
    

---

## 6️⃣ Qué opciones nos podemos encontrar en la BIOS

### 🔍 Las más importantes para tu taller

#### 🥇 Boot Order / Boot Priority

- Cambiar el orden:
    
    1. USB
        
    2. Disco duro
        

O elegir el USB manualmente.

#### 🥈 Secure Boot

- Puede estar:
    
    - Activado (Ventoy lo soporta).
        
    - Bloqueando ISOs no firmadas.
        
- A veces hay que:
    
    - Desactivarlo.
        
    - O confirmar clave de Ventoy.
        

#### 🥉 Legacy / UEFI Mode

- Ventoy funciona en ambos.
    
- Mejor **UEFI** si existe.
    

---

## 7️⃣ Flujo completo de arranque (explicado paso a paso)

![https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Flow-diagram-computer-booting-sequences.svg/960px-Flow-diagram-computer-booting-sequences.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Flow-diagram-computer-booting-sequences.svg/960px-Flow-diagram-computer-booting-sequences.svg.png)

![https://i.sstatic.net/QIzyq.png](https://images.openai.com/static-rsc-1/Y81UcJW7HHDOp7gcG5mLbD-Q9tSUi0ZtaIcUr7kXr-b1yu3DV51fcH2ff-Gj-TmZasl_ILDFjmUxhKfvqahrTlnJqVkSfcJPWI5HOswU5u7R741l2B1Q2UmyTSpTdAF-qKMqYmbtvW6pBE-qYoZAgQ)

![https://scaler.com/topics/images/boot-sequence-in-os.webp](https://scaler.com/topics/images/boot-sequence-in-os.webp)

4

1. Enciendes el ordenador.
    
2. Arranca BIOS/UEFI.
    
3. Detecta dispositivos de arranque.
    
4. Seleccionas el USB (o está primero).
    
5. Se carga Ventoy.
    
6. Ventoy muestra el menú.
    
7. Seleccionas Ubuntu ISO.
    
8. Ubuntu se carga en RAM (modo Live).
    
9. Escritorio listo.
    

---

## 8️⃣ Recomendaciones prácticas para el taller

### ✅ Antes del taller

- Probar Ventoy en:
    
    - BIOS legacy
        
    - UEFI con Secure Boot
        
- Llevar **1 USB de respaldo**.
    
- Tener ISOs comprobadas.
    

### ✅ Durante el taller

- Explicar:
    
    - “No instalar, solo Probar Ubuntu”.
        
- Usar el **Boot Menu**, no cambiar BIOS si se puede.
    
- Ir despacio en la primera máquina (en vivo).
    

### ❌ Evitar

- Equipos con contraseña BIOS.
    
- Portátiles de empresa/colegio sin permisos.
    

---

## 🎯 Frase-resumen para tus alumnos

> “Ventoy es un pequeño sistema que arranca antes que Windows y nos permite probar Linux sin tocar el ordenador.”

## Qué está pasando realmente cuando eliges “Instalar Ubuntu”

Aunque el menú diga _Instalar_, lo que ocurre es esto:

1. Desde **Ventoy** se arranca la ISO oficial de **Ubuntu**.
    
2. Ubuntu arranca en **modo Live** (en RAM).
    
3. Se carga el escritorio **temporal**, sin modificar discos.
    
4. En ese escritorio existe un programa llamado **Ubiquity / Ubuntu Installer**.
    
5. **Nada se escribe en el disco** hasta fases muy avanzadas.
    

👉 Es decir:  
**“Instalar Ubuntu” = entrar en un entorno Live con el instalador disponible**, no ejecutarlo.

---

## 🧠 Fases reales de riesgo (muy importante)

![https://upload.wikimedia.org/wikipedia/commons/d/d5/Ubuntu_Desktop_11.04_Live_CD_-_Welcome_screen.png](https://upload.wikimedia.org/wikipedia/commons/d/d5/Ubuntu_Desktop_11.04_Live_CD_-_Welcome_screen.png)

![https://i.sstatic.net/ZTyHs.png](https://images.openai.com/static-rsc-1/UcXHt6ra4tuCQ_JMeHQt9fWtKDZS40OYoVtmTs2rFWdMEW0zSQbHaK96uXGyh2BloWoXTQcSY1mSNKy1Gt1fK0-fKe7J_Rhc_Doe5_CVW0zsdQmWsE9ASZObmU7dA5eUZ3hDBN1z68R6ORxbHY9bIQ)

![https://i.sstatic.net/H8Vwj.png](https://images.openai.com/thumbnails/url/WlcEvXicu5mZUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw5JdzYKd08uz9L1djVIzorK98iOzCkPNSsrTc3JTwv3zC33D_L0j6wsiTJJtPALS_dUKwYAZsImpA)

4

El instalador tiene **varias barreras conscientes**. Para que ocurra un desastre, alguien tendría que:

1. Abrir el instalador manualmente.
    
2. Elegir idioma y teclado.
    
3. Elegir **tipo de instalación**.
    
4. Seleccionar explícitamente:
    
    - “Borrar disco e instalar Ubuntu”.
        
5. Confirmarlo.
    
6. Esperar a que empiece la copia de datos.
    

⚠️ **El punto de no retorno real es el botón “Instalar ahora” tras el particionado.**  
Antes de eso:

- No se escriben particiones.
    
- No se formatea nada.
    

---

## 🔐 Seguridad práctica: ¿puede pasar “sin querer”?

### ❌ No, por varias razones:

- No hay instalación automática.
    
- Hay **pantallas explícitas de advertencia**.
    
- El lenguaje es claro (“borrar disco”).
    
- Requiere **varias decisiones conscientes**.
    
- Requiere tiempo (no es instantáneo).
    

📌 **No existe una combinación de clics rápidos que borre un disco por accidente.**

---

## 🧑‍🏫 Cómo decirlo bien en clase (frase perfecta)

Te dejo una formulación **clara, honesta y tranquilizadora**:

> “Aunque ponga _Instalar Ubuntu_, en realidad solo estamos arrancando un sistema temporal en memoria.  
> El ordenador no se modifica a menos que alguien complete voluntariamente todo el asistente de instalación y acepte borrar o cambiar el disco.  
> Si no haces eso, no pasa absolutamente nada.”

Si quieres dejarlo aún más blindado:

> “Habría que ignorar varios avisos muy claros para causar un problema.”

---

## 🛡️ Recomendaciones extra (opcional, pero pro)

Si quieres **máxima seguridad psicológica**:

- Di explícitamente:
    
    > “No vamos a abrir el icono de instalar.”
    
- O incluso:
    
    - Quita el acceso directo del escritorio (se puede).
        
    - O usa la opción **“Probar Ubuntu”** si la ISO lo muestra.
        

Pero **no es estrictamente necesario**.

# Minitaller para usar la Terminal
## Qué es la terminal (5 min)

La terminal es una **interfaz de texto** para comunicarse con el sistema operativo.  
No sustituye lo gráfico: lo complementa y lo potencia.

En GNU/Linux y Unix, un shell es un programa que se utiliza para ingresar comandos escritos por un usuario en la
terminal y los traduce como un conjunto de instrucciones para que puedan ser entendidas por el sistema operativo.
Consiste en la interfaz mediante la cual el usuario del sistemas operativos puede comunicarse con su núcleo a través
de instrucciones que le aporta al intérprete y por extensión, ejecutar dichas órdenes o programas (internos/propios
del shell o externos/instalados) como herramientas que le permiten controlar el funcionamiento de la computadora
entre otras cosas. En pocas palabras, le permite al usuario darles ordenes a una maquina.
Las órdenes se introducen siguiendo la sintaxis incorporada por dicho intérprete.

Abrir terminal y mostrar:

`whoami pwd`

- `whoami` → usuario actual
- `pwd` → directorio en el que estamos

Explicar el prompt:

`usuario@equipo:~$`

- `~` → directorio personal
- `$` → usuario normal
---

## Navegación por directorios (10 min)

### Ver qué hay en un directorio

`ls ls -l ls -a`

- `ls` → lista archivos
- `-l` → formato largo
- `-a` → incluye ocultos

---
### Moverse por el sistema

`cd cd .. cd / cd ~`

- `cd` → cambiar directorio
- `..` → subir un nivel
- `/` → raíz
- `~` → home del usuario
---
### Saber dónde estás
`pwd`

---

## Crear y manipular archivos (10 min)

### Crear directorios y archivos

`mkdir taller cd taller touch archivo.txt ls`
- `mkdir` → crear carpeta
- `touch` → crear archivo vacío
---

### Editar un archivo

`nano archivo.txt`

- escribir texto
- `Ctrl + O` guardar
- `Ctrl + X` salir
---

### Mostrar contenido por pantalla

`cat archivo.txt`
---

### Copiar, mover y borrar

`cp archivo.txt copia.txt mv copia.txt movido.txt rm movido.txt`

- `cp` → copiar
- `mv` → mover / renombrar
- `rm` → borrar archivo

---

## `sudo` y permisos

Linux separa **usuario normal** y **administrador**.

Probar:
`apt update`
→ error de permisos

Solución:

`sudo apt update`

- `sudo` → ejecutar como administrador
    
- pide contraseña (no la muestra)
    

Regla básica:

> No usar `sudo` si no es necesario.

---

## 5️⃣ Instalar software con `apt` (8 min)

### Buscar un paquete

`apt search cowsay`

### Instalar

`sudo apt install cowsay`

### Probar

`cowsay "Hola Linux" cowsay -f tux "Ya sé usar la terminal"`

Mensaje clave:

> Instalar software desde terminal es rápido, seguro y controlado.

---

## 6️⃣ Manuales: `man` (3 min)

Todos los comandos importantes tienen documentación.

`man ls`

Navegación:

- flechas → mover
    
- `/texto` → buscar
    
- `q` → salir
    

Idea clave:

> No hay que memorizar, hay que saber **buscar ayuda**.

---

## 7️⃣ Ver principio, final y líneas concretas de un archivo (5 min)

### Principio del archivo

`head archivo.txt head -5 archivo.txt`

### Final del archivo

`tail archivo.txt tail -3 archivo.txt`

### Línea concreta (ejemplo línea 7)

`sed -n '7p' archivo.txt`

---

## 8️⃣ Buscar texto con `grep` (10 min)

`grep` **busca patrones de texto y devuelve líneas**.

### Búsqueda básica

`grep Ana datos.csv`

### Ignorar mayúsculas/minúsculas

`grep -i ana datos.csv`

### Mostrar número de línea

`grep -n Ana datos.csv`

### Contar líneas que coinciden

`grep -c Ana datos.csv`

### Mostrar lo que NO coincide

`grep -v Soporte datos.csv`

### Buscar en una carpeta

`grep -r Ana data/`

### Expresiones simples

`grep "^Ana" datos.csv     # empieza por grep "Madrid$" datos.csv # termina en`

Idea clave:

> `grep` selecciona líneas.  
> Otros comandos hacen otras cosas.

---

## 9️⃣ Procesos: ver qué está corriendo (7 min)

### Procesos en vivo

`top`

Salir con `q`.

---

### Foto de procesos

`ps aux`

Buscar un proceso:

`ps aux | grep firefox`

---

## 🔟 Crear y matar procesos (10 min)

### Crear un proceso controlado

`sleep 300`

Buscarlo:

`ps aux | grep sleep`

---

### Matar proceso (forma correcta)

`kill PID`

### Forzar (último recurso)

`kill -9 PID`

### Matar por nombre

`killall sleep`

Regla de oro:

> Primero `kill`, solo si no responde `kill -9`.

---

## 1️⃣1️⃣ Script “noise” (reto práctico)

Ejecutar:

`./scripts/noise.sh`

Objetivo:

- identificar el proceso
    
- matarlo correctamente
    

Combina:

- ejecución
    
- `ps`
    
- `grep`
    
- `kill`

# Dos retos
