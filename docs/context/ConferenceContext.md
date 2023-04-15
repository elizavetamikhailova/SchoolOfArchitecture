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

Rel(Speaker, WebApp, "Использует")
Rel(Listener, WebApp, "Использует")
Rel(WebApp, Regisrations, "Использует для подачи заявки спикером")
Rel(WebApp, Regisrations, "Использует для регистрации на конференцию слушателя")
Rel(WebApp, Content, "Использует для получения контента о конференциях")
Rel(Content, Conferences, "Использует для получения контента о конференциях")

Rel(ibs, es, "Sends e-mails", "SMTP")
