# Scala 簡介

> Class, Traits, Objects, 和 Packages

## Classes
在 Scala 中的 class 與 Java 中的 class 非常類似. 他是包含 fields 和 methods 的樣板. 就像 Java 一樣，
classes 可以使用 new construct 被實例化，一個 class 可以有多個 instances (或物件)

在 Scala 中存在一個特殊類型的 class, 稱作 `case class`, 你將會在接下來的課程中學到此類型

Classes 在 scala 中無法有靜態成員(members), 你可以使用 `Object` 達到跟 Java 中的靜態變數的類似的功能

## Traits
Traits 與 Java 的 interface 很類似，但他可以有具體成員，也就是有實作的 method 或 field 的定義

## Objects
Object 在 Scala 中就跟 classes 一樣，但是對於每一個 object 的定義來說，只會有一個 instance。
使用 new 來建立一個新的建立一個新的 instance 對於 object 來說是不可能的，
也就是說你可以直接使用 object 名字存取他的成員(methods or fields)。如果你定義了一個類別 MyClass 在 foo.bar 這個 package 中, 
你可以使用 `import foo.bar.MyClass` 來個別 import 這個類別，而不是全部都 import 進來。

## Packages
將語句 `package foo.bar` 加入到檔案的最上方，會使檔案中的程式碼成為 `foo.bar` 這個 package 的一部分。
然後你可以 `import foo.bar._` 使 `foo.bar` 這個 package 中的所有內容在您的程式碼中都可以使用。
package 的內容可以分散在許多文件中。

在 Scala 中，所有的東西都可以被 `import`, 不只是 class 名稱。例如你有一個 object `baz` 在 package `foo.bar` 裡面，
則 `import foo.bar.baz._` 將會 import 此 object 的所有成員

## Hello, World! in Scala

在 Scala 中有兩種方式來定義程式輸出 "Hello, World"

```scala
object HelloWorld extends App {
  println("Hello, World")
}
```
或
```scala
object HelloWorld {
  def main(args: Array[String]) {
    println("Hello, World!!")
  }
}
```

在 Scala 中，main 或一個進入點是定義在一個 object 中。一個可以被執行的 object 可以藉由繼承 App 這個 type 
或加入一個 main(args: Array[String]) 這個 method。

## Source Files, Classfiles 和 JVM

Scala 的程式碼是存在附檔名為 **.scala** 的文字檔中。一般來說，Scala 碼農會針對一個類別新增一個程式碼檔案或一個檔案對應一個類別結構。
但事實上，Scala 是允許多個**類別**和**物件**都定義在同一個檔案中的。

+ 程式碼檔案名稱和定義在裡面的類別名稱是可以自由選擇的（可以不相同），但是，還是建議使用相同名稱比較好。
+ **Package** 的層次結構必須和資料夾的層次結構相同：在 **Package** foo.bar 裡面定義一個類別 **C**，其程式碼的檔案必須儲存在資料夾
  **foo/bar/C.scala**。雖然 Scala 並沒有真強制這種風格，但某些工具例如 Eclipse 的 Scala IDE 如果沒有這樣做可能會有些問題
   
Scala 編譯器會將 **.scala** 檔案編譯成為 **.class** 檔，就如同 Java 的編譯器依樣。Classfiles 是包含了給 JVM 的機器碼讀取
的 binary 檔案，為了執行 Scala 程式，JVM必須知道 classfile 儲存的資料夾位置，而這個參數就稱作 **"classpath"**

如果你是使用 eclipse 或 sbt 來編譯或執行 scala 程式碼，你就不需手動作以上的任何設定，這些工具會幫助你完成設定。

