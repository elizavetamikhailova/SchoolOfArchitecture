# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783368
-->
## Контейнерная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(Customer, "Пользователь", "Спикер/Слушатель/Администратор")

System_Boundary(c, "HelloConf") {

    Container(WebApp, "Веб приложение для конференций HelloConf", "html, JavaScript, Angular", "Сайт")

    Container(Lectures, "Lectures System", "Система, отвечающая за информацию о лекциях", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(LecturesDB, "Lectures Storage", "Хранение информации о лекциях", "CassandraDB", $tags = "storage")

    Container(Schedule, "Schedule System", "Система, отвечающая за расписание", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(ScheduleCache, "Schedule Cache", "Хранение расписания", "Redis", $tags = "storage")

    ContainerDb(ScheduleDB, "Schedule Storage", "Хранение расписания", "PostgreSQL", $tags = "storage")

    Container(OrganizationZone, "Organization Zone System", "Система, отвечающая за орагнизационную зону", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(OrganizationZoneDB, "Organization Zone Storage", "Хранение данных орагнизационной зоны", "CassandraDB", $tags = "storage")

    Container(Content, "Content System", "Система, отвечающая за формирование контента на сайте", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(ContentDB, "Content Storage", "Хранение информации о контенте на сайте", "Redis", $tags = "storage")

    Container(Conferences, "Conferences System", "Система, отвечающая за информацию о конференциях и ее участниках", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(ConferencesDB, "Conferences Storage", "Хранение информации о конференциях и ее участниках", "CassandraDB", $tags = "storage")

    Container(Communications, "Communications System", "Система, отвечающая за коммуникации между спикерами и администраторами", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(CommunicationsDB, "Communications Storage", "Хранение коммуникации между спикерами и администраторами", "CassandraDB", $tags = "storage")
}

System_Ext(TicketingSystem, "Ticketing System", "Внешняя система по продаже билетов")

System_Ext(Youtube, "Youtube", "Стриминг видео")

Rel(Customer, WebApp, "Использует")

Rel(WebApp, OrganizationZone, "Использует для подачи заявки спикером", "REST API")
Rel(WebApp, OrganizationZone, "Использует для регистрации на конференцию слушателя", "REST API")
Rel(OrganizationZone, OrganizationZoneDB, "Сохраняет и получает данные орагнизационной зоны")

Rel(WebApp, Content, "Использует для отображения информации о конференциях", "REST API")
Rel(Content, ContentDB, "Сохраняет и получает данные о контенте на сайте")

Rel(WebApp, Schedule, "Использует для того, чтобы спикер мог выбрать свободный слот в расписании", "Web Socket API")
Rel(Schedule, ScheduleDB, "Сохраняет и получает данные о расписании")
Rel(Schedule, ScheduleCache, "Кеширует и получает данные о расписании")

Rel(WebApp, Conferences, "Использует для того, чтобы пользователь мог оставить свой отзыв о конференции", "REST API")
Rel(Conferences, ConferencesDB, "Сохраняет и получает данные о конференции")

Rel(WebApp, Lectures, "Использует для того, чтобы пользователь мог оставить свой отзыв о конкретной лекции", "REST API")
Rel(WebApp, Lectures, "Использует для того, чтобы получить информацию о лекции", "REST API")
Rel(Lectures, LecturesDB, "Сохраняет и получает данные о лекциях")


Rel(WebApp, Youtube, "Использует для отображения трансляций/записей видео", "HTTPS")

Rel(WebApp, Communications, "Использует для отображения сообщений в чате между спикерами и администраторами", "Websocket API")
Rel(Communications, CommunicationsDB, "Сохраняет и получает данные о сообщениях в чате между спикерами и администраторами")

Rel(Communications, OrganizationZone, "Использует для получения данных о спикерах, заявках и админах", "gRPC")

Rel(Content, Conferences, "Использует для получения контента о конференциях", "gRPC")

Rel(Conferences, Lectures, "Использует для получения информации о лекциях", "gRPC")

Rel(Conferences, OrganizationZone, "Использует для получения информации о спикерах и слушателях", "gRPC")

Rel(OrganizationZone, TicketingSystem, "Использует для продажи билетов и получения списка билетов", "REST API, HTTPS")

SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| WebApp | Веб приложение для конференций HelloConf |
| Lectures System | Система, отвечающая за информацию о лекциях |
| Lectures Storage | Хранение информации о лекциях |
| Schedule System | Система, отвечающая за расписание |
| Schedule Storage | Хранение расписания |
| Organization Zone System | Система, отвечающая за орагнизационную зону |
| Organization Zone Storage | Хранение данных орагнизационной зоны |
| Content System | Использует для отображения информации о конференциях |
| Content Storage | Хранение информации о контенте на сайте |
| Conferences System | Система, отвечающая за информацию о конференциях и ее участниках |
| Conferences Storage | Хранение информации о конференциях и ее участниках |
| Communications System | Система, отвечающая за коммуникации между спикерами и администраторами |
| Communications Storage | Хранение коммуникации между спикерами и администраторами |