
```plantuml
@startuml
namespace A {

    class B {
        id: Int
        c: C
    }
    
}

namespace D {

    class C {
        id: Int
        title: String
    }

     A.B ..> C : ref
}


