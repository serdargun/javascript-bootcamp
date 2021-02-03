# 31.01.2021 Pazar Notları

> Konular:

## this keyword

```js
// 3. new Binding
function User(name, age) {
  this.name = name;
  this.age = age;
}

const Joe = new User("Joe", 27);
```

```js
// 4. lexical binding
//kendisinde olmayan şeyin parentta var mı diye bakma durumu
const user = {
  name: "Joe",
  age: 27,
  languages: ["JavaScript", "Python"],
  greet() {
    const hello = "Hello my name is " + this.name + " and I know";
    const langs = this.languages.reduce(function (str, lang, i) {
      if (i === this.languages.length - 1) {
        return str + " and " + lang;
      }
      return str + " " + lang;
    }, "");
    console.log(hello + langs);
  },
};

user.greet(); // hata alırız... reduce içindeki function'ın içindeki this yüzünden
//function yerine arrow function kullanabiliriz. Arrow function içindeki this parent'ının this'i ne ise odur.

const user = {
  name: "John",
  sayMyName() {
    setTimeout(function () {
      console.log("Says " + this.name + " after 1 second");
    }, 1000);
  },
};

user.sayMyName(); //Says after 1 second... this.name undefined oldu... yine aynı şekilde setTimeout içindeki function yerine arrow function kullanarak sorun çözülebilir.
```

```js
// 5. window binding
window.age = 27;
function sayAge() {
  console.log("My age is" + this.age);
}
sayAge(); //solunda window var aslında.
//window.sayAge();
```

## Type Coercion

```js
Array(16).join("wat" - 1) + " Batman"; //sonucu gözlemle
```

```js
// Implicit Coercion vs Explicit Coercion
//çevirme üç tip stringe booleana ya da number'a

//String
String(123); //Explicit coercion
123 + ""; // Implicit coercion, biz sadece number ile stringi topluyoruz JavaScript kendisi bunu stringe çeviriyor.
String(-12.3); //"-12.3"
String(null); //"null"
String(true); //"true"

//Boolean
Boolean(2); // Explicit coercion ... true
if (2) {
  console.log("Hello");
} //implicit due to logical context ... true
2 || "Hello"; // implicit due to logical context ... true

Boolean(""); //false
Boolean(0); //false
Boolean(null); //false

Boolean({}); //true, memory'de tuttuğu bir referans var
Boolean([]); //true, memory'de tuttuğu bir referans var
Boolean(-1); //true

//Numeric conversion
Number("123") + //123 //explicit
  //comparison operators (>, <, <=, >=)
  //arithmetic operators (-, +, *, /, %) //taraflardan biri string ise + operatörü numeric conversion tetiklenmez
  //unary +
  // loose equality (==), != operatoru iki taraf da string ise numeric conversion yapmaz
  "123"; //123
123 != "456"; // 123 != 456 //implicit

4 > "5"; // 4 > 5
5 / null; // 5 / 0 infinity //implicit coercion

Number(true); // 1 //explicit coercion
Number(false); // 0
Number(" 12 "); // önce boşlukları siler // 12
Number("  "); // 0
Number(" 12ab "); //NaN

NaN == NaN; //false, kendisi dahil hiçbir şeye eşit değil.
```

```js
//reference type'da type coercion
//reference type'da JavaScript'in ilk yapmaya çalıştığı şey primitive tipe çevirmeye çalışmak olacaktır.
Boolean({}) //true
Boolean([]) //true

//reference type'da type coercion yapılırken JavaScript motorunun işlediği adımlar şunlardır:
//1. değer primitive ise hiçbir şey yapma
//2. input.toString(), primitive ise dur, return
//3. input.valueOf(), primitive ise dur, return
//4. TypeError

true + false // 1 + 0 yani 1
12 / "6" // 12 / 6 yani 2
"number" + 15 + 3 // number153
15 + 3 + "number" // 18number

[1] > null // "1" > null --> 1 > 0 --> true
[1, 2, 3] > null // "1, 2, 3" > null --> NaN > 0 --> false

[] + [] // "" + "" --> ""

"foo" + + "bar" // "foo" + NaN --> "fooNaN"

"true" == true // NaN == 1 --> false

["x"] == "x" // "x" == "x" --> true

[] + null + 1 // "" + "null" --> "null" --> "null" + 1 --> "null1"

[1, 2, 3] == [1, 2, 3] // burada coercion'a gerek yok çünkü ikisi de aynı tip. false döner çünkü memory adresleri farklı.

{} + [] + {} + [1] // JavaScript, ilk gelen curly braces'ları obje olarak değil de blok olarak görür ve ignore eder.
// + [] + {} + [1] --> + "" + {} + [1] --> 0 + {} + [1] --> 0 + "[object Object]" + [1] --> "0[object Object]" + [1] -->
// "0[object Object]" + "1" --> "0[object Object]1"
```

https://getify.github.io/coercions-grid/
https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/
