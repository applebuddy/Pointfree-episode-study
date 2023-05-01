# Pointfree-episode-study
point free episode study

## SwiftUI and State Management: Part 1

- https://www.pointfree.co/collections/swiftui/state-management/ep65-swiftui-and-state-management-part-1
- notice : BindableObject -> ObservableObject로 rename 되었음

- SwiftUI는 Xcode 11 beta+, Swift 5.1+ 에서 사용 가능합니다.

### Accounting App

- 숫자를 설정하고, 소수인지(소수라면 소수임을 알려주고 저장 or 삭제 가능) N번째 소수가 무엇인지 판별 가능
- 저장한 가장 좋아하는 소수 리스트 확인 기능

- PlaygroundPage.current.liveView에 SwiftUI View를 래핑한 UIHostingController를 넣으면 playground에서 View를 볼 수 있음
- NavigationView 블럭 안에 NavigationLink를 사용하면 버튼 역할 + 누르면 특정 뷰로 네비게이션 전환 할 수 있음

~~~swift
func foo() -> Int {
  fatalError()
}

// MARK: ContentView

struct ContentView: View {
  var body: some View {
    // body 계산 프로퍼티는 View를 준수하는 객체를 반환한다.
   	NavigationView {
      List {
        // "Counter demo"를 눌렀을때 EmptyView()가 보여집니다.
        NavigationLink(destination: EmptyView()) {
       	 Text("Counter demo")
      	}
        NavigationLink(destination: EmptyView()) {
       	 Text("Favorite primes")
      	}
      }
    }
  }
}
~~~

- Button 은 안에 Label View를 설정할 수 있고, 클릭 시의 이벤트 클로져를 설정 가능

~~~swift
struct CounterView: View {
  var body: some View {
    VStack {
      HStack {
        Button(action: {
          // tap action에 대한 callback event 설정 가능
        }) {
          // 버튼에 대한 label view가 설정
          Text("-")
        }
        Text("0")
        Button(action: {}) {
          Text("+")
        }
      } //: HStack
      Button(action: {}) {
        Text("Is this prime?")
      }
      Button(action: {}) {
        Text("What is the 0th price?")
      }
    }
  }
}
~~~

- ObservableObject

~~~swift
import SwiftUI
import Combine

class AppState: ObservableObject {
    var count = 0 {
        willSet {
            // count 값이 설정 되면, publisher에서 이벤트를 방출할 수 있음
            objectWillChange.send()
        }
    }
    
    init() {}
    
    /// public var objectWillChange: ObservableObjectPublisher
    /// - ObservableObject protocol은 Failure타입이 Never인 ObservableObjectPublisher타입의 objectWillChange publisher가 정의되어 있습니다.
    var objectWillChange: ObservableObjectPublisher = .init()
}
~~~

