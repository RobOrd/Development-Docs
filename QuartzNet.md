# Quartz.NET

### Versión y Configuración
La versión actual de Quartz.NET asi como otras librerias en uso para el proyecto es:
```xml
<package id="Quartz" version="3.0.7" targetFramework="net461" />
<package id="NLog" version="4.5.11" targetFramework="net46" />
```

El App.Config deberá quedar como sigue en base a los requerimientos actuales de logging.
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="nlog" type="NLog.Config.ConfigSectionHandler, NLog"/>
  </configSections>
  
  <appSettings>
    ...
  </appSettings>

  <nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >
    <targets async="true">
      <target xsi:type="ColoredConsole" name="console" useDefaultRowHighlightingRules="True" />
    </targets>

    <rules>
      <logger name="*" minlevel="Trace" writeTo="console"/>
    </rules>
  </nlog>
  
  <startup> 
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6" />
  </startup>
  
</configuration>
```

### Best Practices
- Solo almacenar tipos de datos primitivos (incluyendo string) en los **JobDataMap**
- Nunca escribir datos directamente a la base de datos, se debe usar solo el API; hacerlo puede resultar en problemas extraños, corrupción de datos y hasta Dead-Locks.
- Evitar programar Jobs para ejecutarse cerca de los horarios de transición de Daylight Savings Time. Por ejemplo se podría dar el caso de que la 1:05 AM ocurra dos veces.
- Evitar crear Jobs que tengan que esperar a que se cumpla ciertas condiciones, es mejor reprogramar la ejecución del Job para dejar los Threads disponibles para ser usado por otro Job.
- El método de ejecución de un Job debe estar contenido en un bloque try-catch que gestione las posibles excepciones. Si un Job lanza una exception Quartz lo ejecutará de nuevo inmediatamente, por lo que es mejor manejar el error y reprogramar el job o executar algún otro.
- Un job puede ser ejecutado mas de una vez por situaciones imprevistas, por lo que se debe programar un Job de tal manera que su trabajo se idempotente, que es la propiedad para realizar una acción determinada varias veces y aun así conseguir el mismo resultado que se obtendría si se realizase una sola vez.

### Convenciones
- Los Jobs a implementar usando la interface IJob deben estar dentro de una carpeta llamada Jobs en la raiz del proyecto, y ese esta carpeta no debe haber otro tipo de clases o funcionalidades, incluso helpers. :fa-folder:
- La clase que implementa IJob debe tener el postfijo Job, por ejemplo ReadRoomStatus**Job**.

### The Quartz API
The key interfaces and classes of the Quartz API are:
- IScheduler - the main API for interacting with the scheduler.
- IJob - an interface to be implemented by components that you wish to have executed by the scheduler.
- IJobDetail - used to define instances of Jobs.
- ITrigger - a component that defines the schedule upon which a given Job will be executed.
- JobBuilder - used to define/build JobDetail instances, which define instances of Jobs.
- TriggerBuilder - used to define/build Trigger instances.

### Time Zones
Para especificar una zona horaria distinta a la del sistema se puede usar CronScheduleBuilder con el método InTimeZone, para conocer las zonas horarias se puede consultar https://www.timeanddate.com/time/zone/mexico.

Las zonas estandar para México son:
UTC | Time Zone
---|---
UTC -8 | Pacific Standard Time
UTC -7 | Mountain Standard Time
UTC -6 | Central Standard Time
UTC -5 | Eastern Standard Time

```csharp
var tzId = "Central America Standard Time"

var trigger = TriggerBuilder.Create()
    .WithSchedule(CronScheduleBuilder
        .InTimeZone(TimeZoneInfo.FindSystemTimeZoneById(tzId)))
    .Build();
```
