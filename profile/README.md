# Bilabonnement.dk

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Flask](https://img.shields.io/badge/flask-%23000.svg?style=for-the-badge&logo=flask&logoColor=white)
![SQLite](https://img.shields.io/badge/sqlite-%2307405e.svg?style=for-the-badge&logo=sqlite&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Swagger](https://img.shields.io/badge/-Swagger-%23Clojure?style=for-the-badge&logo=swagger&logoColor=white)

## Formål

## Opfyldelse af krav
I opgaven blev der stillet nogle krav som vi har løst gennem følgende gateways:

**Data registrering** : [sales-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/sales-gateway)

**Skade og udbedring** : [maintenance-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/maintenance-gateway)

**forretningsudviklere** : [finance-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/finance-gateway)

## Domain-model

```mermaid
classDiagram
    class subscriptions {
        Int subscription_id
        Int car_id
        Date subscription_start_date
        Int subscription_end_date
        Int subscription_duration_months
        Int monthly_subscription_price
        Int contracted_km
        Int monthly_subscription_price
        Boolean has_delivery_insurance
        get_subscription() List
        get_subscription_by_id(Int) Dict
        get_subscription_by_userid(Int) List
        get_subscription_by_car_id(Int) List
        get_car_by_subscription_id(subscription_id: Int) Dict
        update_subscription(updated_subscription: Dict) String
        delete_subscription(subscription_id: Int) String
        add_subscription(new_subscription: Dict) String
    }

    class cars {
        Int car_id
        String car_brand
        String car_model
        String fuel_type
        Date registration_date
        Int purchase_price
        Int km_driven_since_last_end_subscription
        Boolean is_available
        get_cars() List
        get_available_cars() List
        get_car_by_id(car_id: Int) Dict
        update_car(updated_car: Dict) String
        delete_car(car_id: Int) String
        add_car(new_car: Dict) String
    }

    class damage_reports {
        Int damage_report_id
        Int subscription_id
        Int car_id
        Date report_date
        String description
        Int damage_type_id
        get_damage_reports() List
        get_damage_reports_by_id(id: Int) Dict
        get_damage_reports_by_subscription_id(id: Int) List
        get_total_price_of_subscription_damages(subscription_id: Int) Int
        get_damage_reports_by_car_id(id: Int) List
        update_damage_report(updated_damage_report: Dict) String
        delete_damage_report(id: Int) String
        add_damage_report(new_damage_report: Dict) String
    }

    class damage_types {
        Int damage_type_id
        String damage_type
        String severity
        get_damage_types() List
        get_damage_type_by_id(id: Int) Dict
        update_damage_type(updated_damage_type: Dict) String
        delete_damage_type(id: Int) String
    }

    subscriptions "1" -- "*" cars
    damage_reports "*" -- "1" damage_types
    subscriptions "1" -- "*" damage_reports
    cars "1" -- "*" damage_reports
```

## Oversigt over Repositories

| Github repo | Azure deployed |
|------------|----------------|
| [user-microservice](https://github.com/Bilabonnement-eksamensopgave-2024/user-microservice) | [Azure](https://user-microservice-d6f9fsdkdzh7hndv.northeurope-01.azurewebsites.net/) |
| [abonnement-microservice](https://github.com/Bilabonnement-eksamensopgave-2024/abonnement-microservice) | [Azure](https://abonnement-microservice-dkeda4efcje4aega.northeurope-01.azurewebsites.net/) |
| [bil-microservice](https://github.com/Bilabonnement-eksamensopgave-2024/bil-microservice) | [Azure](https://car-microservice-ayhzdgdrfxgrdgby.northeurope-01.azurewebsites.net/) |
| [skade-microservice](https://github.com/Bilabonnement-eksamensopgave-2024/skade-microservice) | [Azure](https://skade-microservice-cufpgqgfcufqa8er.northeurope-01.azurewebsites.net/) |
| [admin-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/admin-gateway) | [Azure-Health](https://admin-gateway-fqevcraygyfvafe2.northeurope-01.azurewebsites.net/health) |
| [finance-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/finance-gateway) | [Azure](https://finance-gateway-b3grdpa6e6bterbg.northeurope-01.azurewebsites.net/) |
| [sales-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/sales-gateway) | [Azure](https://sales-gateway-adcsa0dwahcxhkep.northeurope-01.azurewebsites.net/) |
| [maintenance-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/maintenance-gateway) | [comming soon](TBA.com) |


---
## Arkitektur Diagram

```mermaid
graph TD
    Browser["Browser"] --> FinanceGateway["Finance Gateway"]
    Browser --> MaintenanceGateway["Vedligeholdelse Gateway"]
    Browser --> SalesGateway["Sales Gateway"]
    Browser --> AdminGateway["Admin Gateway"]

    FinanceGateway --> SkadeMicroservice["Skade Microservice"]
    FinanceGateway --> BilMicroservice["Bil Microservice"]

    MaintenanceGateway --> BilMicroservice
    MaintenanceGateway --> AbonnementMicroservice["Abonnement Microservice"]

    SalesGateway --> AbonnementMicroservice

    AdminGateway --> UserMicroservice["User Microservice"]

    SkadeMicroservice --> SkadeDB["Skade Database"]
    BilMicroservice --> BilDB["Bil Database"]
    AbonnementMicroservice --> AbonnementDB["Abonnement Database"]
    UserMicroservice --> UserDB["User Database"]
```

Diagrammet viser, hvordan systemets mikroservice arkitektur er struktureret med de forskellige gateways og mikroservices, samt hvordan de relaterer til hinanden og databaserne.

## CI/CD pipeline

