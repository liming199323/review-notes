# 特质(trait)
特质 (Traits) 用于在类 (Class)之间共享程序接口 (Interface)和字段 (Fields)。  
它们类似于Java 8的接口。 类和对象 (Objects)可以扩展特质，但是特质不能被实例化，因此特质没有参数。
# 定义特质
最简单的定义方法就是关键字trart+标识符
特质用作泛型类型和定义抽象方法非常有用
```scala
trait HairColor  // 定义一个简单的特质

trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}
```
# 使用特质
使用extends关键字来实现特质
使用override关键字来实现特质里面的任何抽象成员
```scala
trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}

// IntIterator将参数to作为hasNext方法比较的上限，并扩展了Iterator的类型为Int，因此next方法必须返回Int类型的值
class IntIterator(to: Int) extends Iterator[Int] {
  private var current = 0
  override def hasNext: Boolean = current < to
  override def next(): Int =  {
    if (hasNext) {
      val t = current
      current += 1
      t
    } else 0
  }
}

val iterator = new IntIterator(10)
iterator.next()  // returns 0
iterator.next()  // returns 1
```
# 子类型
凡是需要特质的地方，都可以由该特质的子类型来替换。
```scala
trait Pet {
  val name: String
}

class Cat(val name: String) extends Pet
class Dog(val name: String) extends Pet

val dog = new Dog("Harry")
val cat = new Cat("Sally")
// 此处集合中需要放入Pet类型的对象，因为Cat和Dog实现了Pet特质，因此对象dog和cat可以放入该集合
val animals = ArrayBuffer.empty[Pet]
animals.append(dog)
animals.append(cat)
animals.foreach(pet => println(pet.name))  // Prints Harry Sally
```
# sealed关键字
特质（trait）和类（class）可以用sealed标记为密封的，这意味着其所有子类都必须与之定义在相同文件中，从而保证所有子类型都是已知的。  
用于模式匹配，当抽象类加了sealed关键字之后，scala在编译的时候会进行检查，如果有漏掉的case类型，会警告提示。
```scala
// Couch和Chair必须和Furniture定义在相同的文件中
sealed abstract class Furniture
case class Couch() extends Furniture
case class Chair() extends Furniture

// 由于Man加了sealed关键字，模式匹配时，scala会检测是否遗漏匹配的类型，编译时会警告提醒
sealed abstract class Man
case object American extends Man
case object Chinese extends Man
case object Russia extends Man

def from(m: Man) = m match {
  case American ⇒ println("American")
  case Chinese ⇒ println("Chinese")
}
// Warning:(61, 29) match may not be exhaustive.It would fail on the following input: Russia
//    def from(m: Man) = m match {

// 如果确定不会有Russia类型的对象传入，可以使用下面的方法去掉警告提示
def from(m: Man) = (m : @unchecked) match {
  case American ⇒ println("American")
  case Chinese ⇒ println("Chinese")
}
```