# MacOS Setup

Start RabbitMQ
docker run -p 15672:15672 -p 5672:5672 -p 4369:4369 -p 25672:25672 --hostname my-rabbit --name some-rabbit --rm rabbitmq:3-management

Navigate to RabbitMQ Admin and login as guest/guest
http://localhost:15672/#/queues

Start SQL Server
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd" -p 1433:1433 --name sql1 --rm mcr.microsoft.com/mssql/server:2022-latest 

Create SQL Server databases
* TicketInfo_SagaMedium
* StateSaga_SagaMedium
* Ticket_SagaMedium

Run Migrations - dotnet ef database update
* TicketService
* SagaService
* GenerateTicket

Run services to create queues
* TicketService
* SagaService
* GenerateTicket
* EmailService

Run TicketService and POST JSON body to http://localhost:5000/api/ticket
{
    "title": "title-test",
    "email": "scott@firmpilot.com"
}

# Saga Medium
In this repo, the Saga orchestration will be covered, and a simple scenario will be implemented to indicate how it works. In the previous article, MassTransit covered and demonstrated how can send a message through MassTransit and RabbitMQ. The code is available on GitHub and if there are any questions, please feel free to visit my website and contact me. So, let's get started.

## Scenario
In this scenario, three services are going to be used to illustrate the process of performing the saga pattern and managing the consistency of data. The services are:
TicketService: Responsible for registering a ticket
GenerateTicketService: Responsible for generating a ticket
EmailService: Responsible for sending ticket information to the user (to keep the example simple, logging information will be used)

![SagaDiagram](https://github.com/Vahidalizadeh7070/SagaStateMachine_Medium/assets/98050724/dddd4a19-078f-4518-a8b1-74653671e8eb)
As shown in the above image, a client will send a request to TicketService to register a ticket, then a AddTicketEvent will occur and send a message to GenerateTicketService and if the message contains valid values, it will send a message to EmailService to send a confirmation email that contains all information of ticket to the client. If one of the Services of GenerateTicketService or EmailService retrieves an invalid message the Cancel event will occur and incoming data will be removed from all Service's database.

## Prerequires
- C# and .NET Core
- Visual Studio or VSCode
- Entity Framework core or any other ORMs
- SQL or NoSQL databases
- MassTransit
- RabbitMQ
- Docker
- Postman

## YouTube 
https://youtu.be/dTI4FF7VogU 

## Medium 
https://medium.com/@vahidalizadeh1990/implementation-of-saga-orchestration-using-masstransit-dd238530f0d7
