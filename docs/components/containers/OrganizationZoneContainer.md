## Контейнерная диаграмма ограниченного контекста организационной зоны

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(Customer, "Пользователь", "Спикер/Слушатель/Администратор")

System_Boundary(c, "UI") {
    Container(WebApp, "Веб приложение для конференций HelloConf", "html, JavaScript, Angular", "Сайт")
}

System_Boundary(c1, "Services") {
    Container(OrganizationZone, "Organization Zone System", "Система, отвечающая за орагнизационную зону", "Java/Kotlin, Spring Boot", $tags = "microService")
}

System_Boundary(c3, "Data") {
    ContainerDb(OrganizationZoneDB, "Organization Zone Storage", "Хранение данных орагнизационной зоны", "CassandraDB", $tags = "storage")
}


System_Boundary(c2, "Внешняя зависимость") {
    System_Ext(TicketingSystem, "Ticketing System", "Внешняя система по продаже билетов")
}

Rel_D(Customer, WebApp, "Использует")

Rel_D(WebApp, OrganizationZone, "Использует для подачи заявки спикером", "REST API")
Rel_D(WebApp, OrganizationZone, "Использует для регистрации на конференцию слушателя", "REST API")
Rel_D(OrganizationZone, OrganizationZoneDB, "Сохраняет и получает данные орагнизационной зоны")

Rel_L(OrganizationZone, TicketingSystem, "Использует для продажи билетов и получения списка билетов", "REST API, HTTPS")

SHOW_LEGEND()
@enduml
```