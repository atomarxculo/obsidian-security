La IP del contenedor es la 172.17.0.2, la cual sale según se despliega el lab.
## Escaneo de puertos

Realizamos el escaneo de puertos:
```bash
nmap -sS 172.17.0.2
```

Resultado:
![[Pasted image 20251201225155.png]]

Obtenemos que tenemos acceso por el puerto 22 (SSH). Hacemos un escaneo más extenso sobre dicho puerto para obtener más información al respecto.
```bash
nmap -sV -p 22 -vvv 172.17.0.2
```

Resultado:
![[Pasted image 20251201225335.png]]

Nos devuelve la versión **OpenSSH 7.7**, la cual tiene una vulnerabilidad [CVE-2018-15473](https://www.incibe.es/incibe-cert/alerta-temprana/vulnerabilidades/cve-2018-15473), la cual nos permite hacer una enumeración de usuarios con los que poder conectarnos.

Vamos a probar a hacer un ataque de fuerza bruta usando el listado de top usernames que da SecList.

```bash
hydra -l /usr/share/seclist/SecLists-master/Usernames/top-usernames-shortlist.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
```

Resultado:
![[Pasted image 20251201232645.png]]
Paré el proceso porque el equipo iba a despegar.

Probamos a conectarnos directamente con el usuario root y vemos que ya tenemos acceso al servidor.

![[Pasted image 20251201232017.png]]

