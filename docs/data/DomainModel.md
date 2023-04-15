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
}

namespace Lectures {

    class Lecture {
        id: Int
        title: String
        url: YoutubeUrl
    }

    class Feedback {
        user: User
        lecture: Lecture
    }
    
    class Schedule {
        timeslots: Timeslot []
    }

    class Timeslot {
        id: String
        date: Date
        lecture: Lecture
    }

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
    
    class Ticket {
        id: Int
        date: Date
        ownerEmail: String
    }

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

    class InformationAboutPastConferences {
        conferences: Conference []
    }
}

Conference "many" *-- "many" Place 
Conferences.Feedback "many" o-- "1" Conference
Lecture "1" *-- "1" YoutubeUrl
Conferences.Conference "many" *-- "many" Lecture : ref
Conferences.Speaker "1" *-- "many" Lecture : ref
Lectures.Feedback "many" o-- "1" Lecture
Schedule "1" *-- "many" Timeslot
Timeslot "1" *-- "1" Lecture
Conferences.Speaker "1" o-- "1" User : ref
Conferences.Listener "1" o-- "1" User
Conferences.Feedback "many" o-- "1" User
Lectures.Feedback "many" o-- "1" User
Conferences.Listener "1" *-- "many" Ticket : ref
UpcomingConferenceInfo "1" o-- "1" Conference
InformationAboutPastConferences "1" o-- "many" Conference
