# Bilabonnement.dk

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Flask](https://img.shields.io/badge/flask-%23000.svg?style=for-the-badge&logo=flask&logoColor=white)
![SQLite](https://img.shields.io/badge/sqlite-%2307405e.svg?style=for-the-badge&logo=sqlite&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Swagger](https://img.shields.io/badge/-Swagger-%23Clojure?style=for-the-badge&logo=swagger&logoColor=white)

## Formål

Vi ønsker at udarbejde et minimal viable product som kan akkommodere den resterende rejse for bilen. Fra afhentningssted, til 
tilbagelevering og videre til skaderegistrering.

## Opfyldelse af krav
I opgaven blev der stillet nogle krav som vi har løst gennem følgende gateways:

**Data registrering** : [sales-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/sales-gateway)

**Skade og udbedring** : [maintenance-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/maintenance-gateway)

**forretningsudviklere** : [finance-gateway](https://github.com/Bilabonnement-eksamensopgave-2024/finance-gateway)

## User-guide for medarbejdere

Bekræft hos din arbejdsgiver at du er blevet oprettet i som medarbejder i systemet til den rigtige afdeleing.

1. ### Brug Postman eller lignende
   `method` POST
   
   `request` /din_afdelings_gateway_url/login
   
 ```json
{
    "email": "din_email@mail.com",
    "password": "paswoard123"
}
```
2. ### Du kan nu tilgå din afdelings endpoints
    

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
        get_subscription()  List
        get_subscription_by_id(id : Int)  Dict
        get_active_subscriptions()  Int
        update_subscription(id : Int, data : Any)  String
        delete_item_by_id(id : Int)  String
        add_subscription(data : Any)  String
        heath_check()  String
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
        get_cars()  List
        get_available_cars()  List
        get_car_by_id(id: Int)  Dict
        update_car(id : Int, data : Any)  String
        delete_car_by_id(id : Int)  String
        add_car(data : Any)  String
    }

    class damage_reports {
        Int damage_report_id
        Int subscription_id
        Int car_id
        Date report_date
        String description
        Int damage_type_id
        get_damage_reports()  List
        get_damage_reports_by_id(id: Int)  Dict
        get_damage_reports_by_subscription_id(id: Int)  List
        get_total_price_of_subscription_damages(id: Int)  Int
        get_damage_reports_by_carid(id: Int)  List
        get_the_repair_cost_by_subid(id : Int)  List
        get_the_repair_cost_by_carid(id : Int)  List
        update_damage_report(id : Int, updated_fields : Dict) String
        delete_damage_report(id : Int) String
        add_new_damage_report(data : Any) String
    }

    class damage_types {
        Int damage_type_id
        String damage_type
        String severity
        get_all_damage_types()  List
        find_type_by_id(id : Int)  Dict
        update_type(id : Int, data : Any)  String
        delete_type_by_id(id : Int)  String
        add_new_types(data : Any)  String
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

### Roller

Da vi arbejder med JWT authentication har vi valgt at en user skal have en rolle før de kan tilgå en gateway. De roller og deres gateways er således:

| Gateway | Role Required |
|------------|----------------|
| admin-gateway | `admin` |
| finance-gateway | `finance` |
| sales-gateway | `sales` |
| maintenance-gateway | `maintenance` |

---
## Arkitektur Diagram
![arkitektur diagram](arkitektur_diagram.png)


Diagrammet viser, hvordan systemets mikroservice arkitektur er struktureret med de forskellige gateways og mikroservices, samt hvordan de relaterer til hinanden og databaserne.

## CI/CD pipeline

```mermaid
graph LR
    A["Develop Flask API in VS Code"] --> B["Create Dockerfile"]
    B --> C["Push to GitHub"]
    C --> D["DevOps"]
    D --> D1["Build Docker Image"]
    D --> D2["Push to DockerHub"]
    D2 --> E["Azure Web App pulls image from DockerHub"]
```

Diagrammet viser en CI/CD-pipeline til deployment af en Flask API-applikation. Her er en kort forklaring:

1. **Udvikling**  
   Flask API udvikles i Visual Studio Code (eller et andet IDE).

2. **Dockerfile oprettes**  
   En Dockerfile skrives for at containerisere applikationen.

3. **Push til GitHub**  
   Koden pushes til et GitHub-repository for versionskontrol og samarbejde.

4. **DevOps-processen**  
   - **Build Docker Image**: En Docker-container bygges automatisk ud fra koden og Dockerfilen.
   - **Push to DockerHub**: Docker-image pushes til DockerHub, som fungerer som en container registry.

5. **Deployment**  
   Azure Web App trækker Docker-imaget fra DockerHub og deployer applikationen, så den er tilgængelig for brugere.

Denne pipeline automatiserer processen fra udvikling til deployment, hvilket gør det nemt og hurtigt at rulle opdateringer ud.

