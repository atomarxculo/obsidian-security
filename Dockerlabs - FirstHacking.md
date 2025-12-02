La IP del contenedor es la 172.17.0.2, la cual te sale según se despliega el lab.

## Escaneo de puertos

Realizamos el escaneo de puertos:
```bash
nmap -sS 172.17.0.2
```

Resultado:
![[Pasted image 20251202163719.png]]

Obtenemos que hay un puerto abierto, el 21 (FTP). Hacemos un escaneo del puerto para obtener más información al respecto.
```bash
nmap -sV -p 21 -vvv 172.17.0.2
```

Resultado:
![[Pasted image 20251202163811.png]]

Si hacemos una búsqueda rápida en google sobre "exploit vsftpd 2.3.4", nos sale que está relacionada con el CVE-2011-2523.
En rapid7 cuentan con un módulo para poder explotar dicha vulnerabilidad, https://www.rapid7.com/db/modules/exploit/unix/ftp/vsftpd_234_backdoor/

Abrimos `msfconsole`, cargamos el módulo, seteamos el host destino y ejecutamos.
![[Pasted image 20251202164257.png]]

Con esto ya tenemos acceso root al servidor.