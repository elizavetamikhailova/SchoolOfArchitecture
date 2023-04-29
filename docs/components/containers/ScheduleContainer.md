## Контейнерная диаграмма ограниченного контекста расписания

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
    Container(Schedule, "Schedule System", "Система, отвечающая за расписание", "Java/Kotlin, Spring Boot", $tags = "microService")
}

System_Boundary(c3, "Data") {
    ContainerDb(ScheduleCache, "Schedule Cache", "Хранение расписания", "Redis", $tags = "storage")

    ContainerDb(ScheduleDB, "Schedule Storage", "Хранение расписания", "PostgreSQL", $tags = "storage")
}

Rel_D(Customer, WebApp, "Использует")

Rel_D(WebApp, Schedule, "Использует для того, чтобы спикер мог выбрать свободный слот в расписании", "Web Socket API")
Rel_D(Schedule, ScheduleDB, "Сохраняет и получает данные о расписании")
Rel_D(Schedule, ScheduleCache, "Кеширует и получает данные о расписании")

SHOW_LEGEND()
@enduml
```