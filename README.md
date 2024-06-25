# Log4net
## Logging usando Log4net y C# 
-------------------------------------------------
Los logs en una aplicación son muy importantes ya que con ellos podemos rastrear problemas o ver la funcionalidad de nuestra aplicación, entre otras cosas.
Log4net, un producto gratuito de Apache.org, es una librería configurable de manera muy sencilla mediante un fichero XML.

- Documentación: https://logging.apache.org/log4net/release/manual/introduction.html

 
## Instalación y guía rápida
Los pasos esenciales son…
1.	Instalación de paquetes (log4net y log4net.Ext.Json)
2.	log4net.cs 		// Assembly
3.	log4net.config 	// Configuración
4.	Implementación
5.	Visualizar Logs


## 1.Instalación de paquetes (log4net y log4net.Ext.Json)
- Usando  Package Manager Console:
```
Install-Package log4net
Install-Package log4net.Ext.Json
```
- Desde el instalador de paquetes NuGet:

      Instalar “log4net” y “log4net.Ext.Json”


## 2.log4net.cs // Assembly
Este  paso puede obviarse, si al archivo de configuración que se define en el paso 3, le ponemos como nombre **"log4net.config"**

Si no, se debe crear el archivo “log4net.cs”, en el directorio raíz, con el siguiente código anexado

```
[assembly: log4net.Config.XmlConfigurator(ConfigFile = "log4net.config")]
```

## 3.log4net.config // Configuración
Crear el archivo “log4net.config”, en el directorio raíz, con el siguiente código anexado (ejemplo básico con salida a un archivo .txt)

```
<?xml version="1.0" encoding="utf-8" ?>
<log4net>
	<appender name="RollingLogFileAppender" type="log4net.Appender.RollingFileAppender">
		<lockingModel type="log4net.Appender.FileAppender+MinimalLock" />
		<file value="Logs/Apilog_" />
		<param name="DatePattern" value="yyyy-MM-dd.\tx\t"/>
		<param name="StaticLogFileName" value="false" />
		<appendToFile value="true" />
		<rollingStyle value="Date" />
		<maxSizeRollBackups value="10" />
		<maximumFileSize value="15MB" />
		<layout type="log4net.Layout.PatternLayout">
			<param name="ConversionPattern" value="%-5p%d{ yyyy-MM-dd HH:mm:ss } - Clase: %logger - Mensaje: %message%newline"/>
		</layout>
	</appender>
	<root>
		<level value="ALL" />
		<appender-ref ref="RollingLogFileAppender" />
	</root>
</log4net>
```




### Detalle del "log" que se muestra o guarda 

  **%date %level %logger - %message%newline**

  ejemplo como se guarda: **2024-06-19 15:27:09,855 DEBUG log4netAdvanced.Program - Some debug log**


Se puede agregar más información, a continuación se indican las más relevantes…
https://logging.apache.org/log4net/log4net-1.2.13/release/sdk/log4net.Layout.PatternLayout.html

| Pattern | Efecto |
| ------------- | ------------- |
|%date | Muestra la fecha y la hora en que se generó el mensaje de log  |
|%level| Muestra el nivel de log (por ejemplo, DEBUG, INFO, WARN, ERROR, FATAL). |
|%logger|Muestra el nombre del logger que generó el mensaje de log.|
|%message|Muestra el mensaje de log que fue pasado al logger.|
|%newline|Inserta un salto de línea.|
|%timestamp|Muestra el timestamp en milisegundos desde el inicio de la aplicación.|
|%username|Muestra el nombre del usuario bajo el cual se está ejecutando la aplicación.|
|%location|Muestra la ubicación en el código (incluye el nombre de la clase, el nombre del método y el número de línea).|
|%file|Muestra el nombre del archivo desde el cual se genera el mensaje de log.|
|%line|Muestra el número de línea desde el cual se genera el mensaje de log.|


## 4.Implementación

**En Program.cs**
```
// inyectar dependencia Log4net
builder.Logging.AddLog4Net();

```

**Declaramos el logger, en cada conrtolador, de la siguiente manera:**

```
private static readonly log4net.ILog logger = log4net.LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);
```



- Luego ya podemos hacer registro invocando el objeto logger, seguido del nivel que se requiera.en este ejemplo se muestra un error.

```
logger.Error(String.Format("Error accrued at {0}", DateTime.Now));
```


## 5.Visualizar Logs 

Luego podemos visualizar los logs en la carpeta que especificamos en **log4net.config**

- en este ejemplo: **Logs/Apilog_**

