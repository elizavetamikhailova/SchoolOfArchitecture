## Контейнерная диаграмма ограниченного контекста коммуникаций

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
    
    Container(Communications, "Communications System", "Система, отвечающая за коммуникации между спикерами и администраторами", "Java/Kotlin, Spring Boot", $tags = "microService")


    System_Boundary(c2, "Внешний контекст") {
        Container(OrganizationZone, "Organization Zone System", "Система, отвечающая за орагнизационную зону", "Java/Kotlin, Spring Boot", $tags = "microService")
    }
}

System_Boundary(c3, "Data") { 
        ContainerDb(CommunicationsDB, "Communications Storage", "Хранение коммуникации между спикерами и администраторами", "CassandraDB", $tags = "storage")
}

Rel_D(Customer, WebApp, "Использует")

Rel_D(WebApp, Communications, "Использует для отображения сообщений в чате между спикерами и администраторами", "Websocket API")
Rel_D(Communications, CommunicationsDB, "Сохраняет и получает данные о сообщениях в чате между спикерами и администраторами")

Rel_R(Communications, OrganizationZone, "Использует для получения данных о спикерах, заявках и админах", "gRPC")

SHOW_LEGEND()
@enduml
```