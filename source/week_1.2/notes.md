# 24.01.2021 Pazar Notları

> Konular: Primitive Types, Reference Types, Expressions&Statements, If-Else Statement, Loops

### Variable; **var**, **let**, **const**

![Variable](/source/week_1.2/img/declaration.jpg)

### **var**

Kısaca _var_, _let_ ve _const_ ile değişken tanımlamalarında ve değer atamalarında ne gibi farklar var bakalım.

ES6'nın çıkmasından önce _var_ declarations hüküm sürüyordu. _var_ ile tanımlanan değişkenlerde birtakım sorunlar ortaya çıkıyor. _var_ ile tanımlanan değişkenlerin tekrar tanımlanabilmesi bazı durumlarda kodumuzda çok fazla bug'a ve karışıklığa yol açabiliyor. Bu sebeple değişkenleri tanımlamak için yeni yollar bulundu; _let_ ve _const_.

```js
var greeter = "hey hi";
var times = 4;

if (times > 3) {
  var greeter = "say Hello instead";
}

console.log(greeter); // "say Hello instead"
```

### **let**

_let_; _var_ ile değişken tanımlamalarındaki ortaya çıkan sorunu çözüyor. _let_, block-scoped değişkenler tanımlar. Böylece değişkenleri hangi blokta tanımlarsak sadece o blokta yaşarlar. Böylece if-else statement bloğunda tanımlanan bir değişken globale taşmaz.

```js
let greeting = "say Hi";
let times = 4;

if (times > 3) {
  let hello = "say Hello instead";
  console.log(hello); // "say Hello instead"
}
console.log(hello); // hello is not defined
```

let ile tanımlanan bir değişkenin değeri değiştirilebilir fakat tekrar tanımlanamaz.

```js
let car = "nissan";
car = "honda";
console.log(car); // honda

let computer = "asus";
let computer = "hp"; //Uncaught SyntaxError: Identifier 'greeting' has already been declared
```

### **const**

_const_ ile tanımlanan değişkenler adı üstünde _constant_ yani _sabit_ değişkenlerdir. Değişken bir kez tanımlandığında ve bir değer atandığında, o değişkenin değeri değiştirilemez. Genellikle, değiştirilmesini istemediğimiz değişkenleri const ile oluştururuz. Fakat const ile bir obje tanımlarsak, objeyi değiştiremesek de objenin içeriğini değiştirebiliriz.

```js
const car = "nissan";
car = "honda"; // Uncaught TypeError: Assignment to constant variable.

const computer = {
  brand: "Asus",
  price: "5.000",
};
computer.brand = "hp"; // hata alınmaz
```

Eğer objenin içeriğinin değiştirilmesini istemiyorsak örnekteki gibi objeyi dondurabiliriz.

```js
Object.freeze(computer); //Artık objenin içeriğini değiştiremeyiz.
```

> **Ek bilgi** => JavaScript'te her kelimeyi değişken olarak tanımlayamayız. Bazı kelimeler dilin kendi yapısında kullanıldığı için dil tarafından **reserved** olmuştur. Değişken tanımlarken kullanmamanız gereken **reserved** kelimeler bu linkte: https://www.w3schools.com/js/js_reserved.asp

### Temel Farklar

> **var** ile tanımlanan bir değişkeni redeclare edebiliriz ama **let** ve **const** ile tanımlanan bir değişkeni redeclare edemeyiz.

> **var** ile tanımlanan değişkenler globally-scoped ya da local-scoped iken **let** ve **const** ile tanımlanan değişkenler block-scoped'tur.

## Primitive Types

Primitive data tipi obje olmayan dataları temsil ediyor. JavaScript'te 7 tana primitive data tipi vardır.

- number
- string
- boolean
- undefined
- null
- symbol
- bigint

```js
let number = 20;
let isOkay = true;
let text = "Hello";
```

Primitive tipte oluşturulan değişkenler için memory'de tutulan şey, değişkenlerin değerleridir.

```js
let name = "Mehmet";
let displayName = name;
name = "Ahmet";
console.log(name); //Ahmet
console.log(displayName); //Mehmet
```

Yukarıdaki örnekte _displayName_'e, _name_'i atadığımızda _name_'in **değerini** veriyoruz, _name_'in **adresini** değil. Bu sebeple _name_ değişkeninde yapılan bir güncelleme _displayName_ değişkenini etkilemiyor.

## Reference Types

Reference veri tipi için aslında kısaca **objeler** diyebiliriz. **array**'ler de reference veri tipi tanımının içine dahildir. Arrayler aslında bir objedir. Nerden mi anlayabiliriz?

```js
typeof []; // object
```

Reference veri tipleri için memory'de tutulan şey, bu verilerin memory'deki adresleridir. Bu sebeple aşağıdaki kod bloğundaki çıktılar şaşırtıcı olmayacaktır.

```js
let araba = {
  type: "Sport",
  name: "Porche",
};
let baskaAraba = araba;
let baskabirAraba = {
  type: "Sport",
  name: "Porche",
};
baskaAraba.name = "Mercedes";
console.log(araba); //Mercedes
console.log(baskaAraba); //Mercedes
console.log(araba === baskaAraba); //true
console.log(araba === baskabirAraba); //false
```

## Expressions&Statements

### **Expressions**

_Expression_; kısaca value **üreten** ifadelerdir.

```js
let x = 5;
let text = "hello world";
```

### **Statements**

_Statement_; kısaca bir **aksiyon gerçekleştiren** durumlardır.

```js
if (5 > 3) console.log("işlem");
```

### **Operators**

JavaScript'te birçok operatör grubu bulunmaktadır. _Aritmetic operators_, _Assignment operators_, _Comparison operators_ ve _Logical operators_ bunlardan bazılarıdır.

Hepsini tek tek detaylı bir şekilde açıklamak yerine stratejik birkaç noktaya değinelim.

_Aritmetic operators_

```js
let toplam = 8;
toplam++; // toplam = toplam + 1

let a = toplam++; // a = 8, toplam = 9
let b = ++toplam; // b = 9, toplam = 9
```

_Assignment operators_

```js
let a = 3;
// +=
let x = 5;
x += 3; // x = 5 + 3
x -= 3; // x = x - 3

let text = "Hello" + " " + "World";
```

_Comparison operators_

```js
// == equals to
// === equal value and equal type
// != not equal to
// !== not equal value and not equal type
// > greater than < less than
// ? ternary

3 == "3"; // true
3 === "3"; // false

let age = 100;
let isOld = age > 90 ? "Yes" : "No";
console.log(isOld); //Yes
```

_Logical operators_

```js
// && logical and
// || logical or
// ! logical not

console.log(3 > 2 && 5 > 4); //true
console.log(1 > 5 || 3 > 6 || 10 > 8); //true

const ornek = 3 && 5 && 1 && 0 && 2;
console.log(ornek); // 0

const ornek2 = 0 || false || undefined || 5 || null;
console.log(ornek2); // 5

const firstName = person && person.name && person.name.firstName;

//optional chaning
const firstName = person?.name?.firstName; // yukarıdaki ifadenin aynısı

const isOpen = true;
function toggleDropdown() {
  isOpen = !isOpen;
}
```

Aşağıdaki örnekte olduğu gibi bir **objeyi** _not_ operatörünü iki defa kullanarak objenin **boolean** değerine dönüştürebiliriz.

```js
!!someObject; // conversion
```

### NaN Property

> **NaN**; Not-A-Number

```js
const deger = 5 * "hello";
console.log(deger); // NaN
typeof deger; //"number"
//Açıklanamayan bir sayı gibi düşünülebilir. Not a number
console.log(NaN === NaN); // false. Değerini bilmediğimiz iki şeyi karşılaştırdığımızda false alırız.
console.log(NaN == NaN); // false. Değerini bilmediğimiz iki şeyi karşılaştırdığımızda false alırız.
//NaN kendisi dahil hiçbir şeye eşit olmayan bir değer. Falsy bir değerdir.
```

![Variable](/source/week_1.2/img/nan.png)

## If-Else Statement

If-Else statement ile verilen değerin _truthy_ mi _falsy_ mi olduğunu ölçtüğümüz kısa bir uygulama yapalım.

```js
function logTruthiness(val) {
  if (val) {
    console.log("Truthy!");
  } else {
    console.log("Falsy!");
  }
}
logTruthiness(true); //Truthy
logTruthiness({}); //Truthy
logTruthiness([]); //Truthy
logTruthiness("some string"); //Truthy
logTruthiness(3.14); //Truthy
logTruthiness(new Date()); //Truthy

logTruthiness(false); //Falsy
logTruthiness(null); //Falsy
logTruthiness(""); //Falsy
logTruthiness(undefined); //Falsy
logTruthiness(NaN); //Falsy
logTruthiness(0); //Falsy
```

**Ternary** operatör ile bazı durumlar için if-else statement'ı daha kısa ve pratik bir şekilde oluşturabiliriz.

```js
const getFee = function () {
  return isMember ? "$2.00" : "$10.00";
};
```

Birden çok case'in bulunması durumunda switch-case yapısının kullanılması daha temiz ve anlaşılır bir kod yazmamızı sağlayacaktır.

> _switch-case_ yapısını incelemek için: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch

## Loops

Bu kısımda yapılarını incelemek isteyenler için var olan döngülerin linklerini bırakacağım.

> _while loop_: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while

> _do...while loop_: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/do...while

> _for loop_: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for

## Functions

### Function Declarations vs Function Expressions

```js
function example1() {
  return "A function declaration";
}

const example2 = function () {
  return "A function expression";
};

const example3 = () => {
  return "An arrow function";
};
```

**Function declaration**'larda _hoisting_ vardır, **function expression**'larda yoktur. Hoisting konusu bir sonraki haftanın konusudur ve detaylı bir şekilde anlatılacaktır.

> **IIFE**: Immediatly Invoked Function Expressions

**IIFE** oluşturduğumuz anda çağrılan fonksiyonlardır. Önümüzdeki haftanın konusu olduğu için daha detaylı bir şekilde anlatılacaktır.

```js
(function multiply(a, b) {
  console.log(a * b);
})(3, 5);
```

## RegEx

**RegEx** yani _Regular Expressions_, bir _pattern_ kullanarak string'lerde karakter kombinasyonları üzerinde işlemler yapmamızı sağlar. String validation _RegEx_'in en sık kullanılan alanıdır.

```js
const regexp = new RegExp("pattern", "flags");
const myRegex = /pattern/;
const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
re.test("a@b.com"); //true
re.rest("123343"); //false

let str = "We will, we will rock you!";
str.match(/w/); // ilk bulduğunu döndürür
str.match(/w/g); // global flag ile arar, ne kadar varsa döndürür
str.match(/w/gi); // i flagi ile case sensitivity yi kaldırır.

str.replace(/we/, "I"); //We will, I will rock you!
str.replace(/we/gi, "I"); //I will, I will rock you!
```

# Ek Kaynaklar

- https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/
- https://developer.mozilla.org/en-US/docs/Glossary/Primitive
