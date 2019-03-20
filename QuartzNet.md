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
- El método de ejecución de un Job debe estar contenido en un bloque try-catch que gestione las posibles excepciones. Si un Job lanza una exception, Quartz lo ejecutará de nuevo inmediatamente, por lo que es mejor manejar el error y reprogramar el job o executar algún otro.
- Un job puede ser ejecutado mas de una vez por situaciones imprevistas, por lo que se debe programar un Job de tal manera que su trabajo se idempotente, que es la propiedad para realizar una acción determinada varias veces y aun así conseguir el mismo resultado que se obtendría si se realizase una sola vez.

### Convenciones
- Los Jobs a implementar usando la interface IJob deben estar dentro de una carpeta llamada **Jobs** en la raiz del proyecto, y ese esta carpeta no debe haber otro tipo de clases o funcionalidades, incluso helpers. :fa-folder:
- La clase que implementa IJob debe tener el postfijo Job, por ejemplo ReadRoomStatus**Job**.

### The Quartz API
The key interfaces and classes of the Quartz API are:
- IScheduler - the main API for interacting with the scheduler.
- IJob - an interface to be implemented by components that you wish to have executed by the scheduler.
- IJobDetail - used to define instances of Jobs.
- ITrigger - a component that defines the schedule upon which a given Job will be executed.
- JobBuilder - used to define/build JobDetail instances, which define instances of Jobs.
- TriggerBuilder - used to define/build Trigger instances.

### Using Quartz
Before you can use the scheduler, it needs to be instantiated. To do this, you use an implementor of ISchedulerFactory. Once a scheduler is instantiated, it can be started, placed in stand-by mode, and shutdown. Note that once a scheduler is shutdown, it cannot be restarted without being re-instantiated. Triggers do not fire (jobs do not execute) until the scheduler has been started, nor while it is in the paused state.


```csharp
// construct a scheduler factory
NameValueCollection props = new NameValueCollection
{
    { "quartz.serializer.type", "binary" }
};
StdSchedulerFactory factory = new StdSchedulerFactory(props);

// get a scheduler
IScheduler sched = await factory.GetScheduler();
await sched.Start();

// define the job and tie it to our HelloJob class
IJobDetail job = JobBuilder.Create<HelloJob>()
    .WithIdentity("myJob", "group1")
    .Build();

// Trigger the job to run now, and then every 40 seconds
ITrigger trigger = TriggerBuilder.Create()
    .WithIdentity("myTrigger", "group1")
    .StartNow()
    .WithSimpleSchedule(x => x
        .WithIntervalInSeconds(40)
        .RepeatForever())
.Build();
  
await sched.ScheduleJob(job, trigger);
```

Jobs can be created and stored in the job scheduler independent of a trigger, and many triggers can be associated with the same job. Another benefit of this loose-coupling is the ability to configure jobs that remain in the scheduler after their associated triggers have expired, so that that it can be rescheduled later, without having to re-define it. It also allows you to modify or replace a trigger without having to re-define its associated job.

##### Identities
Jobs and Triggers are given identifying keys as they are registered with the Quartz scheduler. The keys of Jobs and Triggers (JobKey and TriggerKey) allow them to be placed into ‘groups’ which can be useful for organizing your jobs and triggers into categories such as “reporting jobs” and “maintenance jobs”. The name portion of the key of a job or trigger must be unique within the group.

### Triggers
The block of code that builds the job definition is using JobBuilder using fluent interface to create the product, IJobDetail. Likewise, the block of code that builds the trigger is using TriggerBuilder’s fluent interface and extension methods that are specific to given trigger type. Possible schedule extension methods are:

- WithCalendarIntervalSchedule
- WithCronSchedule
- WithDailyTimeIntervalSchedule
- WithSimpleSchedule

The **DateBuilder** class contains various methods for easily constructing DateTimeOffset instances for particular points in time.

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

### Proyectos de Orión que implementan Quartz.Net
- Orion.Services.Integration.PBX
- Orion.Services.Nightly.Hotelero
- Orion.Services.Nightly.PagaLlegar