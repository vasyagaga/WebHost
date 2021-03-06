# WebHost

## Program

``` csharp
        public static async Task Main(string[] args)
        {
            await ServiceHost.Create<Startup>(args)
                .UseRabbitMq()
                .SubscribeToCommand<CheckAppointmentCommandModel>()
                .Build()
                .RunAsync();
        }
```

## Startup

``` csharp
public void ConfigureServices(IServiceCollection services)
        {
            services.AddRabbitMq(Configuration);
            services.AddMongoDb(Configuration);
            services.AddScoped<ICommandHandler<CheckAppointmentCommandModel>, CheckAppointmentCommandHandler>();
            services.AddScoped<IAppointmentRepository, AppointmentRepository>();
            services.AddScoped<IDatabaseSeeder, MongoCustomSeeder>();

        }

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.InitilizeDatabase();

            app.UseRouting();

            ...
        }


```

### appsettings.json

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "RabbitMq": {
    "Username": "guest",
    "Password": "guest",
    "VirtualHost": "/",
    "Port": 5672,
    "Hostnames": [ "localhost" ],
    "RequestTimeout": "00:00:10",
    "PublishConfirmTimeout": "00:00:01",
    "RecoveryInterval": "00:00:10",
    "PersistentDeliveryMode": true,
    "AutoCloseConnection": true,
    "AutomaticRecovery": true,
    "TopologyRecovery": true,
    "Exchange": {
      "Durable": true,
      "AutoDelete": true,
      "Type": "Topic"
    },
    "Queue": {
      "AutoDelete": true,
      "Durable": true,
      "Exclusive": true
    }
  },
  "Mongo": {   
    "connectionString": "mongodb://localhost:27017",
    "database": "KIS-Services-Appointment",
    "seed": true
  }
}

```