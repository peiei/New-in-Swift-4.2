# new in Swift 4.2
# 1.Bool.toggle
<div>常用于嵌套类型的Bool属性的的取反，<code>A.B.C.D.bool = !A.B.C.D.bool</code> 可以改写成 <code>A.B.C.D.bool.toggle()</code>
代码：
<pre><code>
struct Layer {
    var isHidden = false
}
struct View {
    var layer = Layer()
}
var view = View()
// Before:
view.layer.isHidden = !view.layer.isHidden
view.layer.isHidden
// Now:
view.layer.isHidden.toggle()
</code></pre></div>

# 2. Sequence and Collection algorithms
# 序列和集合的算法
## 2.1 allSatisfy
类似于<code>contains(where:)</code>,判断包含的元素是否满足某个条件，<code>A(集合或序列).allSatisfy{条件}</code>,当A的所有元素都满足条件则返回<code>true</code>,否则返回<code>false</code><br/>
代码：
<pre><code>
let digits = 0...9
let areAllSmallerThanTen = digits.allSatisfy { $0 < 10 }
areAllSmallerThanTen
let areAllEven = digits.allSatisfy { $0 % 2 == 0 }
areAllEven
</code></pre>
</div>
## 2.2 last(where:), lastIndex(where:), and lastIndex(of:) 
序列（Sequence）添加新的方法last(where:),返回满足条件的所有元素的最后一个元素（可选类型，所以可能返回nil）<br/>
代码：
<pre><code>
let digits = 0...9
let lastEvenDigit = digits.last { $0 % 2 == 0 }
lastEvenDigit
</code></pre>
<div>集合添加新的方法 lastIndex(where:)表示返回满足条件的所有元素的最后一个元素的坐标，（可选类型，所以可能返回nil）<br/> lastIndex(of:)表示指定的元素出现在数列中的最后一个的坐标<br/>(两者区别：一个是条件，一个是特定元素)<br/>
代码：
<pre><code>
let students = ["Kofi", "Abena", "Peter", "Kweku", "Akosua"]
if let i = students.lastIndex(where: { $0.hasPrefix("A") }) {
		print("\(students[i]) starts with 'A'!")
 }
 ///Prints "Akosua starts with 'A'!"
  </code>
  <code>
var students = ["Ben", "Ivy", "Jordell", "Ben", "Maxime"]
if let i = students.lastIndex(of: "Ben") {
         students[i] = "Benjamin"
     }
print(students)
 /// Prints "["Ben", "Ivy", "Jordell", "Benjamin", "Max"]"
 </code></pre>
 
## 2.3 removeAll(where:)
移除符合条件的所有元素, 与filter方法相反，一个是符合条件移除，一个是符合条件筛选出来。  
代码：

	var numbers = Array(0...10)
	numbers.removeAll(where: { $0 % 2 != 0 })
# 3. Enumerating enum cases
## 3.1 CaseIterable协议
该协议提供了一个<code>allCases</code>计算属性，例如枚举A遵循该协议，则A.allCases返回一个包含所有A的成员值的集合，该属性自动生成的前提是枚举A的成员值没有关联值（关联值可能无限大的原因），假如需要关联的话，可以自己实现协议,代码如下：
<pre><code>
enum CompassDirection: CaseIterable {
	case north, south, east, west
 }
print("There are \(CompassDirection.allCases.count) directions.")
// Prints "There are 4 directions."
let caseList = CompassDirection.allCases
                               .map({ "\($0)" })
                               .joined(separator: ", ")
 // caseList == "north, south, east, west"
</code></pre>
# 4. Random numbers
## 4.1 Generating random numbers
所有的number类型都有random(in:) 方法，该方法返回一个给定范围的内的随机数  
代码：  

	Int.random(in: 1...1000)
	UInt8.random(in: .min ... .max)
	Double.random(in: 0..<1)
Bool.random()也同样会返回一个随机的bool值

	func coinToss(count tossCount: Int) -> (heads: Int, tails: Int) {
    var tally = (heads: 0, tails: 0)
    for _ in 0..<tossCount {
        let isHeads = Bool.random()
        if isHeads {
            tally.heads += 1
        } else {
            tally.tails += 1
        }
    }
    return tally
	}

	let (heads, tails) = coinToss(count: 100)
	print("100 coin tosses — heads: \(heads), tails: \(tails)")
## 4.2 Random collection elements
集合类的通过<code>randomElement()</code>方法返回其中的一个随机元素，当集合为空时返回nil，可选类型

	let emotions = "😀😂😊😍🤪😎😩😭😡"
	let randomEmotion = emotions.randomElement()!
	
	let names = ["Zoey", "Chloe", "Amani", "Amaia"]
	let randomName = names.randomElement()!
使用<code>shuffled()</code>返回一个打散后（集合重新随机排序）集合

	let numbers = 0...9
	let shuffledNumbers = numbers.shuffled()
	// shuffledNumbers == [1, 7, 6, 2, 8, 9, 4, 3, 5, 0]
同样的，也为遵循了 MutableCollection 和 RandomAccessCollection协议的类型提供了一个shuffle()方法，例如数组，该方法可以打乱原有的排序，并随机重新排序。

   	var names = ["Alejandro", "Camila", "Diego", "Luciana", "Luis", "Sofía"]
	names.shuffle()
	print(names)
	// ["Luis", "Camila", "Diego", "Luciana", "Sofía", "Alejandro"]
## 4.3 Custom random number generators
除了使用系统默认提供的随机生成算法<code>SystemRandomNumberGenerator()</code>外，我们还可以自定义新的随机生成器A，A必须要遵循RandomNumberGenerator协议。

	struct MyRandomNumberGenerator: RandomNumberGenerator {
    var base = SystemRandomNumberGenerator()
    mutating func next() -> UInt64 {
        return base.next()
    }
	}

	var customRNG = MyRandomNumberGenerator()
	Int.random(in: 0...100, using: &customRNG)
 
## 4.4 Extending your own types
可以给自定义的类型添加一个生成随机数的方法
<pre><code>
enum Suit: String, CaseIterable {
    case diamonds = "♦"
    case clubs = "♣"
    case hearts = "♥"
    case spades = "♠"
    static func random<T: RandomNumberGenerator>(using generator: inout T) -> Suit {
        // Using CaseIterable for the implementation
        return allCases.randomElement(using: &generator)!
    }
    static func random() -> Suit {
        var rng = SystemRandomNumberGenerator()
        return Suit.random(using: &rng)
    }
}
let randomSuit = Suit.random()
randomSuit.rawValue
</code></pre>
# 5. Hashable redesigng
更方便的实现哈希，例如你想要用一个集合存储一些自定义类型的点，但这类型没有遵循Hashable时是存不了的，所以要给自定义类型实现Hashable，代码如下
<pre><code>
///  A point in an x-y coordinate system.
struct GridPoint {
	var x: Int
	var y: Int
}  
extension GridPoint: Hashable {
    static func == (lhs: GridPoint, rhs: GridPoint) -> Bool {
        return lhs.x == rhs.x && lhs.y == rhs.y
    }
    func hash(into hasher: inout Hasher) {
        hasher.combine(x)
        hasher.combine(y)
    }
}  
var tappedPoints: Set = [GridPoint(x: 2, y: 3), GridPoint(x: 4, y: 1)]
let nextTap = GridPoint(x: 0, y: 1)
     if tappedPoints.contains(nextTap) {
         print("Already tapped at (\(nextTap.x), \(nextTap.y)).")
     } else {
         tappedPoints.insert(nextTap)
         print("New tap detected at (\(nextTap.x), \(nextTap.y)).")
    }
     // Prints "New tap detected at (0, 1).")
</code></pre>

# 6. Conditional conformance enhancements
## 6.1 Dynamic casts



 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 




















