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

Person(customer, "Пользователь", "Спикер/Слушатель/Администратор")

System_Boundary(c, "HelloConf") {

    Container(WebApp, "Веб приложение для конференций HelloConf", "html, JavaScript, Angular", "Сайт")

    Container(Lectures, "Lectures System", "Система, отвечающая за информацию о лекциях", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(LecturesDB, "Lectures Storage", "Хранение информации о лекциях", "CassandraDB", $tags = "storage")

    Container(Schedule, "Schedule System", "Система, отвечающая за расписание", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(ScheduleDB, "Schedule Storage", "Хранение расписания", "MongoDB", $tags = "storage") "тут можно подумать, и например выбрать не CP, CA, хотя если докладчиков много, то наверное нужно CP, лучше в ADR добавить вариант CA и посмотреть почему он не подходит"

    Container(OrganizationZone, "Organization Zone System", "Система, отвечающая за орагнизационную зону", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(OrganizationZoneDb, "Organization Zone Storage", "Хранение данных орагнизационной зоны", "CassandraDB", $tags = "storage")

    Container(Content, "Content System", "Система, отвечающая за формирование контента на сайте", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(ContentDB, "Content Storage", "Хранение информации о контенте на сайте", "Redis", $tags = "storage")

    Container(Conferences, "Conferences System", "Система, отвечающая за информацию о конференциях и ее участниках", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(ConferencesDB, "Conferences Storage", "Хранение информации о конференциях и ее участниках", "CassandraDB", $tags = "storage")

    Container(Communications, "Communications System", "Система, отвечающая за коммуникации между спикерами и администраторами", "Java/Kotlin, Spring Boot", $tags = "microService")

    ContainerDb(CommunicationsDB, "Communications Storage", "Хранение коммуникации между спикерами и администраторами", "CassandraDB", $tags = "storage")


Container(offering_service, "Product Offering Service", "Java, Spring Boot", "Сервис управления продуктовым предложением", $tags = "microService")      
   ContainerDb(offering_db, "Product Catalog", "PostgreSQL", "Хранение продуктовых предложений", $tags = "storage")
   
   Container(ordering_service, "Product Ordering Service", "Golang, nginx", "Сервис управления заказом", $tags = "microService")      
   ContainerDb(ordering_db, "Order Inventory", "MySQL", "Хранение заказов", $tags = "storage")
    
   Container(message_bus, "Message Bus", "RabbitMQ", "Транспорт для бизнес-событий")
   Container(audit_service, "Audit Service", "C#/.NET", "Сервис аудита", $tags = "microService")      
   Container(audit_store, "Audit Store", "Event Store", "Хранение произошедших события для аудита", $tags = "storage")
}

System_Ext(TicketingSystem, "Ticketing System", "Внешняя система по продаже билетов")

System_Ext(Youtube, "Youtube", "Стриминг видео")

System_Ext(logistics_system, "msLogistix", "Система управления доставкой товаров.")  

Lay_R(offering_service, ordering_service)
Lay_R(offering_service, logistics_system)
Lay_D(offering_service, audit_service)

Rel(customer, WebApp, "Оформление заказа", "HTTPS")
Rel(app, offering_service, "Выбор продуктов для корзины(Продукт):корзина", "JSON, HTTPS")

Rel(offering_service, message_bus, "Отправка заказа(Корзина)", "AMPQ")
Rel(offering_service, offering_db, "Сохранение продуктового предложения(Продуктовая спецификация)", "JDBC, SQL")

Rel(ordering_service, message_bus, "Получение заказа: Корзина", "AMPQ")
Rel_U(audit_service, message_bus, "Получение события аудита(Событие)", "AMPQ")

Rel(ordering_service, ordering_db, "Сохранение заказа(Заказ)", "SQL")
Rel(audit_service, audit_store, "Сохранение события(Событие)")
Rel(ordering_service, logistics_system, "Доставка(Наряд на доставку):Трекинг", "JSON, HTTP")  

SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| *Название компонента* | *Описание назначения компонента* |