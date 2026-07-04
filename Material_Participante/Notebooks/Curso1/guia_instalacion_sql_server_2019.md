# Guía de Instalación: Microsoft SQL Server 2019 Standard (Standalone)

Esta guía detalla el proceso paso a paso para realizar una instalación nueva e independiente (Standalone) de **Microsoft SQL Server 2019 Standard Edition** utilizando el asistente de instalación interactivo (interfaz gráfica de usuario - GUI) en sistemas operativos Windows Server.

---

## 1. Requisitos Previos y Preparación

Antes de iniciar la instalación, asegúrese de cumplir con los siguientes requisitos mínimos y recomendaciones de arquitectura.

### 1.1. Requisitos de Hardware y Software
*   **Sistema Operativo:** Windows Server 2016 o superior (Standard o Datacenter), o Windows 10/11 (Professional o Enterprise) para entornos de desarrollo.
*   **Procesador:** Mínimo de 1.4 GHz (Recomendado: 2.0 GHz o superior), arquitectura x64.
*   **Memoria RAM:** Mínimo de 4 GB. Para edición Standard, se recomienda dimensionar según la carga de trabajo (el límite de RAM utilizable por el motor en Standard es de 128 GB).
*   **Espacio en Disco:** Mínimo de 6 GB de espacio libre en la unidad del sistema. Se requiere almacenamiento adicional y dedicado para las bases de datos (Datos, Logs y TempDB).
*   **Sistema de Archivos:** NTFS o ReFS (NTFS es mandatorio para los directorios de instalación del sistema).
*   **Framework:** .NET Framework 4.6 o superior (el instalador suele habilitarlo o solicitarlo si falta).

### 1.2. Cuentas de Servicio y Permisos
Se recomienda crear cuentas de servicio dedicadas en Active Directory (o cuentas locales de usuario si no está en un dominio) para ejecutar los servicios de SQL Server de forma aislada:
*   **Servicio del Motor (SQL Server Database Engine):** p. ej., `DOMINIO\svc-sql-engine`
*   **Servicio del Agente (SQL Server Agent):** p. ej., `DOMINIO\svc-sql-agent`

> [!IMPORTANT]  
> Asegúrese de que la cuenta que ejecutará la instalación tenga privilegios de **Administrador Local** en el servidor de destino.

---

## 2. Proceso de Instalación Paso a Paso

### Paso 1: Ejecutar el Medio de Instalación
1. Monte el archivo ISO de SQL Server 2019 Standard o inserte el medio físico.
2. Ejecute el archivo `setup.exe` como Administrador.
3. En el **Centro de Instalación de SQL Server** (SQL Server Installation Center), haga clic en la pestaña **Instalación** (Installation) en el menú lateral izquierdo.
4. Seleccione la primera opción: **Nueva instalación independiente de SQL Server o agregar características a una instalación existente** (New SQL Server stand-alone installation or add features to an existing installation).

```
+-------------------------------------------------------------+
| Centro de Instalación de SQL Server                         |
+-------------------------------------------------------------+
| Planeación   |  > Nueva instalación independiente de...     |
| Instalación  |    (New SQL Server stand-alone installation) |
| Mantenimiento|                                              |
+-------------------------------------------------------------+
```

### Paso 2: Clave del Producto (Product Key)
1. En la pantalla **Product Key**, elija la opción **Escribir la clave del producto** (Enter the product key) e ingrese la licencia de 25 caracteres correspondiente a su versión Standard.
2. Haga clic en **Siguiente** (Next).

### Paso 3: Términos de Licencia (License Terms)
1. Lea y acepte los Términos de Licencia de Microsoft.
2. Opcional: Marque la casilla para enviar datos de uso a Microsoft si lo considera oportuno.
3. Haga clic en **Siguiente** (Next).

### Paso 4: Reglas Globales y Actualizaciones (Global Rules & Microsoft Update)
1. El instalador ejecutará un chequeo rápido de reglas globales (requisitos del sistema). Si todas pasan con éxito, avanzará automáticamente o mostrará el reporte.
2. En la pantalla **Microsoft Update**, se recomienda marcar la casilla **Use Microsoft Update to check for updates** para garantizar que los Cumulative Updates (CU) y parches de seguridad de SQL Server se descarguen automáticamente en el ciclo de mantenimiento del sistema.
3. Haga clic en **Siguiente** (Next).

### Paso 5: Reglas de Soporte de Instalación (Install Setup Files / Install Rules)
1. El instalador descargará y preparará los archivos de configuración necesarios.
2. Evaluará las reglas del sistema. 
    *   *Nota común:* Es probable que aparezca una advertencia (Warning) sobre el **Windows Firewall**. Esto es normal e indica simplemente que los puertos necesarios para conectar externamente al motor no están abiertos por defecto. No impide continuar la instalación.
3. Asegúrese de que no haya fallas críticas (Errors). Si las hay, resuélvalas antes de continuar. Haga clic en **Siguiente** (Next).

### Paso 6: Selección de Características (Feature Selection)
Seleccione las características específicas requeridas para su servidor. Para una instalación típica standalone del motor de bases de datos, elija:
*   **Servicios de Motor de Base de Datos** (Database Engine Services)
    *   *Opcional:* **Replicación de SQL Server** (SQL Server Replication) si la requiere.
*   **SDK de conectividad de cliente de bases de datos** (Client Tools Connectivity)
*   **Compatibilidad con el SDK de conectividad de cliente de bases de datos** (Client Tools Backwards Compatibility)
*   **Herramientas de conectividad de cliente de bases de datos** (Client Tools SDK)

> [!TIP]  
> Evite seleccionar componentes innecesarios (como Machine Learning Services o servicios de análisis) a menos que se requieran expresamente, para reducir la superficie de ataque y el consumo de recursos del sistema.

Deje las rutas de directorios predeterminadas para los binarios (usualmente `C:\Program Files\Microsoft SQL Server`) y haga clic en **Siguiente** (Next).

### Paso 7: Configuración de la Instancia (Instance Configuration)
Defina si desea utilizar una instancia predeterminada o con nombre:
*   **Instancia predeterminada** (Default instance): Se creará con el nombre de red del servidor y el nombre de instancia `MSSQLSERVER`. (Recomendada si es el único SQL Server en el host).
*   **Instancia con nombre** (Named instance): Introduzca un nombre identificativo (ej. `SQL2019STANDARD`). La cadena de conexión final será `NombreServidor\NombreInstancia`.
*   Haga clic en **Siguiente** (Next).

---

## 3. Configuración del Servidor (Server Configuration)

Esta sección es crucial para el rendimiento y la seguridad operacional.

### 3.1. Cuentas de Servicio y Colación (Collation)

#### Pestaña Service Accounts
Asigne las cuentas de servicio preparadas en el paso 1.2 y defina los tipos de inicio correspondientes:

| Servicio | Cuenta de Servicio Recomendada | Tipo de Inicio |
| :--- | :--- | :--- |
| **SQL Server Agent** | Cuenta de dominio dedicada (ej. `DOMINIO\svc-sql-agent`) | Automatic |
| **SQL Server Database Engine** | Cuenta de dominio dedicada (ej. `DOMINIO\svc-sql-engine`) | Automatic |
| **SQL Server Browser** | `NT AUTHORITY\LOCAL SERVICE` | Disabled (o Manual si requiere instancias con nombre expuestas externamente) |

> [!IMPORTANT]  
> Active la casilla **Conceder el privilegio de realizar tareas de mantenimiento de volumen al servicio del motor de base de datos** (*Grant Perform Volume Maintenance Tasks privilege to SQL Server Database Engine Service*). Esto habilita la **Inicialización Instantánea de Archivos (IFI)**, lo que acelera dramáticamente la creación y crecimiento de archivos de bases de datos y la restauración de backups.

#### Pestaña Collation (Colación)
*   La colación predeterminada suele heredarse de la configuración regional del sistema operativo (por ejemplo, `Modern_Spanish_CI_AS` o `SQL_Latin1_General_CP1_CI_AS`).
*   Verifique con sus desarrolladores o el proveedor del software que se alojará si requieren una colación específica (sensible a mayúsculas/minúsculas, acentos, etc.).
*   Haga clic en **Siguiente** (Next).

---

## 4. Configuración del Motor de Base de Datos (Database Engine Configuration)

Esta ventana cuenta con varias pestañas de parametrización crítica.

### 4.1. Server Configuration (Autenticación)
1.  **Modo de autenticación** (Authentication Mode):
    *   **Modo mixto** (Mixed Mode): Permite autenticarse mediante Windows (Directorio Activo) y cuentas nativas de SQL Server (como el usuario administrador `sa`). Es el más recomendado para compatibilidad general.
2.  **Contraseña del Administrador del Sistema (sa)**: Ingrese una contraseña de alta complejidad y guárdela de forma segura.
3.  **Administradores de SQL Server** (Specify SQL Server administrators): Haga clic en el botón **Agregar usuario actual** (Add Current User) para añadir su cuenta de administrador actual y use el botón **Agregar** (Add) para asignar grupos del Active Directory de administradores de base de datos (DBAs).

### 4.2. Data Directories (Directorios de Datos)
Es una buena práctica de rendimiento de I/O separar los tipos de almacenamiento en discos físicos independientes (por ejemplo, LUNs de almacenamiento diferentes):

*   **Data root directory (Directorio raíz de datos):** `C:\Program Files\Microsoft SQL Server`
*   **User database directory (Datos de usuario):** Unidad de almacenamiento rápido dedicada a lectura/escritura (Ej: `D:\SQLData`).
*   **User database log directory (Logs de transacciones):** Unidad de almacenamiento dedicada a escrituras secuenciales (Ej: `L:\SQLLogs`).
*   **Backup directory (Copias de seguridad):** Unidad dedicada a resguardo o ruta UNC de red (Ej: `B:\SQLBackups`).

### 4.3. TempDB
La base de datos `TempDB` es compartida por todos los usuarios del sistema y requiere una configuración óptima para evitar cuellos de botella (tempdb allocation contention):

*   **Número de archivos (Number of files):** De forma predeterminada, el instalador detecta la cantidad de núcleos lógicos y configura el número de archivos correspondientemente.
    *   *Regla general:* Configure 1 archivo por cada CPU lógica hasta un máximo de 8. Si experimenta contención posterior, aumente en bloques de 4.
*   **Tamaño inicial (Initial size):** Establezca un tamaño inicial generoso (ej. `1024 MB` o `8192 MB` por archivo) para evitar autocompresiones y fragmentación física durante la ejecución del servidor.
*   **Crecimiento automático (Autogrowth):** Configure en megabytes en lugar de porcentajes (ej. `256 MB` o `512 MB` de crecimiento fijo).
*   **Directorio de TempDB:** Ubique estos archivos en discos de estado sólido muy veloces (SSD/NVMe) (Ej: `T:\SQLTempDB`).

### 4.4. MaxDOP (Maximum Degree of Parallelism)
El instalador de SQL Server 2019 calcula el valor recomendado de MaxDOP según la arquitectura de NUMA y sockets del procesador detectado.
*   Siga las recomendaciones indicadas por el instalador. Generalmente, el valor límite no debe exceder de 8 por nodo NUMA, o manténgalo en el valor calculado por el asistente gráfico.

### 4.5. Memory (Configuración de Memoria)
Por defecto, SQL Server está configurado para consumir casi toda la memoria física del host. Para evitar la inanición del sistema operativo de Windows Server, configure límites realistas:
1.  Seleccione la opción **Recomendado** (Recommended).
2.  El asistente calculará el límite máximo de memoria basándose en la cantidad de memoria física disponible en el sistema.
3.  **Límite Máximo (Max server memory):** Asegúrese de dejar libre entre **2 GB y 4 GB** para el sistema operativo y otros servicios del servidor. 
    *   *Ejemplo:* En un host con 64 GB de RAM, asigne un valor máximo de memoria a SQL Server de aproximadamente `57344 MB` a `61440 MB` (56 a 60 GB).

### 4.6. FileStream (Opcional)
*   Active la característica **Habilitar FILESTREAM para acceso de E/S de Transact-SQL** (*Enable FILESTREAM for Transact-SQL access*) únicamente si su base de datos necesita almacenar archivos binarios de gran volumen directamente en el sistema de archivos de Windows integrado con SQL Server. Si no es así, déjelo desactivado.

Haga clic en **Siguiente** (Next).

---

## 5. Resumen e Instalación final

### Paso 8: Resumen y Ejecución
1.  En la pantalla **Listo para instalar** (Ready to Install), revise el listado de componentes que serán instalados y las rutas de configuración.
2.  Tome nota de la ruta del archivo de configuración `ConfigurationFile.ini` mostrado al final de la pantalla (le servirá si desea replicar esta instalación de forma desatendida en el futuro).
3.  Haga clic en el botón **Instalar** (Install).

El asistente mostrará una barra de progreso que completará el despliegue. Dependiendo de los recursos del host, este proceso toma entre 10 y 25 minutos.

### Paso 9: Finalización y Verificación
1.  Una vez concluida la tarea, la pantalla **Complete** mostrará el estatus de la instalación para cada característica seleccionada. Verifique que todas muestren un estatus de **Correcto** (Succeeded).
2.  Haga clic en **Cerrar** (Close) y reinicie el sistema operativo del servidor si el asistente lo solicita.

---

## 6. Tareas Post-Instalación Fundamentales

### 6.1. Instalar SQL Server Management Studio (SSMS)
A partir de las versiones recientes, SSMS no se instala junto con el motor de SQL Server. 
1.  Descargue la versión más reciente de SSMS desde la documentación oficial de Microsoft.
2.  Ejecute el instalador gráfico y siga los pasos sencillos para instalar la interfaz de administración en su servidor o en su estación de trabajo local.

### 6.2. Habilitar Protocolo TCP/IP
Para que aplicaciones externas se conecten al servidor de base de datos, el protocolo TCP/IP debe habilitarse manualmente:
1.  Abra el **Administrador de configuración de SQL Server** (SQL Server Configuration Manager).
2.  Despliegue **Configuración de red de SQL Server** (SQL Server Network Configuration) > **Protocolos de [NombreInstancia]** (Protocols for [InstanceName]).
3.  Haga clic secundario sobre **TCP/IP** y seleccione **Habilitar** (Enable).
4.  Reinicie el servicio de **SQL Server (MSSQLSERVER)** desde el panel de servicios para aplicar los cambios.

### 6.3. Configuración del Firewall de Windows
Si el servidor requiere conexiones remotas, debe abrir el puerto estándar en el cortafuegos del host:
1.  Abra PowerShell como Administrador.
2.  Ejecute el siguiente comando para abrir el puerto de comunicación TCP predeterminado (1433):

```powershell
New-NetFirewallRule -DisplayName "SQL Server Engine (TCP 1433)" -Direction Inbound -Protocol TCP -LocalPort 1433 -Action Allow
```

3.  *Opcional (si se usan instancias con nombre y servicio SQL Server Browser):* Ejecute este comando para abrir el puerto UDP de escucha de instancias:

```powershell
New-NetFirewallRule -DisplayName "SQL Server Browser (UDP 1434)" -Direction Inbound -Protocol UDP -LocalPort 1434 -Action Allow
```

### 6.4. Validar Conexión
1.  Abra **SQL Server Management Studio (SSMS)**.
2.  En el cuadro de diálogo de conexión, especifique el nombre del servidor local (o `.`) y seleccione **Autenticación de Windows** (Windows Authentication) o **Autenticación de SQL Server** usando el usuario `sa` y la contraseña definida durante la instalación.
3.  Haga clic en **Conectar** (Connect). Si se establece la conexión con éxito, la instalación se ha completado de forma correcta.
