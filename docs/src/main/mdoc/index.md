## Installation
For latest snapshot use

```scala
libraryDependencies += "@ORG@" % "@NAME@" % "@SNAPSHOT_VERSION@"
```

And for the latest stable release use
```scala
libraryDependencies += "@ORG@" % "@NAME@" % "@RELEASE_VERSION@"
```

For ease of useability, this project is cross compiled against @CROSS_VERSIONS@ 

## ProcessTimer Usage
The ProcessTimer annotation can be used to log runtime information about a method
```scala mdoc
import com.bbrownsound.macros._

@ProcessTimer
def testMethod[String]: Double = {
  val x = 2.0 + 2.0
  Math.pow(x, x)
}

@ProcessTimer
def methodWithArguments(a: Double, b: Double) = {
  val c = Math.pow(a, b)
  c > a + b
}

@ProcessTimer
def longRunning(n: Int) = {
  for (i <- 0 until n) yield Math.pow(i, i)
}
```

## StringObfuscation Usage
The StringObfuscation annotation can be used to obfuscate sensitive values by overwriting the `toString` method
```scala mdoc
import com.bbrownsound.macros.ToStringObfuscate

@ToStringObfuscate("password", "pinCode")
case class TestPassword(name: String, username: String, password: String, pinCode: String)

@ToStringObfuscate("cardNumber")
case class TestCreditCard(cardNumber: String, cvv: Int, endDate: String)

@ToStringObfuscate("password")
case class UserPassword(username: String, password: String)

@ToStringObfuscate("password", "pinCode")
case class TestUser(username: String, password: String, pinCode: Long)

case class NestedExample(user: TestUser)
```

## Debug Usage
The Debug annotation can be used to println debug a given line of code without needing to litter printlns
```scala mdoc
import com.bbrownsound.macros.Debug
import com.bbrownsound.macros.ToStringObfuscate
import java.util.UUID

@Debug val pinCode = UUID.randomUUID().toString
//[com.bbrownsound.examples.ObfuscateSpec:19] pinCode - 2f7ab2f6-9795-433d-9744-46382754e101

@Debug val obfuscatedInstance = TestPassword(
  name = "name",
  username = "username",
  password = "password",
  pinCode = pinCode
)
//[com.bbrownsound.examples.ObfuscateSpec:21] obfuscatedInstance - TestPassword&#40;a3e79549-13bf-41c8-8984-4962f3038abe,738699be-4a96-442c-a2e2-b60b0e785f8f,************************************,************************************

def foo(x: Int): Int = x * x

@Debug
val t: Int = foo(36)
//[com.bbrownsound.examples.ObfuscateSpec:29] t - 1296
```
