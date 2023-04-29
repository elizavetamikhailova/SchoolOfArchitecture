# Контейнерная диаграмма ограниченного контекста конференций

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

    System_Boundary(c3, "Внешний ограниченный контекст") {
        Container(Lectures, "Lectures System", "Система, отвечающая за информацию о лекциях", "Java/Kotlin, Spring Boot", $tags = "microService")
    }

    Container(Conferences, "Conferences System", "Система, отвечающая за информацию о конференциях и ее участниках", "Java/Kotlin, Spring Boot", $tags = "microService")
}

System_Boundary(c2, "Data") { 
    ContainerDb(ConferencesDB, "Conferences Storage", "Хранение информации о конференциях и ее участниках", "CassandraDB", $tags = "storage")
}

Rel_D(Customer, WebApp, "Использует")

Rel_D(WebApp, Conferences, "Использует для того, чтобы пользователь мог оставить свой отзыв о конференции", "REST API")
Rel_D(Conferences, ConferencesDB, "Сохраняет и получает данные о конференции")

Rel_R(Conferences, Lectures, "Использует для получения информации о лекциях", "gRPC")


SHOW_LEGEND()
@enduml
```
