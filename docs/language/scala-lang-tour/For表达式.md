# For表达式
Scala提供一个轻量级的标记方式用来表示序列推导。推导使用形式为for (enumerators) yield e 的表达式。  
enumerators指一组以分号分隔的枚举器。一个enumerator要么是一个产生新变量的生成器，要么是一个过滤器。  
for表达式在枚举器产生的每一次绑定中都会计算e值，并在循环结束后返回这些值组成的序列。  
注意for表达式并不局限于使用列表。任何数据类型只要支持withFilter，map，和flatMap操作(不同数据类型可能支持不同的操作)都可以用来做序列推导。
```scala
case class User(name: String, age: Int)

val userBase = List(User("Travis", 28),
  User("Kelly", 33),
  User("Jennifer", 44),
  User("Dennis", 23))

// for循环后面使用的yield语句实际上会创建一个List[String]。
// user <- userBase 是生成器，if (user.age >=20 && user.age < 30) 是过滤器用来过滤掉那些年龄不是20多岁的人。
val twentySomethings = for (user <- userBase if (user.age >=20 && user.age < 30)) yield user.name

twentySomethings.foreach(name => println(name))  // prints Travis Dennis
```
```scala
def foo(n: Int, v: Int) =
   for (i <- 0 until n;
        j <- i until n if i + j == v)
   yield (i, j)

foo(10, 10) foreach {
  case (i, j) =>
    println(s"($i, $j) ")  // prints (1, 9) (2, 8) (3, 7) (4, 6) (5, 5)
}

// 也可以省略yield，此时返回Unit，该示例和上面的输出结果完全相同
def foo(n: Int, v: Int) =
   for (i <- 0 until n;
        j <- i until n if i + j == v)
   println(s"($i, $j)")

foo(10, 10)
```