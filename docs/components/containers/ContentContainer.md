## Контейнерная диаграмма ограниченного контекста контента

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
    Container(Content, "Content System", "Система, отвечающая за формирование контента на сайте", "Java/Kotlin, Spring Boot", $tags = "microService")

        System_Boundary(c2, "Внешний контекст") {
            Container(Conferences, "Conferences System", "Система, отвечающая за информацию о конференциях и ее участниках", "Java/Kotlin, Spring Boot", $tags = "microService")
        }
}

System_Boundary(c3, "Data") { 
    ContainerDb(ContentDB, "Content Storage", "Хранение информации о контенте на сайте", "Redis", $tags = "storage")
}


Rel_D(Customer, WebApp, "Использует")

Rel_D(WebApp, Content, "Использует для отображения информации о конференциях", "REST API")
Rel_D(Content, ContentDB, "Сохраняет и получает данные о контенте на сайте")

Rel_R(Content, Conferences, "Использует для получения контента о конференциях", "gRPC")

SHOW_LEGEND()
@enduml
```