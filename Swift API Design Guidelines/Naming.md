# Naming

## Promote Clear Usage

* **Include all the words needed to avoid ambiguity**
<br>코드로 사용되는 모든 이름들은, 가독성을 높이기 위해 필요한 모든 단어를 사용해야 한다.
<br>collection에서 at position이라는 매개변수를 가지는 remove라는 메소드를 예로 들어보자.
    ```swift
    ✅

    extension List {
        public mutating func remove(at position: Index) -> Element
    }
    employees.remove(at: x)
    ```
    여기서 만약 at이라는 표현이 없다면, 코드를 읽는 사람은 'x'라는 인자와 같은 값을 삭제하는 메소드라고 이해할 수 있다.

    ```swift
    ❌

    employees.remove(x) // unclear: are we removing x?
    ```

* **Omit needless words**
<br>필요없는 단어는 과감히 삭제하자.
<br>사용되는 모든 단어는 사용되는 위치에서 핵심적이어야 한다.

    의도를 명확히 표현하거나 의미의 분명한 차이를 보이기위해 더 많은 단어가 필요할 수 있지만,
<br>이전에 나왔던, 중복되는 단어는 삭제해야 한다.
<br>특히, *그저 반복되기만하는* 타입정보 같은걸 삭제해야 한다.
    
    ```swift
    ❌

    public mutating func removeElement(_ member: Element) -> Element?

    allViews.removeElement(cancelButton)
    ```
    위의 removeElement에서 Element라는 단어는 호출시 핵심적인 느낌이 들지않는다. (당연)
<br>아래의 경우처럼 삭제하는게 더 명확한 표현이 될 수 있다.

    ```swift
    ✅

    public mutating func remove(_ member: Element) -> Element?

    allViews.remove(cancelButton) // clearer
    ```
    간혹, 모호성을 피하기 위해 타입정보를 반복하는 경우가 있지만,
<br>타입 보다는 매개변수의 *역할*을 설명하는 단어를 사용하는 것이 더 좋다.

<br>

* **Name variables, parameters, and associated types according to their roles**
<br>변수, 매개변수, 관련 타입들의 이름으로 해당 역할을 적어주는게 타입을 쓰는것 보다 낫다.
    ```swift
    ❌

    var string = "Hello"
    protocol ViewController {
        associatedtype ViewType : View
    }
    class ProductionLine {
        func restock(from widgetFactory: WidgetFactory)
    }
    ```
    string, ViewType, widgetFactory 이런식의 이름으로 지어주면, 명확한 표현을 나타내도록 최적화했다고 보기 어렵다.
    <br>greeting, ContentView, supplier 이런식으로 타입의 역할을 나타내는 단어로 적어주는게 훨씬 좋다.

    ```swift
    ✅

    var greeting = "Hello"
    protocol ViewController {
        associatedtype ContentView : View
    }
    class ProductionLine {
        func restock(from supplier: WidgetFactory)
    }
    ```
    연결된 타입이 프로토콜의 제약조건에 너무 엄격하게 바인딩 되어있는 경우에는,
    <br>프로토콜 이름에 'protocol'단어를 추가해서 해석이 충돌되는 것을 방지해주자. 
    ```swift
    protocol Sequence {
        associatedtype Iterator : IteratorProtocol
    }
    protocol IteratorProtocol { ... }
    ```
* **Compensate for weak type information**
<br>타입 정보가 부실한 경우, 역할 의미를 충분히 나타내도록 보충해야 한다.
<br>특히 파라미터로 NSObject, Any, AnyObject 또는 Int, String과 같은 기본타입, 
<br>타입정보나 사용시점에대한 정보에 대한 의도를 제대로 전달하지 못할 수도 있다.
<br>아래의 예시에서는, 선언할 때는 명확하지만 사용할 때는 모호하다.
    ```swift
    ❌

    func add(_ observer: NSObject, for keyPath: String)
    grid.add(self, for: graphics) // vague
    ```
    더욱 명확하게 표현하려면, 부족한 매개변수설명에 해당 역할표현을 보충시켜야 한다.
    <br>add -> addObserver, for -> forKeyPath
    ```swift
    ✅

    func addObserver(_ observer: NSObject, forKeyPath path: String)
    grid.addObserver(self, forKeyPath: graphics) // clear
    ```

<br>
<br>

## Strive for Fluent Usage
* **Prefer method and function names that make use sites form grammatical English phrases**
<br>문법적인 영어구문을 이용하여 메소드나 함수이름을 명명하여, 호출할 때 함수 이해도를 높이자.
    ```swift
    ✅

    x.insert(y, at: z)         // “x, insert y at z”
    x.subViews(havingColor: y) // “x's subviews having color y”
    x.capitalizingNouns()      // “x, capitalizing nouns”
    ```
    ```swift
    ❌

    x.insert(y, position: z)
    x.subViews(color: y)
    x.nounCapitalize()
    ```
    전달인자의 이름에 호출하는 의도가 들어가지 않는경우, 표현력이 부족해질 수 있다.
    ```swift
    AudioUnit.instantiate(
        with: description, 
        options: [.inProcess],
        completionHandler: stopProgressBar )
    ```

* **Begin names of factory methods with “make”**
<br>만들어내는 메소드 앞에는 'make'를 넣어주자.
<br>ex> x.makeIterator().

* **initializer and factory methods calls**
<br>생성자나 만들어내는 메소드의 첫번째 인자의이름 앞에는 생성자나 메소드이름이 들어가면 안된다.
<br>ex> x.makeWidget(cogCount: 47)

    이러한 예시로, 호출할 때 생성자나 메소드이름을 첫번째 인자에 연결시키는 경우가 있다.
<br>아래의 경우가 영문법적으로 연결시키는 잘못된 예다.
    ```swift
    ❌

    let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
    let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
    let ref = Link(to: destination)
    ```
    havingRGBValuesRed -> red / andBlue -> blue / havingGearCount -> gear / 
    <br>andSpindleCount -> spindles / to -> target으로 변경해줘야 한다.
    ```swift
    ✅

    let foreground = Color(red: 32, green: 64, blue: 128)
    let newPart = factory.makeWidget(gears: 42, spindles: 14)
    let ref = Link(target: destination)
    ```
    실제로 이 가이드라인은 [argument labels](https://swift.org/documentation/api-design-guidelines/#argument-labels)(함수 호출시 [value값이 타입을 바꾸는 경우](https://swift.org/documentation/api-design-guidelines/#type-conversion)일 때만 인자앞에 표기가 가능하다)를 따른다.
    <br>

    ```swift
    let rgbForeground = RGBColor(cmykForeground)
    ```
* **Name functions and methods according to their side-effects**
<br>함수와 메소드의 이름을 지을 때는, 부과효과에 대해 생각해봐야 한다.

    부과효과가 없는 경우에는 명사형으로 읽혀야 한다. 
<br>ex> `x.distance(to: y)`, `i.successor()`

    부과효과가 있는 경우에는 명령형 동사로 읽혀야 한다. 
<br>ex> `print(x)`, `x.sort()`, `x.append(y)`

    변환시키는/변환시키지않는 메서드 한 쌍은 일관되게 이름지어야 한다.
<br>변환시키는 메서드는 보통 변환시지지 않는, 변종과 유사한 의미를 가지지만,
<br>변종의 경우, 인스턴스를 제자리에서 업데이트 시키기보다는, 새로운 값을 리턴한다.

    * 함수의 동작이 **동사표현에 의해 자연스럽게 설명**될 때,
    <br>변환시키는 메서드는 명령형 동사를 사용하고,
    <br>변종의 이름뒤에는 접미사로 "ed"나 "ing"를 붙인다.

        |Mutating|Nonmutating|
        |--|--|
        |x.sort()|z = x.sorted()|
        |x.append(y)|z = x.appending(y)|
        |||

        * 변종의 이름은 동사의 과거분사형으로 쓰는것을 선호한다. (보통 끝에 "ed"를 붙인다)
            ```swift
            /// Reverses `self` in-place.
            mutating func reverse()

            /// Returns a reversed copy of `self`.
            func reversed() -> Self
            ...
            x.reverse()
            let y = x.reversed()
            ```
        * 동사가 진행적인 뜻을 담아 "ed"를 붙이는 것이 문법적으로 틀릴 땐, 
        <br>진행의 의미를 담고있는 과거분사형인 "ing"를 붙인다.
            ```swift
            /// Strips all the newlines from `self`
            mutating func stripNewlines()

            /// Returns a copy of `self` with all the newlines stripped.
            func strippingNewlines() -> String
            ...
            s.stripNewlines()
            let oneLine = t.strippingNewlines()
            ```
    * 함수의 동작이 **명사표현에 의해 자연스럽게 설명**될 때,
    <br>변종은 명사를 사용하고,
    <br>변환시키는 메서드는 명사에 접두어"from"을 붙여 사용한다.

        |Nonmutating|Mutating|
        |--|--|
        |x = y.union(z)|y.formUnion(z)|
        |j = c.successor(i)|c.formSuccessor(&i)|
        |||

* 불리언 메서드와 프로퍼티가 변환시키지 않는형태로 사용될 때는, 보는 이에게 주장의 느낌이들도록 해야 한다.
<br>ex> `x.isEmpty`, `line1.intersects(line2)`.

* 무언가를 설명하는 프로토콜은 명사로 읽어야 한다.
<br>ex> `Collection`

* 기능을 설명하는 프로토콜은 'able', 'ible', 'ing'와 같은 접미사를 사용하여 이름지어야 합니다.
<br>ex> `Equatable`, `ProgressReporting`

* 이외의 다른 타입, 프로퍼티, 변수, 상수들은 명사형으로 이름지어야 한다.

<br>

## Use Terminology Well
> 예술적 표현으로, 명사란 특정분야나 직업내에서 정확하고 전문화된 의미를 갖는 단어나 구를 뜻한다.

* **Avoid obscure terms**
<br>애매한 용어는 피하고, 되도록 더 자주쓰이고 의미를 잘 전달하는 이름으로 써야한다.
<br>목적이 "skin(피부)"이라면, "epidermis(표피)"라는 단어를 사용하면 안된다.
<br>예술적인 표현은 필수적인 의사소통 수단이지만,
<br>꼭 이렇게만 써야하는 이유가 있는것이 아니라면 피해야한다.

* 예술적 표현을 쓰고싶다면, 확실한 의미로써 이름을 붙여야한다.

    일반적인 표현보다 기술적인 용어를쓰는 가장큰 이유는, 모호함을 방지하기 위함이다.
    <br>그러므로 API는 일반적으로 인정하는 의미에 따라 엄격하게 사용해야한다.

    * **Don’t surprise an expert**
    <br>우리가 특정 용어에 합의되지않은 새로운 의미를 넣게된다면, 기존의미에 익숙한 사람은 불쾌할 것이다.

    * **Don’t confuse a beginner**
    <br>처음 용어를 접하는 사람은 우리의 의도와 다르게 그 용어를 기존의 뜻으로 잘못 해석할 수 있다.

* **Avoid abbreviations**
<br>줄임표현은 피하자. 특히 비표준약어의 경우, 원래 형태가 뭔지 생각하는 과정에서 글쓴이와 읽는이의 사고방향이 다를 수 있기때문에, 예술적 표현을 쓸 때와같은 부작용이 일어날 수 있다.

    > 사용하려는 줄임표현의 일반적인 의미는, 웹 검색을 통해 쉽게 찾을 수 있다.


* **Embrace precedent**
<br>선례를 받아들여라. 새로 배우기 시작하는 사람들을 위해 굳이 잘 쓰고있는 용어를 억지로 최적화시키지 마라.

    "List"와 같이 심플한 표현이 새로 시작하는 사람들 입장에서 더 쉽게 의미를 파악할 수 있을 지라도, 자료구조적으로 더 적합한 "Array"와 같이 이름을 짓는게 더 좋다.
    <br>"Array"는 현대 컴퓨팅의 기본으로, 모든 프로그래머가 알고있거나, 알고있는 개념이다.
    <br>대부분의 프로그래머가 익숙한 용어를 사용하면 웹 검색 및 질문에 대한 답변이 수월해질 것이다.

    수학과같은 프로그래밍적인 특정한 영역내에서 sin(x)와같은 널리 쓰이고있는 용어는 PositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)와같은 설명구절보다 바람직하다.
    <br>위의 경우에서, 선례가 줄임표현을 피하기위한 지침보다 더 크다는 점에 주목해야한다.
    <br>완전한 단어는 "sine"이지만, "sin(x)"는 수십년동안 프로그래머들 사이에서, 수세기동안 수학자들 사이에서 공통적으로 사용되어 왔다.