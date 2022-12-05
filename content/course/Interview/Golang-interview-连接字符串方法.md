---

title: Golang 面试题：连接字符串有几种方法
date: '2022-11-29'
type: book
weight: 101
commentable: true
editable: true
tags:
  - golang
  - 腾讯真题
---

> 面试真题来源：腾讯会议

字符串连接是编程中最基本的操作之一，每种语言都有不同的方法来实现，我们接下来研究下 go 能够有几种方式来连接字符串。

## +

加号（+）运算符可用于连接字符串。它通常是最常用的字符串连接方式，无需太多考虑。

```go
name := "John" + " " + "Doe" // John Doe
```

只要提供多个字符串作为参数，fmt 包中的 print 函数就会自动连接。它还可以在字符串之间添加空格。

```go
fmt.Println("It", "works!") // prints "It works!"
```

## +=

可以使用 += 运算符将字符串附加到另一个字符串上。和加号运算符一样，但是是一种稍微短一点的连接方式。它将右侧附加到操作它的字符串上。所以，它本质上是附加字符串。下面是使用 += 运算符追加字符串的示例。

```go
u := "This"
v := " is working."
u += v   // sets u to "This is working."
```

## strings.join()

strings 包中的 join 函数可用于连接字符串，如下所示：

```go
func Join(a []string, sep string) string
```

它需要两个参数，一个字符串数组和一个分隔符来连接他们。它将从中生成一个字符串，下面是一个示例：

```go
package main
 
import (
    "fmt"
    "strings"
)
 
func main() {
 
    s := []string{"This", "is", "a", "string."}
 
    v := strings.Join(s, " ")
     
    fmt.Println(v)  // This is a string.
}
```

## fmt.Sprintf()

fmt 包中的 Sprintf 方法可用于字符串连接。下面是个例子：

```go
    "fmt"
)
 
func main() {
 
    s1 := "abc"
    s2 := "xyz"
     
    v := fmt.Sprintf("%s%s", s1, s2)
     
    fmt.Println(v) // abcxyz
}
```

如你所看到的，字符串 format 允许我们以这种方式进行字符串连接。

## bytes.buffer

bytes 包包含一种类型的 buffer，即字节缓冲。我们可以使用 WriteString 方法写入字符串，然后将缓冲转换为字符串。下面是一个示例：

```go
package main
 
import (
	"bytes"
    "fmt"
)
 
func main() {
    var b bytes.Buffer
     
    b.WriteString("abc")
    b.WriteString("def") // append
     
    fmt.Println(b.String()) // abcdef
}
```

## strings builder

strings 包有一个 builder 类型，这是构建字符串的一种非常有效的方法。它在连接字符串时使用的内存要少得多，是一种更好的连接方式。函数 WriteString 允许我们以更快的方式连接字符串，下面是示例：

```go
package main
 
import (
    "fmt"
    "strings"
)
 
func main() {
    var sb strings.Builder
    sb.WriteString("First")
    sb.WriteString("Second")
    fmt.Println(sb.String())    // FirstSecond
}
```

使用同一个 builder，我们可以向字符串里面添加 rune 或 bytes。

## strings.Repeat

我们可以多次连接同一个字符串以形成另一个字符串。strings 包中的 repeat 方法以一种高效的方式完成了这项工作：

```go
package main
 
import (
    "fmt"
    "strings"
)
 
func main() {
    fmt.Println(strings.Repeat("abc", 3))  // abcabcabc
}
```

## 思维导图

![go-面试题-连接字符串有几种方法.png](https://cnymw.github.io/GolangStudy/docs/go-面试题-连接字符串方法/go-面试题-连接字符串有几种方法.png)