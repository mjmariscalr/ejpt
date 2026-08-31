# Alternate data streams

**Alternate Data Streams (ADS)** son un atributo del sistema de archivos **NTFS (New Technology File System)** y fueron diseñados para proporcionar compatibilidad con el **HFS (Hierarchical File System) de macOS**.

Cualquier archivo creado en una unidad formateada con NTFS tendrá dos flujos (*streams*) diferentes:

- **Flujo de datos (Data stream):** flujo predeterminado que contiene los datos del archivo.
- **Flujo de recursos (Resource stream):** normalmente contiene los metadatos del archivo.

Los atacantes pueden utilizar **ADS** para ocultar código malicioso o archivos ejecutables dentro de archivos legítimos con el objetivo de evadir la detección. Esto puede hacerse almacenando el código malicioso o los ejecutables en el atributo de archivo **Resource Stream (metadatos)** de un archivo legítimo. Esta técnica suele utilizarse para evadir **antivirus (AV) básicos basados en firmas** y herramientas de **análisis estático**.



[⟵ Anterior](12_clearing.md) | [Siguiente ⟶](.md)
