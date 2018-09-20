# New-in-Swift-4.2
New in Swift 4.2 中文
1. <div>Bool.toggle
常用于嵌套类型的Bool属性的的取反，<code>A.B.C.D.bool = !A.B.C.D.bool</code> 可以改写成 <code>A.B.C.D.bool.toggle()</code>
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

2. <div>Sequence and Collection algorithms<br/>序列和集合的算法</div>
<div>2.1 allSatisfy<br/>
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
<div>2.2 last(where:), lastIndex(where:), and lastIndex(of:) <br/>序列（Sequence）添加新的方法last(where:),返回满足条件的所有元素的最后一个元素（可选类型，所以可能返回nil）<br/>
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
 
 





















