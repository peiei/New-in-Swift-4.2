# New-in-Swift-4.2
New in Swift 4.2 中文
1. Bool.toggle
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

2.
