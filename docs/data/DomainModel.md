# Модель предметной области

```plantuml
@startuml
namespace Conferences {

    class Conference {
        id: Int
        date: Date
        place: Place
        lectures: Lecture []
    }
   
    Conference "many" *-- "many" Place 

    class Speaker {
        id: Int
        user: User
        occupation: String
        lectures: Lecture []
    }

    class Listener {
        id: Int
        user: User
        tickets: Ticket []
    }

    class Place {
        id: Int
        address: String
        description: String
    }

    class Feedback {
        user: User
        conference: Conference
    }
    Feedback "many" o-- "1" Conference

}

namespace Lectures {

    class Lecture {
        id: Int
        title: String
        url: YoutubeUrl
    }
    Lecture "1" *-- "1" YoutubeUrl
    Conferences.Conference "many" *-- "many" Lecture : ref
    Conferences.Speaker "1" *-- "many" Lecture : ref

    class Feedback {
        user: User
        lecture: Lecture
    }
    Feedback "many" o-- "1" Lecture
    
    class Schedule {
        timeslots: Timeslot []
    }
    Schedule "1" *-- "many" Timeslot

    class Timeslot {
        id: String
        date: Date
        lecture: Lecture
    }
    Timeslot "1" *-- "1" Lecture

    class YoutubeUrl {
        url: String
    }

}

namespace Registration {

    class User {
        id: Int
        firstName: String
        lastName: String
        email: String
    }

    Conferences.Speaker "1" o-- "1" User : ref
    Conferences.Listener "1" o-- "1" User
    Conferences.Feedback "many" o-- "1" User
    Lectures.Feedback "many" o-- "1" User
    
    class Ticket {
        id: Int
        date: Date
        ownerEmail: String
    }
    Conferences.Listener "1" *-- "many" Ticket : ref

    class SpeakerRequest {
        id: Int
        user: User
        lectureTheme: String
        aboutSpeaker: String
    }
}

namespace Content {

    class UpcomingConferenceInfo {
        conference: Conference
    }
    UpcomingConferenceInfo "1" o-- "1" Conference


    class InformationAboutPastConferences {
        conferences: Conference []
    }
    InformationAboutPastConferences "1" o-- "many" Conference

}