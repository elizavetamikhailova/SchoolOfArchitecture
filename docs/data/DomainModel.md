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
    Conference "many" *-- "many" Lecture
    Conference "many" *-- "many" Place 

    class Speaker {
        id: Int
        user: User
        occupation: String
        lectures: Lecture []
    }
    Speaker "1" *-- "many" Lecture
    Speaker "1" o-- "1" User

    class Listener {
        id: Int
        user: User
        tickets: Ticket []
    }
    Listener "1" o-- "1" User
    Listener "1" *-- "many" User

    class Place {
        id: Int
        address: String
        description: String
    }

    class Feedback {
        user: User
        conference: Conference
    }
    Feedback "many" o-- "1" User
    Feedback "many" o-- "1" Conference

}


namespace Lectures {

    class Lecture {
        id: Int
        title: String
        url: YoutubeUrl
    }
    Lecture "1" *-- "1" YoutubeUrl

    class Feedback {
        user: User
        lecture: Lecture
    }
    Feedback "many" o-- "1" User
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
    UpcomingConferenceInfo "1" o-- "1" Conference


    class InformationAboutPastConferences {
        conferences: Conference []
    }
    InformationAboutPastConferences "1" o-- "many" Conference

}

