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
<br>Compensate for weak type information to clarify a parameter’s role.
<br>Especially when a parameter type is NSObject, Any, AnyObject, or a fundamental type such Int or String, type information and context at the point of use may not fully convey intent. In this example, the declaration may be clear, but the use site is vague.
```swift
❌

func add(_ observer: NSObject, for keyPath: String)

grid.add(self, for: graphics) // vague
```
To restore clarity, precede each weakly typed parameter with a noun describing its role:
```swift
✅

func addObserver(_ observer: NSObject, forKeyPath path: String)
grid.addObserver(self, forKeyPath: graphics) // clear
```
## Strive for Fluent Usage
Prefer method and function names that make use sites form grammatical English phrases.
```swift
x.insert(y, at: z)         // “x, insert y at z”
x.subViews(havingColor: y) // “x's subviews having color y”
x.capitalizingNouns()      // “x, capitalizing nouns”
```
```swift
x.insert(y, position: z)
x.subViews(color: y)
x.nounCapitalize()
```
It is acceptable for fluency to degrade after the first argument or two when those arguments are not central to the call’s meaning:
```swift
AudioUnit.instantiate(
  with: description, 
  options: [.inProcess], completionHandler: stopProgressBar)
```
Begin names of factory methods with “make”, e.g. x.makeIterator().

The first argument to initializer and factory methods calls should not form a phrase starting with the base name, e.g. x.makeWidget(cogCount: 47)

For example, the first arguments to these calls do not read as part of the same phrase as the base name:
```swift
let foreground = Color(red: 32, green: 64, blue: 128)
let newPart = factory.makeWidget(gears: 42, spindles: 14)
let ref = Link(target: destination)
```
In the following, the API author has tried to create grammatical continuity with the first argument.
```swift
let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
let ref = Link(to: destination)
```
In practice, this guideline along with those for argument labels means the first argument will have a label unless the call is performing a value preserving type conversion.

```swift
let rgbForeground = RGBColor(cmykForeground)
```
Name functions and methods according to their side-effects

Those without side-effects should read as noun phrases, e.g. `x.distance(to: y)`, `i.successor()`.

Those with side-effects should read as imperative verb phrases, e.g., `print(x)`, `x.sort()`, `x.append(y)`.

Name Mutating/nonmutating method pairs consistently. A mutating method will often have a nonmutating variant with similar semantics, but that returns a new value rather than updating an instance in-place.

When the operation is naturally described by a verb, use the verb’s imperative for the mutating method and apply the “ed” or “ing” suffix to name its nonmutating counterpart.

|Mutating|Nonmutating|
|--|--|
|x.sort()|z = x.sorted()|
|x.append(y)|z = x.appending(y)|

Prefer to name the nonmutating variant using the verb’s past participle (usually appending “ed”):
```swift
/// Reverses `self` in-place.
mutating func reverse()

/// Returns a reversed copy of `self`.
func reversed() -> Self
...
x.reverse()
let y = x.reversed()
```
When adding “ed” is not grammatical because the verb has a direct object, name the nonmutating variant using the verb’s present participle, by appending “ing.”
```swift
/// Strips all the newlines from `self`
mutating func stripNewlines()

/// Returns a copy of `self` with all the newlines stripped.
func strippingNewlines() -> String
...
s.stripNewlines()
let oneLine = t.strippingNewlines()
```
When the operation is naturally described by a noun, use the noun for the nonmutating method and apply the “form” prefix to name its mutating counterpart.

|Nonmutating|Mutating|
|--|--|
|x = y.union(z)|y.formUnion(z)|
|j = c.successor(i)|c.formSuccessor(&i)|

Uses of Boolean methods and properties should read as assertions about the receiver when the use is nonmutating, e.g. x.isEmpty, line1.intersects(line2).

Protocols that describe what something is should read as nouns (e.g. Collection).

Protocols that describe a capability should be named using the suffixes able, ible, or ing (e.g. Equatable, ProgressReporting).

The names of other types, properties, variables, and constants should read as nouns.

## Use Terminology Well
> Term of Art : noun - a word or phrase that has a precise, specialized meaning within a particular field or profession.



Avoid obscure terms if a more common word conveys meaning just as well. Don’t say “epidermis” if “skin” will serve your purpose. Terms of art are an essential communication tool, but should only be used to capture crucial meaning that would otherwise be lost.

Stick to the established meaning if you do use a term of art.

The only reason to use a technical term rather than a more common word is that it precisely expresses something that would otherwise be ambiguous or unclear. Therefore, an API should use the term strictly in accordance with its accepted meaning.

Don’t surprise an expert: anyone already familiar with the term will be surprised and probably angered if we appear to have invented a new meaning for it.

Don’t confuse a beginner: anyone trying to learn the term is likely to do a web search and find its traditional meaning.

Avoid abbreviations. Abbreviations, especially non-standard ones, are effectively terms-of-art, because understanding depends on correctly translating them into their non-abbreviated forms.

> The intended meaning for any abbreviation you use should be easily found by a web search.


Embrace precedent. Don’t optimize terms for the total beginner at the expense of conformance to existing culture.

It is better to name a contiguous data structure Array than to use a simplified term such as List, even though a beginner might grasp the meaning of List more easily. Arrays are fundamental in modern computing, so every programmer knows—or will soon learn—what an array is. Use a term that most programmers are familiar with, and their web searches and questions will be rewarded.

Within a particular programming domain, such as mathematics, a widely precedented term such as sin(x) is preferable to an explanatory phrase such as verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x). Note that in this case, precedent outweighs the guideline to avoid abbreviations: although the complete word is sine, “sin(x)” has been in common use among programmers for decades, and among mathematicians for centuries.