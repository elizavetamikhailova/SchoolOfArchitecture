```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(Speaker, "Speaker")
Person(Listener, "Listener")

System(WebApp, "Web App", "Веб приложение для конференций HelloConf")

System(Lectures, "Lectures System", "Система, отвечающая за хранение и расписание лекций, а так же за информацию о них")

System(Regisrations, "Regisrations System", "Система, отвечающая за регистрацию спикеров и участников на конференцию")

System(Content, "Content System", "Система, отвечающая за формирование контента на сайте")

System(Conferences, "Conferences System", "Система, отвечающая за хранение информации о конференциях и ее участниках")

System_Ext(TicketingSystem, "Ticketing System", "Внешняя система по продаже билетов")

System_Ext(Youtube, "Youtube", "Стриминг видео")

Rel(Speaker, WebApp, "Использует")
Rel(Listener, WebApp, "Использует")
Rel(WebApp, Regisrations, "Использует для подачи заявки спикером", "REST API")
Rel(WebApp, Regisrations, "Использует для регистрации на конференцию слушателя", "REST API")
Rel(WebApp, Content, "Использует для отображения информации о конференциях", "REST API")
Rel(WebApp, Lectures, "Использует для того, чтобы спикер мог выбрать свободный слот в расписании", "Web Socket API")
Rel(WebApp, Conferences, "Использует для того, чтобы пользователь мог оставить свой отзыв о конференции", "REST API")
Rel(WebApp, Lectures, "Использует для того, чтобы пользователь мог оставить свой отзыв о конкретной лекции", "REST API")
Rel(WebApp, Youtube, "Использует для отображения трансляций/записей видео", "HTTPS")
Rel(Content, Conferences, "Использует для получения контента о конференциях", "gRPC")
Rel(Conferences, Lectures, "Использует для получения информации о лекциях и расписании", "gRPC")
Rel(Conferences, Regisrations, "Использует для получения информации о спикерах и слушателях", "gRPC")
Rel(Regisrations, TicketingSystem, "Использует для продажи билетов и получения списка билетов", "REST API, HTTPS")
