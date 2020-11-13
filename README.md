```
import Foundation

final class Ref<T> {
    var value: T
    init(_ value: T) {
        self.value = value
    }
}

@propertyWrapper
struct COWproperty<T> {
    var ref: Ref<T>
    var wrappedValue: T {
        get {
            return ref.value
        }
        set {
            if(!isKnownUniquelyReferenced(&ref)) {
                ref = Ref(newValue)
                return
            }
            ref.value = newValue
        }
    }
    
    init(wrappedValue: T) {
        self.ref = Ref(wrappedValue)
    }
}

struct Test {
    @COWproperty
    var cowBehaviourValue: Int = 5
}
```
