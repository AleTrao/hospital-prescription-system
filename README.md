# Hospital Prescription Management System

Client-server desktop application for managing medical prescriptions in a hospital, built in Java for the **Programming III** course at Universidad Nacional de Costa Rica (UNA).

> Team project. Team: Alejandra Traña Obando, Alejandro Calderón Campos, Luis Hidalgo Calvo.

## Features

- **Role-based access** with login for three user types:
  - **Administrator:** manages staff (doctors, pharmacists) and medications.
  - **Doctor:** creates prescriptions for patients and views statistics dashboards.
  - **Pharmacist:** processes and delivers prescriptions.
- **Prescription workflow** with status tracking: `Created → In process → Ready → Delivered`.
- **Patient and medication management** (create, search, update, delete).
- **Real-time notifications** pushed from the server to connected clients.
- **Charts** of prescription data using JFreeChart.

## Architecture

```
┌────────────────────────┐        TCP sockets         ┌────────────────────────────┐
│  SistemaRecetas-       │   JSON messages (Gson)     │  SistemaRecetas-Backend    │
│  Frontend (Swing)      │ ◄────────────────────────► │  Multithreaded server      │
│  MVC + Proxy pattern   │                            │  Services + DAO layer      │
└────────────────────────┘                            └─────────────┬──────────────┘
                                                                    │
                                                          MySQL  /  XML files
```

- **Backend:** socket server that spawns a thread per client (`HiloCliente`), a service layer, DAOs for MySQL via JDBC, and XML persistence as an alternative data source.
- **Frontend:** Swing views and controllers (MVC); proxy classes wrap each remote call so the UI never deals with sockets directly.
- **Shared protocol:** command constants (`Comandos`) keep requests and responses consistent between client and server.

## Tech Stack

Java 17 · Maven · Swing · MySQL (JDBC) · XML · Gson · JFreeChart · Log4j · Lombok

## Project Structure

```
SistemaRecetas-Backend/
  src/main/java/servidor/
    Modelo/        domain classes (Receta, Paciente, Medico, Medicamento, ...)
    dao/           MySQL data access objects
    persistencia/  SQL connector and XML persistence
    servicio/      business logic
    red/           socket server, client threads, notifications
  data/            XML data files
SistemaRecetas-Frontend/
  src/main/java/cliente/
    Vista/         Swing views (Admin, Médico, Farmacéutico, Login)
    controlador/   controllers
    proxy/         remote service proxies
    red/           socket client
```

## Running Locally

1. Create a MySQL database named `hospital` and set your credentials in
   `SistemaRecetas-Backend/src/main/resources/db.properties`.
2. Start the server: run `MainServidor` in `SistemaRecetas-Backend`.
3. Start one or more clients: run `MainCliente` in `SistemaRecetas-Frontend`
   (connection settings in `cliente.properties`, default `localhost:5000`).
