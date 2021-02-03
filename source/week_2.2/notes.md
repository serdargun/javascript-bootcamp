# 31.01.2021 Pazar Notları

> Konular: this keyword, Type Coercion

## this keyword

Bir önceki gün _this_'in ilk iki varyasyonunu görmüştük. Şimdi kaldığımız yerden devam edelim.

### 3. new Binding

```js
function User(name, age) {
  this.name = name;
  this.age = age;
}

const Joe = new User("Joe", 27);
```

### 4. Lexical Binding

Kısaca; kendisinde olmayan parent'ta var mı diye bakma durumu olarak düşünülebilir.

```js
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

user.greet(); // hata
```

Yukarıdaki kod parçasında hata aldık çünkü _reduce_ içindeki function'ın içindeki _this_, _greet_'i işaret ediyor fakat _languages_ değişkeni _user_ bloğu içerisinde. Sorunu çözmek için function yerine _arrow function_ kullanabiliriz. _Arrow function_ içindeki _this_ parent'ının _this_'i ne ise odur.

```js
const user = {
  name: "Joe",
  age: 27,
  languages: ["JavaScript", "Python"],
  greet() {
    const hello = "Hello my name is " + this.name + " and I know";
    const langs = this.languages.reduce((str, lang, i) => {
      if (i === this.languages.length - 1) {
        return str + " and " + lang;
      }
      return str + " " + lang;
    }, "");
    console.log(hello + langs);
  },
};

user.greet();
```

```js
const user = {
  name: "John",
  sayMyName() {
    setTimeout(function () {
      console.log("Says " + this.name + " after 1 second");
    }, 1000);
  },
};

user.sayMyName(); //Says after 1 second
```

`this.name` undefined oldu. Yine aynı şekilde setTimeout içindeki function yerine _arrow function_ kullanarak sorun çözülebilir.

```js
const user = {
  name: "John",
  sayMyName() {
    setTimeout(() => {
      console.log("Says " + this.name + " after 1 second");
    }, 1000);
  },
};

user.sayMyName(); //Says after 1 second
```

### 5. window Binding

```js
window.age = 27;
function sayAge() {
  console.log("My age is" + this.age);
}
sayAge(); //solunda window var aslında.
//window.sayAge();
```

## Type Coercion

![Type Coercion](/source/week_2.2/img/type-coercion.jpg)

**Type Coercion**, bir tipteki değeri başka bir tipteki değere dönüştürme işlemidir.

```js
console.log(Array(16).join("wat" - 1) + " Batman");
//"NaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaN Batman"
```

Yukarıdaki kod parçasında gördüğünüz gibi _string_ bir değerden _number_ bir değer çıkartılıyor ve ortaya hiç de sürpriz olmayan bir durum çıkıyor.

Daha önce _this keyword_'deki **implicit** ve **explicit** durumlarını görmüştük. Aynı durum **type coercion** için de geçerli. JavaScript'in otomatik olarak yaptığı tip dönüşümlerine **Implicit Coercion**, geliştiricinin müdahalesiyle yapılan tip dönüşümlerine **Explicit Coercion** deniyor.

_Type Coercion_ temelde üç şekilde oluyor; _string_'e, _boolean_'a ya da _number_'a.

```js
//String
String(123); //Explicit coercion
123 + ""; // Implicit coercion, biz sadece number ile stringi topluyoruz JavaScript kendisi bunu stringe çeviriyor.
String(-12.3); //"-12.3"
String(null); //"null"
String(true); //"true"

//Boolean
Boolean(2); // Explicit coercion  //true
if (2) {
  console.log("Hello");
} // implicit due to logical context //JavaScript if'in koşulu olan 2 değerini true olarak çeviriyor
2 || "Hello"; // implicit due to logical context  //true

Boolean(""); // false
Boolean(0); // false
Boolean(null); // false

Boolean({}); // true, memory'de tuttuğu bir referans var
Boolean([]); // true, memory'de tuttuğu bir referans var
Boolean(-1); // true

// Numeric
Number("123"); // Explicit coercion //123
// comparison operators (>, <, <=, >=)
// arithmetic operators (-, +, *, /, %) // taraflardan biri string ise + operatörü numeric conversion tetiklemez.
// unary +
// loose equality (==), != operatoru iki taraf da string ise numeric conversion yapmaz.
123 != "456"; // 123 != 456 olarak okunur. // Implicit coercion

4 > "5"; // 4 > 5
5 / null; // 5 / 0 infinity //Implicit coercion

Number(true); // 1 // Explicit coercion
Number(false); // 0
Number(" 12 "); // Önce boşlukları siler // 12
Number("  "); // 0
Number(" 12ab "); // NaN

NaN == NaN; // false // NaN, kendisi dahil hiçbir şeye eşit değil.
```

```js
//Reference type'da type coercion
//Reference type'da JavaScript'in ilk yapmaya çalıştığı şey primitive tipe çevirmeye çalışmak olacaktır.
Boolean({}) //true
Boolean([]) //true

//Reference type'da type coercion yapılırken JavaScript motorunun işlediği adımlar şunlardır:

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

[] + null + 1 // "" + null --> "null" --> "null" + 1 --> "null1"

[1, 2, 3] == [1, 2, 3] // Burada coercion'a gerek yok çünkü ikisi de aynı tip. false döner çünkü memory adresleri farklı.

{} + [] + {} + [1] // JavaScript, ilk gelen curly braces'ları obje olarak değil de blok olarak görür ve ignore eder.
// + [] + {} + [1] --> + "" + {} + [1] --> 0 + {} + [1] --> 0 + "[object Object]" + [1] --> "0[object Object]" + [1] -->
// "0[object Object]" + "1" --> "0[object Object]1"
```

# Ders İçerisinde Önerilen Kaynaklar

- https://getify.github.io/coercions-grid/
- https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/
