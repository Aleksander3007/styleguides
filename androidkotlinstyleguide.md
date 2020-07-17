# Coding Conventions

В основе лежит:
https://kotlinlang.org/docs/reference/coding-conventions.html

<br/>

### Именование

- Имена классов начинаются с буквы верхнего регистра и используется camel case.  
Пример: ***TxDeclarationProcessor***.  
Неверно: ***TXDeclarationProcessor*** является неверным написанием, т.к. противоречит camel case стилю.

- Мапы именуются как ***valuesByKey***.  
Пример: ***sinsById***

- Листы именуются во множественном числе как ***values***.  
Пример: ***sins***

- Объекты типа ***Flow*** именуются с постфиксом ***Observable***.  
Пример: ***fieldObservable***

- Объекты типа ***StateFlow*** именуются с постфиксом ***Mutable***.  
Пример: ***fieldMutable***

- Объекты типа ***Boolean*** именуются с префиксом ***is***.  
Пример: ***isClosed***

- UI-Объекты именуются с постфиком типа.  
Пример: ***addressTextView***, ***buyButton***.

- Методы для настройки UI-компонентов именуются с префиксом ***setup***.  
Пример: ***setupSinsRecyclerView()***

- Базовый класс именуется с префиксом ***Base<...>***.

- Сокращений не используются. Исключение составляет переменные видимость которых не превышает 3 строк.  
Пример: использовать ***record*** вместо ***rec***, ***context*** вместо ***c***.

<br/>

### Форматирование

- Все аргументы конструктора пишутся на новой строчке:
```
class MyClass(
    private val field1: Type1,
    private val field2: Type2
)
```
Исключение: если всего один аргумент и он используется только в конструкторе, то пишется в одну строчку, при условии что умещается.
```
class Descendant(field1: Type1): Parent(field2) {
    val field3 = field1.toField3()
}
```
<br/>

- При наследовании первыми в ребенке указываются новые поля:
```
class Parent(val field1: Type1)
class Descendant(val field2: Type2, field1: Type1) : Parent(field1)
```
<br/>

- Если у поля одна аннотации и она умещается вместе с полем в одну строчку, то пишется в одну строчку. Во всех остальных случаях каждая аннотация пишется на новых строках. 
Порядок аннотаций: самые верхние - самые значимые.
```
class Foo {
    @Annotation private val field: Type

    @ImportantAnnotation1
    @Annotation2 
    private val field: Type
}
```
<br/>

- Если тело функции не умещается в одну строчку, то оборачивается в фигурные скобки.
```
 fun foo(num: Int): Type {
    return veryVeryLongClassObject.getVeryVeryLongNamedField() * num + 
        pow(10, -num);
   }
```
<br/>

- Булевы выражения в коротком теле функции оборачивается в круглых скобках.
```
fun Int.isEven() = (this % 2 == 0)
```
<br/>

- Последовательные множественные вызовы пишутся на новых строчках.
```
MyBuilder()
    .setField1(1)
    .setField2(2)
    .build()

sinsStore.flow
    .map {}
    .onEach {}
    .launch()
```
Но допускается в одну строчку, если вызов из не более двух методов.
```
MyBuilder().setField1(1).build()
sinsStore.flow.map{ }.first{ }
```
<br/>

- Если if..else не умещается в одну строку, то пишется полная версия.
```
val x = if (something) {
    veryVeryLongFunction1()
} else {
    veryVeryLongFunction2()
}
```
<br/>

- Если тело if..else не умещается в одну строку, то return пишется в самих блоках.
```
if (something) {
    return veryVeryLongFunction1()
} else {
    veryVeryLongFunction2()
    return veryVeryLongFunction3()
}
```
<br/>

- Не использовать it, если он имеет большую видимость.
```
store.getSomething()?.let { something ->
    // some code
    // some code
    return@let something + 1
}
```
<br/>

- Отдавать приоритет единообразию форматирования кода, даже если это противоречит одному из пунктов.
Пример:
```
view.background = when(position) {
    FIRST_POSITION -> 
        context.getDrawable(R.drawable.d1)
    SECOND_POSITION -> 
        context.getDrawable(R.drawable.d2)
    LAST_POSITION ->
        context.getDrawable(R.drawable.long_drawable_last_position)
    else -> 
        context.getDrawable(R.drawable.else_drawable)
}
```
Перенос в кейсах 1,2, else выполнен только для единообразия.

<br/>

### Организация

- Файл с Koltin extensions должен содержать расширения только для одного класса и иметь следующее имя: ***ClassNameExtensions***.
Пример: ***SinExtensions.kt***

- Отдавать приоритет следующему порядку расположения методов, классов и полей: от public к private.

- BindView указывается в конце класса.

<br/>

### Best practices

- Минимизировать видимость методов, полей, классов и т.п., но в рамках самой задумки, а не выставлять минимально возможное.

- Минимизировать scope переменной: объявление, присвоение и использование должно быть как можно ближе друг к другу.

- Чем больше видимость переменной, метода, класса и т.п., тем более специфичнее должно быть и его имя.

- Использовать preconditions вместо комментариев и устных договоренностей.

- Предпочитать немутабельность мутабельности.

- Избегать использование ***lateinit***, хороший аналог - ***Delegates.notNull()***.

- Предпочитать typealias вместо интерфейсов с методом.

- Всегда обрабатывать конкретные исключения, а не сразу Exception.
```
try {
    // some code
} catch(npex: NullPointerException) { 
    // some code
} catch(ioex: IOException) {
    // some code
}
```
<br/>

- TODO должен содержать: что нужно сделать и когда это можно сделать.
```
// TODO: Раскомментировать строку когда в service1 добавят getSomething()
```
<br/>

- Не писать длинных (по количество строк кода в них) методов и классов.

- Весь лишний код должен быть удален. Не закоментирован.

- Избегать дублирования.

- Избегать concurrency.

- Предпочитать concurrency collections вместо synchronized. 

- Избегать магических чисел.
