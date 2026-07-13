# Enumeración NetBIOS

**NetBIOS** es una API y un conjunto de protocolos de red que proporcionan servicios de comunicación a través de una red local. Se utiliza principalmente para permitir que las aplicaciones que se ejecutan en diferentes equipos puedan encontrarse e interactuar entre sí dentro de la red.

*Funciones:** NetBIOS ofrece tres servicios principales:

- **Servicio de nombres (NetBIOS-NS):** Permite que los equipos registren, eliminen el registro y resuelvan nombres dentro de una red local.
- **Servicio de datagramas (NetBIOS-DGM):** Admite comunicaciones sin conexión (connectionless) y la difusión de mensajes (*broadcast*).
- **Servicio de sesiones (NetBIOS-SSN):** Admite comunicaciones orientadas a conexión para realizar transferencias de datos más fiables.

**NetBIOS** utiliza habitualmente los puertos **137** (Servicio de nombres), **138** (Servicio de datagramas) y **139** (Servicio de sesiones) sobre los protocolos **UDP** y **TCP**.

Las implementaciones modernas de **Windows** utilizan principalmente **SMB** y pueden funcionar sin **NetBIOS**. Sin embargo, **NetBIOS sobre TCP (puerto 139)** sigue siendo necesario para mantener la compatibilidad con sistemas antiguos, por lo que ambos servicios suelen estar habilitados de forma conjunta.

## NBTScan

NBTScan se utiliza para descubrir equipos en una red local mediante NetBIOS. Funciona mediante el envío de consultas al puerto UDP 137 para obtener información de los dispositivos que tienen NetBIOS habilitado.

Entre otros datos, puede mostrar:

- La dirección IP del equipo.
- El nombre NetBIOS (nombre del ordenador).
- El grupo de trabajo o dominio al que pertenece.
- El tipo de servicio registrado (por ejemplo, servidor de archivos, estación de trabajo, controlador de dominio, etc.).
- La dirección MAC (en muchos casos).

```bash
nbtscan host/red
```

[⟵ Anterior](../03_activa.md#323-enummeración-de-servicios)
