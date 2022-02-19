# First Term Revision

### 1. Introduction to Java

- James Gosling
- 1995 with initial name Oak, Sun Microsystems
- Oracle 
- WORA - write once, run anywhere

Current Version:
- Java SE 17 (LTS) 	September 14, 2021 

Java is a whole computing platform:
- Programming language
- Java can be considered both a compiled and an "interpreted" language:
  - source code __=> compiled =>__ binary byte-code 
  - byte-code __=> JVM =>__ machine code
  
> Modern JVMs take bytecode and compile it into native code when first needed. "JIT" in this context stands for "just in time." It acts as an interpreter from the outside, but really behind the scenes it is compiling into machine code.

  - JDK (Java Development Kit) = JRE (an interpreter/loader (Java)) + Compiler (javac) + Archiver (jar)  + documentation generator (Javadoc)
    - JRE - provides the class libraries and other resources that a specific Java program needs to run.

Editions:
- Java Platform Standard Edition __Java SE__
- Java Platform Enterprise Edition __Java EE__
- Java Platform Micro Edition __Java ME__
        vs. Android SDK
- Java Card

--- 
#### Run-time vs. Compile-time Error

__NB:__
> Java is statically-typed, so it expects its variables to be declared before they can be assigned values.
 
1. Compile-time error
    The program need not satisfy any invariants:
    - Syntax errors
    - Typechecking errors
    - Compiler Crashes (Rarely)

2. Run-time
    Run-time invariants are rarely enforced by the compiler alone; it needs help from the programmer.
    - Division by zero
    - null
    - Running out of memory

Compile-time:
```java
string my_value = Console.ReadLine();
int i = my_value;
```

Run-time:
```java
string my_value = Console.ReadLine();
int i = int.Parse(my_value);
```

---

#### Data Types

1. Primitive

- __boolean__
> The size depends on the virtual machine.
- __byte__ 
  * size: 1B (8 bits)
- __char__ 
  * Unicode
  * size: 2B (16 bits)
  * Code point: 0 - 65535
  * \u0000 - Unicode Escape Representation
- __short__
  * size: 2B (16 bits)
- __int__
  * size: 4B (32 bits)
- __long__
  * size: 8B (64 bits)
- __float__
  * size: 4B (32 bits)
  * IEEE 754
  * 6 to 7 decimal digits precision
- __double__
  * size: 8B (64 bits)
  * IEEE 754
  * 15 to 16 decimal digits precision 

Literals:
```java
int i = 1;           // int by default
long l = 1L;         // L or l
double d = 0.1;      // d or D is optional
double d2 = 1e-1;    // same, in scientific => 1 * 10^-1
float f = 0.1;       // will not compile, why? => 0.1f :)
char c = 'A';
char c2 = '\u0041';  // again, 'A'
```

```java
// The number 26, in decimal
int decVal = 26;
// The number 26, in hexadecimal
int hexVal = 0x1a;
// The number 26, in binary
int binVal = 0b11010;
// The number 26, in octal
int octVal = 032;
```

```java
int thousand = 1_000;
int million  = 1_000_000;
long magic   = 0xCAFE_BABE;
int one      = 0b1;
int mask     = 0b1010_1010_1010;
```

* Default Values ?! Yes, but not:
>Компилаторът не присвоява стойности по подразбиране на неинициализираните локални променливи!

Default values when it comes to fields:
```java
byte - 0
short - 0
int - 0
long - 0L
float - 0.0f
double - 0.0d
char - '\u0000'
String (or any Object) - null
boolean - false
```

2. Reference

```java
new => HEAP memory
```
![](Resources/Images/obj.png)

String
  - __Immutable!!!__
  - String Pool - a storage area in Java heap where string literals stores 
     - Performance and memory saving mechanism  
  - .intern() - HEAP to POOL move
```
При опит за добавяне на низ в string pool-a, ако там вече  
съществува низ със същото съдържание, не се създава нов обект, а   
се връща референция към съществуващия (→ пести се памет)
```
Don't forget:
- equals() to compare two Strings
- == to compare their references

```java
// new in Java 15: text blocks for multi-line string literals
String html = """
                       <html>
                           <body>
                               <p>Hello world!</p>
                           </body>
                       </html>
              """;
```

* Mutable Strings ??
  - StringBuilder
  - StringBuffer

| Class         | Mutable | Performant        | Thread-Safe |
| ------------- | ------- | ----------------- | ----------- |
| String        | no      | slow, if modified | yes         |
| StringBuilder | yes     | fast              | no          |
| StringBuffer  | yes     | slower            | yes         |

![](res/../Resources/Images/str.png)

Arrays - again Reference types

```java
int[] a;     // preferred syntax
int a[];     // also valid
// explicit initialization
// can be done only during declaration
int[] a = {1, 2, 3, 4};
int[] b = new int[7];
b.length;
```

* Декларация – не се заделя памет за елементите на масива
* Инициализация – заделя се памет за елементите на масива
* Масивите от примитивни типове се инициализират автоматично със стойността по подразбиране на съответния тип


"Вградености":
```java
System.arraycopy(src, srcPos, dest, destPos, length); // копиране
Arrays.equals(arr1, arr2); // проверка за еднаквост
Arrays.fill(arr, value); // запълване с дадена стойност
Arrays.toString(arr); // конвертиране в низ
Arrays.sort(arr); // сортиране
Arrays.sort(a, Collections.reverseOrder()); // сортиране в обратен ред

И други
```

3. Wrappers - part of __java.util__

> All wrappers are Immutable

 Wrappers Class
     : Will convert primitive data types into objects.

Usage:
- Collection framework store only the objects (reference types) and not the primitive types.
- The objects are necessary if we wish to modify the arguments passed into the method
- Utilities and constants implemented in the wrapper 

* Autoboxing/unboxing in Wrapper Class

Autoboxing/unboxing is used to convert primitive data types (implicitly) into corresponding objects, and vice-versa.

4. Immutable
    : Once an object is created, we cannot change its content

- An immutable class is good for caching purposes because you don’t have to worry about the value changes.
- Thread-safe :)

---
How to create an immutable class:
    1. Declare the class as final so it can’t be extended.
    2. Make all fields private so that direct access is not allowed.
    3. Don’t provide setter methods for variables.
    4. Make all mutable fields final so that its value can be assigned only once.
    5. Initialize all the fields via a constructor performing deep copy.
    6. Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.

---

#### Methods (или както му викат в C++ функции)

![](Resources/Images/funcs.png)

#### Math Library

![](/Resources/Images/math.png)

#### switch on Steroids (Java 15)

```java
char ch = 'a';
int t;
// before
switch (ch) {
    case 'a'      : t = 1; break;
    case 'b', 'c' : t = 12; break;
    default       : t = 27;
}
// since Java 15
switch (ch) {
    case 'a'      -> t = 1;
    case 'b', 'c' -> t = 12;
    default       -> t = 27;
}
```

```java
int z = switch (ch) {
    case 'a'      -> 1;
    case 'b', 'c' -> 12;
    default       -> 27;
};

// Тоя е грозен, ама за пълнота
int r = switch (ch) {
    case 'a' : yield 1;
    case 'b' : yield 12;
    default  : yield 27;
};
```

#### Pattern matching and null in switch (JDK 17)

Ref:
https://mkyong.com/java/what-is-new-in-java-17/#jep-406-pattern-matching-for-switch-preview

```java
    static String formatterJava17(Object o) {
        return switch (o) {
            case Integer i -> String.format("int %d", i);
            case Long l    -> String.format("long %d", l);
            case Double d  -> String.format("double %f", d);
            case String s  -> String.format("String %s", s);
            default        -> o.toString();
        };
    }
```


### 2. OOP in Java pt.1

OOP 
  : programming paradigm based on the concept of
"objects", which can contain data in form of fields and
procedure code, known as methods. 

Object
- data + methods
- състояние на обект = данни
- поведение на обект = методи
- Living in the Heap memory

Експлицитно създаване:
Чрез __new__
```java
String message = new String("Operation completed."); // explicit
```
Чрез фактори методи, Array literal, други...
```java
Integer studentsCount = Integer.valueOf(60); // implicit
String[] stringArr = {"Some", "example"}; // implicit
```

Class
- описва състоянието и поведението на обектите

Може да съдържа:
- Съдържа конструктори (поне 1 задължителен)
- Методи

Class Methods
- име и списък с параметри (сигнатура)
- тяло
- може да има __странични ефекти__ - да прави нещо друго,
от това, за което е предназначен


Varargs (variable arguments)
- редица от нула или повече аргументи от един и същи тип
- Syntax: 
```java
int...
```
- Свежда се и се третира като масив

Static член променливи и методи
- част от класа, а не от инстанцията (обекта)
- могат да се достъпват само с името на класа
- едно единствено копие живее в паметта
- механизъм за комиуникация между всички инстанции на класа
- пазят се отново в heap
```java
System.out.println(Math.PI);        // 3.141592653589793
System.out.println(Math.pow(3, 2)); // 9.0
```

Ключовата дума ```this```
- референция към конкретния обект
- неявно се подава като параметър на всеки конструктор и
нестатичен метод на класа

Упортреба:
```java
public class Human {
    private String name;
    public Human() {
        // Извикване на overload-натия конструктор със String параметър
        this("Unknown");
    }
    public Human(String name) {
        // Достъпване на член-променлива, скрита от едноименен параметър
        this.name = name;
    }
    public void whoAmI() {
        System.out.println("My name is " + name);
    }
}
```

Пакети
- групи от семантично свързани класове
- създават йерархия на кода
- съответстват на директорно дърво на файловата система
- именуване ```com.google.mail``` "обърнат домейн нейм"
- има дефолтен пакет, ама не е добра практика да създаваме класове там
```java
package bg.sofia.uni.fmi.mjt.example;
public class Example {
    // class Example is in package bg.sofia.uni.fmi.mjt.example
}
```
- всеки клас има достъп (имплицитно) до:
    - класовете от собствения си пакет
    - java.lang 
- достъп (експлицитно) чрез import:
```java
package bg.sofia.uni.fmi.mjt.example;

import java.util.Arrays;
import java.util.Scanner;
```
- Прието е import-ите да се подреждат в сортиран лексикографски ред по ```<package>.<class>```

Модификатори за достъп
- public - всички го виждат
- без модификатор - package-private - вижда се в рамките на пакета, в който е
- private - вижда се само в рамките на класа
- protected - вижда се в рамките на класа и наследниците му

Състояние на обект
  : Множеството текущи стойности на член-данните на даден обект се наричат негово състояние

Енкапсулация
- Само вътрешните методи на даден обект имат достъп
до неговото състояние
- Скрит за външния свят
- Постига се чрез модификаторите за достъп

Наследяване
- Позволява преизползване и разширяване на състояние и поведение на вече съществуващи класове
- В Java се реализира с ключовата дума ```extends```
- Класът-наследник получава достъп до всички public и protected член-променливи и методи на родителския клас
- Java не поддържа множествено наследяване

__Method Overriding__
Класът-наследник може да предостави собствена дефиниция на (т.е. да предефинира) методи на родителския клас (method overriding)

- Сигнатурата на метода в класа-родител и класа-наследник трябва да е идентична
- Модификаторът за достъп на метод в класа–наследник трябва да съвпада или да разширява модификатора за достъп на метода в родителския клас (но не може да го свива/ограничава)
- Типът на връщаната стойност трябва да е съвместим с този на override-вания метод. Съвместим означава идентичен или наследник (за референтните типове) - return __type covariance__
- Методът в класа-наследник е препоръчително да се анотира с опционалната анотация @Override. Така компилаторът ще ни помага да не нарушаваме горните правила

```super```
- Референция към прекия родител на обекта
- Употребява се за:
  - достъпване на член-променливи на родителя
  - извикване от конструктор в текущия клас на конструктор в родителския клас
  - извикване на произволен метод на родителския клас

Йерархията в Java
- Дърво от класове с корен Object
В Object:
```java
boolean equals(Object obj)
int hashCode()
String toString()
Object clone()
// и други
  ```

```instaceof```
- Използва се за type checking на референтните типове - дали даден обект е инстанция на даден клас

```java
Student ivan = new Student("Ivan", 61786);
Human petar = new Human("Petar");
System.out.println(ivan instanceof Student);   // true
System.out.println(ivan instanceof Human);     // true
 ```

Ключовата дума __final__
- в декларация на променлива → прави я константа
- в декларация на метод → методът не може да се override-ва
- в декларация на клас → класът не може да се наследява

Полиморфизъм
  : даден обект да се държи като инстанция на друг клас или като имплементация на друг интерфейс

- ООП - наследниците на даден клас споделят поведение от родителския клас, но могат да дефинират и собствено поведение 
- Всички Java обекти са полиморфични, понеже всеки обект наследява java.lang.Object класа

Runtime полиморфизъм
- постига се чрез method __overriding__

Compile-time полиморфизъм
- постига се чрез method __overloading__

Полиморфният код е по-кратък, по-четим


Abstract
- Дефинират се с модификатора abstract
- Могат да имат методи без имплементация, които се декларират с модификатора abstract
- Не са напълно дефинирани (оставят на наследниците си да ги конкретизират/допълнят) 
- Не могат да се създават обекти от тях

Interface
- Съвкупност от декларации на методи без имплементация
- Описват формално поведение, без да го имплементират
- Може да съдържат __static final__ член-променливи == константи
- Методите на интерфейсите са public и abstract по подразбиране
- Модификаторите public и abstract (заедно или поотделно) могат да бъдат указани и експлицитно
  - Дали да бъдат експлицитно указани, е въпрос на стил - но е добре да сме консистентни
- Ако даден клас декларира, че имплементира интерфейс, той трябва или

  - Да даде дефиниции на всичките му методи, или
  - Да бъде деклариран като абстрактен/интерфейс
 
Място на Default методите (Java 8)
- Default-ен метод в интерфейс е метод, който

   - има имплементация
   - има модификатора default в декларацията си

- Имплементиращите класове имплицитно ползват default-ната имплементация на методите, но могат и да я предефинират

- Backward Compatibility

```java
public interface OptimisticLockable {
    default boolean isLocked() {
        return false;
    }
}
public interface PessimisticLockable {
    default boolean isLocked() {
        return true;
    }
}
public class Door implements OptimisticLockable, PessimisticLockable {
    // We will get a compile-time error, if we don't override the isLocked() method here:
    // - "Door inherits unrelated defaults for isLocked() from types
    //        OptimisticLockable and PessimisticOldLockable"
    @Override
    public boolean isLocked() {
        return OptimisticLockable.super.isLocked();
    }
}
```

Най-важно обобщение за Interface:
  - Публични, абстрактни методи без имплементация
  - static final член-променливи == константи
  - default и static методи с имплементация (от Java 8)
  - private методи (от Java 9)

Маркерен интерфейс
  : не съдържа нито един метод

Функционален интерфейс
  : съдържа точно един публичен абстрактен метод (SAM)

Абстракция в Java. Постига се чрез
    - Interface
    - Abstract


Initializer - да инициализираме статични променливи (и не само, но предимно тях):

```java
public class InitializeMe {
    private int a;
    private static int b;
    static {
        // static initializer block
        b = 100;
    }
    {
        // initializer block
        a = 5;
    }
}
```

Enum
- Всеки enum неявно наследява абстрактния клас java.lang.Enum
- Тялото на enum класа може да съдържа член-променливи и методи
- Ако има конструктор, той трябва да е package-private или private
- Той автоматично създава константите в дефиницията на enum-a. Не може да се извиква явно

Records (Java 16)

- спестява boilerplate code
- описват компактно т.нар value objects - класове, които имат само състояние: състоят се от "полетата, само полетата и нищо освен полетата"
- immutable
- getters (имената им съвпадат с имената на съответните полета)
- дефолтно имплементирани ```toString(), equals() и hashCode()```
- Каноничен конструктор 
- всички records имплицитно наследяват абстрактния клас java.lang.Record, самите те са final и не могат да са abstract (immutable)
- 

```java
public record Point(int x, int y) {}
// [...]
Point p = new Point(-1, 2);
System.out.println(p.x() + ", " + p.y());
```

```java
public record FXOrder(int units,
                      CurrencyPair pair,
                      Side side,
                      double price,
                      LocalDateTime sentAt) {}  
```

- изключение са т.нар. компактни конструктори: в тялото на каноничния конструктор, добавяме само валидация и/или нормализация на параметрите, а останалата част от кода (присвояванията на полетата) се осигурява от компилатора

```java
    public FXOrder {
        if (units < 1) {
            throw new IllegalArgumentException("FXOrder units must be positive");
        }
        if (price <= 0.0) {
            throw new IllegalArgumentException("FXOrder price must be positive");
        }
    }
```

- компактните конструктори нямат параметри, пропускат се дори празните скоби ()
- кодът в компактния конструктор се добавя като допълнителен код в началото на дефиницията на каноничния конструктор (а не е отделен конструктор)

```Sealed```

- ```sealed``` типовете ни позволяват да ограничим класовете или интерфейсите, които могат да ги наследяват или имплементират

```java
// sealed класовете декларират, кои класове могат да ги наследяват
public abstract sealed class Shape
    permits Circle, Rectangle {...}
 ```

- sealed класът и неговите permitted класове трябва да са в един пакет

```java
public sealed class Plant permits Herb, Shrub, Climber {}
```

- sealed интерфейсите могат да специфицират, кои интерфейси могат да ги наследяват и кои класове могат да ги имплементират

Exceptions

1. Хвърляне
```java
public Object pop() {
    if (size == 0) {
        throw new EmptyStackException();
    }
    // [...]
}
```

2. Хващане
```java
try {
    // код, който може да хвърли изключение
} catch (Exception e) {
    // обработваме изключението ("exception handler")
    // може да има повече от един catch блок
} finally {
    // при нужда, някакви заключителни операции
    // finally блокът е optional, но ако го има,
    // се изпълнява задължително щом влезем в try-a
    // без значение хвърлен ли е Exception, или не
}
```

3. Catch block chain

```java


try { 
    // [...]
} catch (MostSpecificException mse) {
    // [...]
} catch (MoreGeneralException mge) {
    // [...]
} ... {
    // [...]
} catch (LeastSpecificException lse) {
    // [...]
}
```

4. Multi catch block

```java
try {
    // [...]
} catch (IOException | SQLException e) {
    // [...]
}
```

5. Видове
- checked
  - IOException
  - FileNotFoundException
  - При друди ресурси 
- unchecked
    - RuntimeException
      - NullPointerException - любимият ми 
- errors (unchecked)
  пр.
  - IOError
  - StackOverflow (Stack)
  - OutOfMemory (Heap) 

![](Resources/hierarchyEx.png)

6. Обработка на checked
```java
public void writeList() throws IOException, FileNotFoundException {
    // [...]
}
```

или

```java
try {
    // [...]
} catch (IOException e) {
    // прехващаме изключение, обработваме го 
    // и хвърляме ново, към което го "закачаме"
    throw new SampleException("Oopss..", e); 
}
```

7. Finally

```java
try { 
    // тук може да се хвърлят изключения
    // или да има return/continue/break
} finally {
    // някакъв важен cleanup code -
    // ще се изпълни винаги*, независимо какво се случи в try блока
}
```

Принципи за качествен код:
Справяне с code duplication:
- Наследяване 
- Изнасяне на общата логика в методи

Именувания
- на променливи camelCase
- на методи camelCase
- на класове Capitalized
- на константи ALL_CAPS_UNDER

Отделяме интерфейсите от декларацията
- използваме абстракция

Където е възможно полиморфизъм вместо ```instanceof```

ОО Дизайн
- Един клас => да прави Едно нещо
- Един метод => едно нещо
- Името да казва какво да е това нещо
- Concise and clear methods

Без magic numbers
- заменяме ги със ```static final``` = константи

Exceptions
- Да не се suppress-вай exceptions 

Никога не оставяй празен catch, или catch само с e.printStackTrace()

- Когато е възможно, ползвай ```Exception(String message, Throwable cause)``` конструктор

```java
// Good
try {
    // do something with the file system
} catch (FileNotFoundException e) {
    // catalog file is missing -
    // this is OK, first access should create an empty catalog
    createEmptyShoppingCatalog();
} catch (IOException e) {
    // catalog file is there but cannot be opened or read
    // calling method knows how to handle this
    throw new ShoppingException("Cannot load shopping catalog", e);
}
```

Разделяй I/O логиката от бизнес логиката

Collections

```java
java.util
```

Iterable and Iterator
```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove();
}
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

![](Resources/Images/collection.png)

и отделена от Collection

![](Resources/Images/map.png)

Immutable vs. Unmodifiable

- Колекции, които не поддържат модифициращи операции (като add, remove и clear) се наричат unmodifiable. Колекциите, които не са unmodifiable, са modifiable.

- Колекции, които в допълнение гарантират, че не са възможни никакви промени в съдържанието им, се наричат immutable. Колекциите, които не са immutable, са mutable.


---
Resources or other useful links:
https://mkyong.com/ - Тоя е трепач
https://www.codewithharry.com/blogpost/java-cheatsheet
https://introcs.cs.princeton.edu/java/11cheatsheet/
https://docs.oracle.com/javase/tutorial/java/index.html


