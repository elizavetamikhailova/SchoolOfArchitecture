## Контейнерная диаграмма ограниченного контекста лекций

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
    Container(Lectures, "Lectures System", "Система, отвечающая за информацию о лекциях", "Java/Kotlin, Spring Boot", $tags = "microService")
}

System_Boundary(c3, "Data") {
    ContainerDb(LecturesDB, "Lectures Storage", "Хранение информации о лекциях", "CassandraDB", $tags = "storage")
}

Rel_D(Customer, WebApp, "Использует")

Rel_D(WebApp, Lectures, "Использует для того, чтобы пользователь мог оставить свой отзыв о конкретной лекции", "REST API")
Rel_D(WebApp, Lectures, "Использует для того, чтобы получить информацию о лекции", "REST API")
Rel_D(Lectures, LecturesDB, "Сохраняет и получает данные о лекциях")

SHOW_LEGEND()
@enduml
```