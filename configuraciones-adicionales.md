# Configuraciones adicionales en Windows 11 para laboratorio de análisis de malware

Los sistemas operativos modernos incorporan diferentes mecanismos de seguridad diseñados para proteger al usuario frente a malware, exploits, accesos no autorizados y comportamientos sospechosos.

Desde el punto de vista defensivo, estas protecciones son necesarias y deben permanecer activas en cualquier equipo personal, profesional o productivo. Sin embargo, en un laboratorio controlado de análisis dinámico de malware, algunas de estas medidas pueden impedir que una muestra se ejecute o que se observe su comportamiento real.

Por este motivo, en una máquina virtual aislada con Windows 11 puede ser útil deshabilitar temporalmente determinadas protecciones.

---

## Advertencia importante

Estas configuraciones solo deben aplicarse en:

- Máquinas virtuales.
- Laboratorios aislados.
- Entornos de pruebas.
- Sistemas desechables o restaurables mediante snapshots.

Nunca deben aplicarse en:

- Equipos personales.
- Equipos de trabajo.
- Sistemas productivos.
- Máquinas con datos sensibles.
- Equipos conectados a redes reales sin control.

Antes de modificar cualquier configuración, se recomienda crear un snapshot de la máquina virtual.

---

## Recomendación previa para Windows 11 en laboratorio

Antes de deshabilitar protecciones, lo recomendable es preparar la máquina virtual de Windows 11 de forma segura.

### Buenas prácticas iniciales

- Crear una máquina virtual dedicada solo al laboratorio.
- No iniciar sesión con cuentas personales.
- No usar OneDrive ni sincronización en la nube.
- No compartir carpetas con el equipo anfitrión.
- No copiar muestras al sistema real.
- Usar red interna o aislada siempre que sea posible.
- Crear snapshots antes de ejecutar cualquier muestra.
- Restaurar la máquina tras cada análisis.

---

# Componentes principales que pueden deshabilitarse

## 1. UAC — User Account Control

`UAC` significa `User Account Control`, o Control de Cuentas de Usuario.

Es una protección de Windows que solicita confirmación cuando una acción necesita permisos administrativos.

### Para qué sirve

- Controlar cambios importantes en el sistema.
- Evitar modificaciones no autorizadas.
- Solicitar confirmación antes de elevar privilegios.
- Reducir la ejecución automática de acciones administrativas.

En un laboratorio de malware, `UAC` puede interferir cuando una muestra intenta modificar partes sensibles del sistema o ejecutar acciones con privilegios elevados.

---

## Cómo deshabilitar UAC en Windows 11

### Método gráfico

1. Abrir el menú de inicio de Windows 11.
2. Buscar:

   `Cambiar configuración de Control de cuentas de usuario`

3. Abrir la opción encontrada.
4. Mover el deslizador hasta abajo del todo:

   `No notificarme nunca`

5. Pulsar en:

   `Aceptar`

6. Confirmar la acción si Windows lo solicita.
7. Reiniciar la máquina virtual.

---

## Cómo volver a habilitar UAC

1. Abrir de nuevo:

   `Cambiar configuración de Control de cuentas de usuario`

2. Subir el deslizador al nivel recomendado por defecto.
3. Pulsar en:

   `Aceptar`

4. Reiniciar Windows 11.

---

## 2. Microsoft Defender Antivirus

`Microsoft Defender Antivirus` es el antivirus integrado en Windows 11.

### Para qué sirve

- Detectar malware conocido.
- Analizar archivos descargados o copiados.
- Bloquear muestras sospechosas.
- Eliminar archivos maliciosos.
- Proteger en tiempo real el sistema.

En un laboratorio de malware, Defender puede eliminar la muestra antes de poder analizarla.

---

## Cómo deshabilitar Microsoft Defender en Windows 11

### Método gráfico

1. Abrir el menú de inicio.
2. Buscar y abrir:

   `Seguridad de Windows`

3. Entrar en:

   `Protección contra virus y amenazas`

4. Dentro de `Configuración de antivirus y protección contra amenazas`, pulsar:

   `Administrar la configuración`

5. Desactivar las siguientes opciones:

   - `Protección en tiempo real`
   - `Protección proporcionada en la nube`
   - `Envío automático de muestras`
   - `Protección contra alteraciones`

> Nota: en algunas versiones de Windows 11, la opción `Protección contra alteraciones` debe desactivarse antes de que ciertos cambios tengan efecto.

---

## Cómo deshabilitar Microsoft Defender mediante PowerShell

Abrir PowerShell como administrador y ejecutar:

```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```

Este comando desactiva la protección en tiempo real.

---

## Cómo volver a habilitar Microsoft Defender

Abrir PowerShell como administrador y ejecutar:

```powershell
Set-MpPreference -DisableRealtimeMonitoring $false
```

Después, revisar en `Seguridad de Windows` que estén activadas las opciones principales de protección.

---

## 3. Firewall de Windows Defender

El firewall de Windows controla el tráfico de red entrante y saliente.

### Para qué sirve

- Bloquear conexiones no autorizadas.
- Filtrar tráfico de red.
- Restringir puertos.
- Controlar servicios expuestos.
- Proteger frente a accesos externos.

En un laboratorio puede impedir conexiones entre Kali Linux y Windows 11, bloquear `ping` o dificultar pruebas de conectividad.

---

## Cómo deshabilitar el Firewall en Windows 11

### Método gráfico

1. Abrir el menú de inicio.
2. Buscar:

   `Firewall de Windows Defender`

3. Abrir la opción:

   `Firewall de Windows Defender`

4. En el menú lateral, pulsar:

   `Activar o desactivar Firewall de Windows Defender`

5. Seleccionar:

   - `Desactivar Firewall de Windows Defender` en red privada.
   - `Desactivar Firewall de Windows Defender` en red pública.

6. Pulsar:

   `Aceptar`

---

## Cómo deshabilitar el Firewall mediante CMD

Abrir CMD como administrador y ejecutar:

```cmd
netsh advfirewall set allprofiles state off
```

---

## Cómo volver a habilitar el Firewall

Abrir CMD como administrador y ejecutar:

```cmd
netsh advfirewall set allprofiles state on
```

---

## 4. DEP — Data Execution Prevention

`DEP` significa `Data Execution Prevention`.

Es una protección que impide ejecutar código en zonas de memoria que no deberían ser ejecutables.

### Para qué sirve

- Reducir el riesgo de explotación de vulnerabilidades.
- Dificultar ataques de tipo buffer overflow.
- Bloquear ejecución de shellcode en memoria.
- Proteger frente a ejecución arbitraria de código.

En análisis de exploits o malware avanzado, `DEP` puede alterar o impedir ciertos comportamientos.

---

## Cómo deshabilitar DEP en Windows 11

Abrir CMD como administrador y ejecutar:

```cmd
bcdedit.exe /set {current} nx AlwaysOff
```

Después, reiniciar Windows 11.

---

## Cómo comprobar el estado de DEP

Abrir CMD como administrador y ejecutar:

```cmd
bcdedit /enum
```

Buscar la línea relacionada con:

```text
nx
```

Si aparece `AlwaysOff`, DEP está deshabilitado.

---

## Cómo volver a habilitar DEP

Abrir CMD como administrador y ejecutar:

```cmd
bcdedit.exe /set {current} nx OptIn
```

Después, reiniciar Windows 11.

---

## 5. ASLR — Address Space Layout Randomization

`ASLR` significa `Address Space Layout Randomization`.

Es una técnica de seguridad que aleatoriza las direcciones de memoria usadas por procesos, librerías y componentes del sistema.

### Para qué sirve

- Dificultar la explotación de vulnerabilidades.
- Evitar direcciones de memoria predecibles.
- Reducir la fiabilidad de exploits.
- Complicar ataques basados en reutilización de direcciones.

En laboratorios de explotación o reversing, `ASLR` puede dificultar la repetición exacta de un comportamiento.

---

## Cómo deshabilitar ASLR en Windows 11

### Método gráfico

1. Abrir el menú de inicio.
2. Buscar y abrir:

   `Seguridad de Windows`

3. Entrar en:

   `Control de aplicaciones y navegador`

4. Buscar la sección:

   `Protección contra vulnerabilidades`

5. Pulsar:

   `Configuración de protección contra vulnerabilidades`

6. Entrar en:

   `Configuración del sistema`

7. Localizar la opción:

   `Aleatorizar diseños de memoria para imágenes`

   o en inglés:

   `Force randomization for images (Mandatory ASLR)`

8. Cambiar la configuración a:

   `Desactivado de forma predeterminada`

9. Reiniciar Windows 11.

---

## Cómo volver a habilitar ASLR

1. Volver a:

   `Seguridad de Windows > Control de aplicaciones y navegador > Protección contra vulnerabilidades`

2. Entrar en:

   `Configuración de protección contra vulnerabilidades`

3. Restaurar la opción de ASLR a su valor predeterminado.
4. Reiniciar Windows 11.

---

## 6. Windows Update

`Windows Update` es el servicio encargado de actualizar Windows 11.

### Para qué sirve

- Instalar parches de seguridad.
- Corregir vulnerabilidades.
- Actualizar controladores.
- Actualizar componentes internos del sistema.
- Mejorar estabilidad y compatibilidad.

En un laboratorio, una actualización automática puede modificar el entorno, cambiar configuraciones o alterar la reproducibilidad de una prueba.

---

## Cómo deshabilitar Windows Update en Windows 11

### Método gráfico

1. Pulsar:

   `Win + R`

2. Escribir:

   `services.msc`

3. Pulsar:

   `Enter`

4. Buscar el servicio:

   `Windows Update`

5. Hacer doble clic sobre él.
6. En `Tipo de inicio`, seleccionar:

   `Deshabilitado`

7. Pulsar:

   `Detener`

8. Pulsar:

   `Aplicar`

9. Pulsar:

   `Aceptar`

---

## Cómo deshabilitar Windows Update mediante CMD

Abrir CMD como administrador y ejecutar:

```cmd
sc stop wuauserv
sc config wuauserv start= disabled
```

---

## Cómo volver a habilitar Windows Update

Abrir CMD como administrador y ejecutar:

```cmd
sc config wuauserv start= demand
sc start wuauserv
```

Después, revisar manualmente Windows Update desde:

`Configuración > Windows Update`

---

## 7. Configurar la red como privada

En Windows 11, una red puede estar configurada como pública o privada.

Para un laboratorio interno, suele ser más cómodo configurarla como red privada, ya que permite ciertas comunicaciones locales entre máquinas virtuales.

### Para qué sirve

- Facilitar comunicación entre Kali Linux y Windows 11.
- Permitir pruebas de conectividad.
- Mejorar descubrimiento de red.
- Evitar bloqueos excesivos en entornos internos.

---

## Cómo configurar la red como privada en Windows 11

1. Abrir:

   `Configuración`

2. Entrar en:

   `Red e Internet`

3. Seleccionar la conexión activa:

   `Ethernet`

4. En `Tipo de perfil de red`, seleccionar:

   `Red privada`

---

## Cómo volver a red pública

1. Abrir:

   `Configuración > Red e Internet > Ethernet`

2. En `Tipo de perfil de red`, seleccionar:

   `Red pública`

---

## 8. Habilitar ICMP para permitir ping

Windows 11 puede bloquear las peticiones `ICMP`, lo que impide hacer `ping` desde otra máquina virtual.

Permitir `ICMP` es útil para comprobar si Kali Linux y Windows 11 se ven dentro de la red del laboratorio.

---

## Cómo permitir ping en Windows 11

Abrir CMD como administrador y ejecutar:

```cmd
netsh advfirewall firewall add rule name="ICMP Allow" protocol=icmpv4 dir=in action=allow
```

---

## Cómo bloquear ping de nuevo

Abrir CMD como administrador y ejecutar:

```cmd
netsh advfirewall firewall delete rule name="ICMP Allow"
```

---

# Configuración recomendada de red en UTM

Para un laboratorio con Kali Linux como máquina atacante o de análisis y Windows 11 como máquina víctima, una arquitectura segura sería la siguiente.

---

## Kali Linux

### Adaptador 1

- Modo: `Shared Network`
- Uso: acceso a Internet mediante NAT.
- Objetivo: actualizar herramientas, instalar paquetes y descargar dependencias.

### Adaptador 2

- Modo: `Host Network`
- Uso: comunicación con la máquina Windows 11 del laboratorio.
- Objetivo: red interna entre máquinas virtuales.

---

## Windows 11 víctima

### Adaptador 1

- Modo: `Host Network`
- Uso: comunicación interna con Kali Linux.
- Objetivo: permitir análisis controlado sin conexión directa a Internet.

---

# Resultado de la arquitectura recomendada

| Sistema | Internet | Comunicación con laboratorio | Riesgo para el host |
|---|---:|---:|---:|
| Kali Linux | Sí | Sí | Bajo si está bien aislado |
| Windows 11 víctima | No | Sí | Bajo si no hay carpetas compartidas |
| Mac anfitrión | Sí | No debería participar | Protegido |

---

# Por qué Windows 11 víctima no debería tener Internet

La máquina Windows 11 usada como víctima debería estar lo más aislada posible.

Esto reduce el riesgo de que una muestra maliciosa:

- Contacte con servidores reales de comando y control.
- Descargue nuevas cargas maliciosas.
- Exfiltre información.
- Se propague fuera del laboratorio.
- Genere tráfico no controlado hacia Internet.

---

# Recomendaciones finales de seguridad

Antes de ejecutar muestras o realizar pruebas peligrosas:

- Crear un snapshot limpio.
- Confirmar que Windows 11 no tiene Internet si va a ejecutarse malware real.
- Confirmar que no existen carpetas compartidas con el host.
- No usar credenciales personales.
- No abrir cuentas reales dentro de la máquina víctima.
- Documentar cada cambio realizado.
- Restaurar snapshot tras finalizar el análisis.
- Mantener separadas las VMs de laboratorio de cualquier entorno personal o profesional.

---

# Conclusión

Windows 11 incorpora varias protecciones importantes como `UAC`, `Microsoft Defender`, `Firewall`, `DEP`, `ASLR` y `Windows Update`.

Estas medidas son fundamentales en un uso normal del sistema, pero pueden interferir en un laboratorio de análisis dinámico de malware.

Por ello, en entornos controlados, aislados y restaurables mediante snapshots, puede ser útil deshabilitarlas temporalmente para observar mejor el comportamiento de una muestra.

Una vez finalizadas las pruebas, lo recomendable es restaurar la máquina virtual a un snapshot limpio o volver a activar las protecciones modificadas.
