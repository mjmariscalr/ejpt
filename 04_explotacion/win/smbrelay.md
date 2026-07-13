# SMB relay attack

Un **SMB Relay Attack** es un tipo de ataque de red en el que un atacante **intercepta el tráfico SMB (Server Message Block)**, lo manipula y lo retransmite a un servidor legítimo para obtener acceso no autorizado a recursos o realizar acciones maliciosas. Este tipo de ataque es común en redes **Windows**, donde SMB se utiliza para compartir archivos, impresoras y otros servicios de red.

Funcionamiento de un ataque SMB Relay:

- **Intercepción:** El atacante se sitúa entre el cliente y el servidor (*Man-in-the-Middle*). Esto puede lograrse mediante técnicas como **ARP Spoofing**, **DNS Poisoning** o configurando un servidor SMB malicioso.
- **Captura de la autenticación:** Cuando el cliente se conecta al servidor mediante SMB, envía sus datos de autenticación. El atacante captura esta información, que puede incluir los **hashes NTLM (NT LAN Manager)**.
- **Retransmisión al servidor legítimo:** En lugar de descifrar el hash NTLM, el atacante lo retransmite a otro servidor que confía en el origen de la conexión, haciéndose pasar por el usuario autenticado.
- **Obtención de acceso:** Si el ataque tiene éxito, el atacante obtiene acceso a los recursos del servidor, como archivos, bases de datos o privilegios administrativos. Esto puede facilitar el movimiento lateral dentro de la red y comprometer otros sistemas.
