
# Instrucciones de Gestión de Usuarios, Grupos y Permisos

## Parte 1: Gestión de Usuarios

### 1. Creación de Usuarios
Comandos utilizados:
```sh
sudo useradd usuario1
sudo useradd usuario2
sudo useradd usuario3
```

### 2. Asignación de Contraseñas
Comandos utilizados:
```sh
echo "nueva_contraseña1" | sudo passwd --stdin usuario1
echo "nueva_contraseña2" | sudo passwd --stdin usuario2
echo "nueva_contraseña3" | sudo passwd --stdin usuario3
```

### 3. Información de Usuarios
Comando utilizado:
```sh
id usuario1
```

Resultado:
```sh
uid=1001(usuario1) gid=1001(usuario1) groups=1001(usuario1)
```

### 4. Eliminación de Usuarios
Comando utilizado:
```sh
sudo userdel usuario3
sudo mkdir /home/usuario3
```

## Parte 2: Gestión de Grupos

### 1. Creación de Grupos
Comandos utilizados:
```sh
sudo groupadd grupo1
sudo groupadd grupo2
```

### 2. Agregar Usuarios a Grupos
Comandos utilizados:
```sh
sudo usermod -aG grupo1 usuario1
sudo usermod -aG grupo2 usuario2
```

### 3. Verificar Membresía
Comandos utilizados:
```sh
groups usuario1
groups usuario2
```

Resultados:
```sh
usuario1 : usuario1 grupo1
usuario2 : usuario2 grupo2
```

### 4. Eliminar Grupo
Comando utilizado:
```sh
sudo groupdel grupo2
```

## Parte 3: Gestión de Permisos

### 1. Creación de Archivos y Directorios
Comandos utilizados (como usuario1):
```sh
sudo -u usuario1 touch /home/usuario1/archivo1.txt
echo "contenido" | sudo -u usuario1 tee /home/usuario1/archivo1.txt

sudo -u usuario1 mkdir /home/usuario1/directorio1
sudo -u usuario1 touch /home/usuario1/directorio1/archivo2.txt
```

### 2. Verificar Permisos
Comandos utilizados:
```sh
ls -l /home/usuario1/archivo1.txt
ls -ld /home/usuario1/directorio1
```

Resultados:
```sh
-rw-r--r-- 1 usuario1 usuario1 9 ago  4 12:34 /home/usuario1/archivo1.txt
drwxr-xr-x 2 usuario1 usuario1 4096 ago  4 12:34 /home/usuario1/directorio1
```

### 3. Modificar Permisos usando `chmod` con Modo Numérico
Comando utilizado:
```sh
sudo chmod 640 /home/usuario1/archivo1.txt
```

### 4. Modificar Permisos usando `chmod` con Modo Simbólico
Comando utilizado:
```sh
sudo chmod u+x /home/usuario1/directorio1/archivo2.txt
```

### 5. Cambiar el Grupo Propietario
Comando utilizado:
```sh
sudo chgrp grupo1 /home/usuario1/directorio1/archivo2.txt
```

### 6. Configurar Permisos de Directorio
Comando utilizado:
```sh
sudo chmod 740 /home/usuario1/directorio1
```

### 7. Comprobación de Acceso
Comandos utilizados (como usuario2):
```sh
sudo -u usuario2 cat /home/usuario1/archivo1.txt
sudo -u usuario2 cat /home/usuario1/directorio1/archivo2.txt
```

### 8. Verificación Final
Comandos utilizados:
```sh
ls -l /home/usuario1/archivo1.txt
ls -l /home/usuario1/directorio1/archivo2.txt
ls -ld /home/usuario1/directorio1
```

Resultados:
```sh
-rw-r----- 1 usuario1 usuario1 9 ago  4 12:34 /home/usuario1/archivo1.txt
-rwxr--r-- 1 usuario1 grupo1   0 ago  4 12:34 /home/usuario1/directorio1/archivo2.txt
drwxr----x 2 usuario1 usuario1 4096 ago  4 12:34 /home/usuario1/directorio1
```

### ¿Por qué es importante gestionar correctamente los usuarios y permisos en un sistema operativo?
Gestionar correctamente los usuarios y permisos en un sistema operativo es crucial para garantizar la seguridad, evitando accesos no autorizados y protegiendo datos sensibles. También asegura la integridad de los datos, previniendo modificaciones accidentales o malintencionadas. Además, permite una mejor trazabilidad de las acciones realizadas, facilitando auditorías y el cumplimiento de normativas. Finalmente, optimiza la eficiencia operativa al permitir a los usuarios realizar su trabajo con los permisos adecuados, sin necesidad de intervención constante de los administradores del sistema.

### ¿Qué otros comandos o técnicas conocen para gestionar permisos en Linux?
Además de los comandos básicos, existen herramientas y técnicas como las listas de control de acceso (ACLs) para un control más detallado, la configuración de sudo para ejecutar comandos como superusuario, cambiar propietarios y grupos de archivos, usar máscaras de permisos predeterminados (umask), aplicar políticas de seguridad avanzadas con herramientas como SELinux o AppArmor, realizar auditorías de permisos, y utilizar scripts para automatizar la gestión de permisos en múltiples archivos.