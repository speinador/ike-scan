# ike-scan

**ike-scan** es una herramienta de línea de comandos utilizada para **descubrir y enumerar dispositivos VPN/IPsec** que usan el protocolo **IKE (Internet Key Exchange)**. Es útil en auditorías de red, pentesting (con autorización) y tareas de reconocimiento para identificar endpoints que responden a solicitudes IKEv1/IKEv2 y recopilar información como versiones, identificadores y timbres de respuesta.

---

## Advertencia legal y ética

Usá `ike-scan` únicamente en entornos de laboratorio o sobre redes y sistemas para los cuales tengás **autorización explícita**. El escaneo no autorizado puede ser ilegal y/o violar políticas de uso. Este README tiene fines educativos y defensivos.

---

## Qué puede hacer ike-scan

- Detectar dispositivos que responden a paquetes IKE (routers, firewalls, concentradores VPN).
- Enumerar versiones, identificadores (IDi/IDr) y cookies IKE.
- Probar la existencia de respuestas a diferentes propuestas de IKE (métodos de autenticación, cifrados).
- Integrarse en flujos de reconocimiento para mapear infraestructura VPN/IPsec en una red.

---

## Instalación

En sistemas Debian/Ubuntu y derivados podés instalarlo desde los repositorios oficiales:

```bash
sudo apt update
sudo apt install ike-scan
```

En macOS (con Homebrew):

```bash
brew install ike-scan
```

También se puede compilar desde el código fuente descargándolo desde el repositorio oficial o la página del proyecto, siguiendo las instrucciones proporcionadas allí (requisitos: compilador C, libpcap, etc.).

---

## Uso básico

Formato general:

```bash
ike-scan [opciones] <host-or-network>
```

Ejemplos:

- Escanear un host específico (sólo consulta IKE básica):

```bash
ike-scan 192.168.1.1
```

- Escanear una red completa (requiere privilegios para enviar paquetes raw, p. ej. root):

```bash
sudo ike-scan 192.168.1.0/24
```

- Escanear y guardar resultados en un archivo:

```bash
sudo ike-scan 203.0.113.0/24 | tee ike-scan_results.txt
```

---

## Opciones útiles

- `-A` : Activa el modo agresivo (aggressive mode) para solicitar más información (ID, versión).  
- `-M <file>` : Leer las propuestas IKE desde un fichero (útil para pruebas personalizadas).  
- `--show-backoff` : Muestra tiempos de espera y comportamiento ante retransmisiones.  
- `-v` / `-vv` : Verbosidad (muestra más detalles de la comunicación).  
- `--natt` : Añade detección/soporte para NAT-T (NAT Traversal) si es relevante.  
- `-s <src_port>` : Usar un puerto origen específico.  
- `-I <iface>` : Forzar interfaz de red de salida.  

> Para ver todas las opciones consultá `man ike-scan` o `ike-scan --help` en tu sistema.

---

## Interpretación de resultados

La salida típica muestra, por cada host que responde, información como:

- Dirección IP del host que respondió.  
- Cookie IKE (IDi/IDr) y valores de responder/initiator.  
- Mensajes devueltos (por ejemplo, `NOTICE` o `AM`), identificadores de respuesta y, si se usó modo agresivo, el ID y posibles nombres reales (dependiendo de la configuración del dispositivo).

Ejemplo simplificado de salida:

```
192.0.2.1  Main mode response returned (id: 0x1234)  Vendor: Cisco
```

---

## Integración en auditorías y flujos de trabajo

- Combiná `ike-scan` con herramientas como `nmap` para mapear servicios adicionales en hosts que respondan.  
- Analizá los resultados para identificar dispositivos desactualizados o configuraciones que revelen información sensible (por ejemplo, nombres de hosts o identificadores).  
- Usalo como primer paso para decidir si un equipo debe ser analizado más a fondo en un entorno de pruebas autorizado.

---

## Buenas prácticas y seguridad

- Ejecutar desde una máquina en la que confiás y con conexiones autorizadas.  
- Redirigir la salida a archivos y conservar logs para auditoría.  
- Evitar ejecutar escaneos masivos sin coordinación con el equipo de red (puede generar alertas IDS/IPS).  
- Mantener la herramienta actualizada y revisar la documentación oficial para cambios en opciones o comportamiento.

---

## Ejemplo de flujo en laboratorio

1. Ejecutar `ike-scan` contra el rango de laboratorio:  
   ```bash
   sudo ike-scan 10.10.0.0/24 -A | tee ike_lab_results.txt
   ```
2. Revisar `ike_lab_results.txt` para identificar IPs que respondieron y recoger información de vendor/ID.  
3. Marcar hosts para análisis posterior con `nmap -sV` y comprobación manual de configuraciones VPN en un entorno controlado.

---

## Alternativas y herramientas complementarias

- `nmap` con scripts NSE específicos para IKE/IPsec (`ike-version`, `ike-credentials`, etc.).  
- Herramientas especializadas de descubrimiento de VPN y auditoría de IPsec.  
- Análisis manual con `tcpdump`/`wireshark` para inspeccionar paquetes IKE capturados.

---

## Referencias

- Página del proyecto / documentación (buscá el repositorio oficial o la página del paquete para más detalles).  
- `man ike-scan` en tu sistema para opciones completas y ejemplos.

---

## Licencia y atribución

Respetá la licencia del proyecto original. Usá y redistribuí conforme a los términos indicados por los mantenedores.

---

## Contacto / Contribuir

Si querés adaptar este README para tu curso (añadir ejemplos prácticos, labs paso a paso o integración con otras herramientas), puedo generarlo en inglés, añadir ejemplos de nmap, o preparar un laboratorio listo para presentar a tus alumnos.


