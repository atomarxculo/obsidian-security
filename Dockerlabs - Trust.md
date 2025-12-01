La IP del contenedor es la 172.18.0.2, la cual te sale según despliega el lab.
## Escaneo de puertos

Realizamos el escaneo de puertos:
```bash
nmap -sS 172.18.0.2
```

Resultado:
![[Pasted image 20251201161919.png]]
Obtenemos que hay dos puertos abiertos, el 22 (SSH) y 80 (HTTP). Hacemos un escaneo de esos puertos para obtener más información al respecto.
```bash
nmap -sV -p 22,80 -vvv 172.18.0.2
```

Resultado:
![[Pasted image 20251201162117.png]]

Nos conectamos por firefox a la URL y nos sale la página por defecto de Apache2:
![[Pasted image 20251201162217.png]]

Para poder continuar vamos a hacer un escaneo con el comando `gobuster` para obtener un listado de ficheros o directorios a los que se pueda acceder desde el servidor web.
```bash
gobuster dir -u http://172.18.0.2 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
```

Resultado:
![[Pasted image 20251201162613.png]]

Vemos que hay disponibles, `index.html`, que es la página de inicio, `secret.php` y `/server-status`, pero este último nos da un 403.

Si nos conectamos a `secret.php` nos aparece el siguiente mensaje:
![[Pasted image 20251201162835.png]]

Vamos a revisar el código fuente de la web por si encontrásemos algo interesante al respecto, aunque en este caso no es así.
![[Pasted image 20251201162944.png]]
Vamos a probar ahora con **SSH** y con el usuario **mario** que nos ha salido al visitar `secret.php`. Para ello, vamos a usar la herramienta `hydra`para sacar la contraseña con un ataque de fuerza bruta (previamente hay que descomprimir el fichero `/usr/share/wordlists/rockyou.txt.gz`).

```bash
hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://172.18.0.2
```

Resultado:
![[Pasted image 20251201164140.png]]

Probamos a acceder con la contraseña que nos ha salido:
![[Pasted image 20251201164252.png]]

Vamos a comprobar si el usuario puede ejecutar algún binario como sudo con el siguiente comando.
```bash
sudo -l
```

Y vemos que puede ejecutar `vim`
![[Pasted image 20251201164357.png]]

Miramos en la URL https://gtfobins.github.io/ cómo podemos explotar ese acceso sudo a vim. La URL final donde podemos encontrar esto es https://gtfobins.github.io/gtfobins/vim/#sudo
![[Pasted image 20251201164534.png]]

Ejecutamos el primer comando y comprobamos que usuario somos después de eso:
![[Pasted image 20251201164905.png]]

Con esto hemos conseguido escalar privilegios.