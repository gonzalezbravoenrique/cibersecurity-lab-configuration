# Laboratorio aislado Kali + Windows en UTM

## Objetivo

Separar dos redes:

1. Red con Internet  
   (para actualizar herramientas, descargar paquetes, etc.)

2. Red interna aislada entre Kali y Windows  
   (para ataques, análisis y malware sin tocar tu Mac)

---

# Arquitectura recomendada

## Kali Linux

- Adaptador 1 → Shared Network (NAT)
- Adaptador 2 → Host Network

## Windows

- Adaptador 1 → Host Network

---

# Resultado esperado

- Kali tiene Internet
- Windows NO tiene Internet
- Kali y Windows sí se ven entre ellas
- Tu Mac queda aislado del laboratorio

---

# Cómo configurarlo en UTM

## 1. Crear Host Network

En:

`UTM → Settings → Host Networks`

Crea una red nueva:

- Nombre: `LAB`

> Nota: UTM no permite definir la subred aquí.  
> La red se define manualmente mediante las IPs asignadas dentro de las VMs.

---

# 2. Configuración de Kali

## Network 1 (Internet)

- Mode: `Shared Network`
- Emulated Network Card: `VirtIO`

Esto le dará Internet mediante NAT.

---

## Network 2 (Laboratorio interno)

Añade un segundo adaptador:

- Mode: `Host Network`
- Host Network: `LAB`
- Emulated Network Card: `VirtIO`

---

# 3. Configuración de Windows

## Único adaptador

- Mode: `Host Network`
- Host Network: `LAB`
- Emulated Network Card: `VirtIO`

Windows quedará totalmente aislado de Internet.

---

# 4. Configuración IP manual

> Host Network NO proporciona DHCP automáticamente.

---

# Configuración en Kali

## Ver interfaces

```bash
ip a
```

Normalmente:

- `eth0` → Internet (Shared Network)
- `eth1` → Red LAB

---

## Asignar IP a `eth1`

```bash
sudo ip addr add 192.168.50.10/24 dev eth1
```

---

## Levantar interfaz

```bash
sudo ip link set eth1 up
```

---

# Configuración en Windows

En el adaptador de red:

- IP: `192.168.50.20`
- Máscara: `255.255.255.0`
- Puerta de enlace: vacía
- DNS: vacío

---

# Verificación

## Desde Kali

```bash
ping 192.168.50.20
```

## Desde Windows

```cmd
ping 192.168.50.10
```

---

# Resultado final

| Sistema | Internet | Ve a Kali | Ve a Windows | Ve al Mac |
|---|---|---|---|---|
| Kali | Sí | — | Sí | No |
| Windows | No | Sí | — | No |
| Mac | — | Aislado | Aislado | — |

---

# Importante

## NO uses:

- Bridged
- Shared Network directamente en Windows
- VLAN Emulada para aislamiento

---

# ¿Por qué?

## Bridged

Mete la VM directamente en tu red real.

---

## Shared Network

Permite salida NAT e Internet.

---

## VLAN Emulada

NO aísla del host.

---

# Conclusión

`Host Network` es lo que realmente crea el laboratorio aislado y seguro para practicar:

- Pentesting
- Malware
- Wireshark
- Metasploit
- Active Directory Labs
- Análisis forense
- Transferencia de payloads entre VMs

sin afectar a tu Mac ni a tu red doméstica.
