# 30.01.2021 Cumartesi Notları

> Konular: Execution Context, Functions, Scope, Hoisting, Closures, Objects, new keyword, this keyword, bind, apply, call

## Execution Context

Bu kısımda bakış açımızı değiştirip kodlara JavaScript engine'in gözünden bakacağız. Temelde iki tip execution context olduğunu bilelim. **Global Execution Context** ve **Functional Execution Context**.

### Global Execution Context

Bir JavaScript uygulamasını başlattığımızda hiçbir kod yazmasak dahi JavaScript _Execution Stack_ içerisine _Global Execution Context_'i push eder ve bir _global object_ oluşturur. Bu obje client tarafında _window_ objesi olarak adlandırılır fakat NodeJS olsaydı server tarafında _global_ olarak adlandırılacaktır.

> **window** objesine _this_ keyword'ü ile ulaşabiliriz.

![Global Execution Context](/source/week_2.1/img/1.png)

### Functional Execution Context

Bir fonksiyon çalıştırıldığında JavaScript o fonksiyona ait olan _Execution Context_'i oluşturur ve _Execution Stack_'e push edilir. Fonksiyonun çalışması bittiğinde stack'ten çıkarılır.

```js
var name = "John";
var handle = "@john";

function getUser() {
  return {
    name: name,
    handle: handle,
  };
}

getUser();
```

Yukarıdaki kod parçasını çalıştırdığımızda _getUser_ fonksiyonuna ait bir execution context oluşturulur ve fonksiyonun çalışması bittiğinde fonksiyona ait olan execution context, stack'ten çıkar.

![Functional Execution Context](/source/week_2.1/img/2.png)

> Fonksiyonlar _Execution Stack_'e diğer adıyla _Callstack_'e toplanırlar ve işleri bittikten sonra çıkarlar. Stack'ler _LIFO_ yani Last In First Out mantığıyla çalıştığı için son giren fonksiyon ilk çıkacaktır.

> JavaScript _singlethread_ bir dildir. Aynı anda sadece bir görev execute edilebilir.

JavaScript engine'in nasıl çalıştığını anlamak için görselleştiren siteler:

- https://ui.dev/javascript-visualizer/
- https://www.jsv9000.app/

### Function Declarations vs Function Expressions

Geçen hafta kısaca _function declaration_ ve _function expression_ nedir görmüştük. Bu bağlamda _function declaration_'larda _hoisting_ olduğunu ama _function expression_'larda _hoisting_ olmadığını biliyoruz. Bu durum global execution context'e nasıl yansıyor ona bakalım.

```js
example1(); // A function declaration
example2(); // Uncaught TypeError: example2 is not a function
example3(); // Uncaught ReferenceError: Cannot access 'example3' before initialization

function example1() {
  return "A function declaration";
}

var example2 = function () {
  return "A function expression";
};

const example3 = function () {
  return "Another function expression with const declaration";
};
```

Yukarıdaki kod bloğunu çalıştırdığımızda _example1_ fonksiyonu için her şey yolundayken _example2_ ve _example3_ fonksiyonları için hatalar alıyoruz. Bunun sebebi _function declaration_ olan _example1_, henüz **Phase: Creation**'dayken _function_ olarak tanımlanıyor. _function expression_ olan _example2_, **Phase: Creation**'dayken _undefined_ olarak tanımlanıyor ve _undefined_ bir değeri fonksiyon olarak çalıştırmak istediğimiz için hata alıyoruz. Bir başka _function expression_ olan _example3_, _const_ ile tanımlanan bir değişkene atandığından, _const_'un yapısı gereği değişken tanımlanmadan o değişkene ulaşamıyoruz.

## Scope

**Scope**; bir değişkenin, fonksiyonun ya da objenin programın farklı kısımlarından erişilebilir ya da erişilemez olma durumunu tanımlar.

> Not: **{}** içinde kalan kodların tümüne _block_ denir.

JavaScript'te üç farklı _scope_ vardır:

- Global Scope
- Local Scope (Function Scope)
- Block Scope

### Global Scope

Eğer bir değişken tüm fonksiyonların ve süslü parantezlerin dışında bir yerde tanımlandıysa o değişken _global scope_'tadır. Global scope'taki değişkene programın her satırından erişilebilir.

```js
var bar = "Declared in global scope";

function foo() {
    console.log(bar);
}
foo(): // Declared in global scope
```

### Local Scope (Function Scope)

Yazılan her fonksiyon için JavaScript yeni bir _local scope_ oluşturur ve bu scope içerisinde tanımlanan her değişken _local değişken_'dir. Yani aynı isimle farklı fonksiyonlarda değişkenler tanımlanabilir.

```js
function first() {
  var name = "John";
  console.log(name);
}

function second() {
  var name = "Joe";
  console.log(name);
}

console.log(name); //undefined
var name = "Mehmet";
first(); //John
second(); //Joe
console.log(name); //Mehmet
```

Bu değişkenleri fonksiyonun dışında kullanmaya kalkarsak hata alırız.

```js
function foo() {
  var bar = "Declared in foo";
}

console.log(bar); // Reference error
```

> **scope chain**: Fonksiyon, bir değişken için önce kendi scope'una bakar, bulamazsa bir üst scope'a bakar ve bu şekilde devam eder. En son olarak da global scope'a bakar.

---

```js
function discountPrices(prices, discount) {
  let discounted = [];
  for (let i = 0; i < prices.length; i++) {
    let discountedPrice = prices[i] * (1 - discount);
    let finalPrice = Math.round(discountedPrice * 100) / 100;
    discounted.push(finalPrice);
  }
  console.log(i); // reference error
  console.log(discountedPrice); // üstteki satır hata verdikten sonra bu satıra geçilmeyecek
  console.log(finalPrice); // üstteki satır hata verdikten sonra bu satıra geçilmeyecek

  return discounted; // [50, 100, 150]
}
discountPrices([100, 200, 300], 0.5); // [50, 100, 150]
```

```js
function discountPrices(prices, discount) {
  var discounted = [];
  for (var i = 0; i < prices.length; i++) {
    var discountedPrice = prices[i] * (1 - discount);
    var finalPrice = Math.round(discountedPrice * 100) / 100;
    discounted.push(finalPrice);
  }
  console.log(i); // 3
  console.log(discountedPrice); // 150
  console.log(finalPrice); // 150

  return discounted; // [50, 100, 150]
}
discountPrices([100, 200, 300], 0.5); // [50, 100, 150]
```

---

> _var_ ve _let_ hoisted oluyor fakat creation phase'de var'a undefined değeri atanıyor let e atanmıyor, execution phase'de let'e de undefined atanıyor.

```js
console.log(x); // undefined
console.log(y); // Reference error

var x; // creation phase'de var x = undefined oluyor
let y; // creation phase'de değer atanmıyor, execution phase'de let y = undefined oluyor
```

## Closures

Kısaca function return eden bir function; bunlara _closure_ diyoruz.

```js
var count = 0;

function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}

var add5 = makeAdder(5);

count = count + add5(2); // 7 döndürülür.
```

Yukarıdaki kod parçasında _makeAdder_ fonksiyonu bir fonksiyon return etmiştir. Return edilen bu _inner function_ bir üst scope'undaki **x** değişkenine erişebilir. Normalde bir fonksiyon çağrıldığında o fonksiyona ait bir _execution context_ oluşturulur ve _callstack_'e push edilir. Fonksiyonun işi bittiğinde stack'ten çıkar. _Closure_ fonksiyonlarda ise bir _Closure Scope_ oluşturulur. Yukarıdaki örnekte _makeAdder_ fonksiyonu için bir _Closure Scope_ oluşturulmuştur. Inner function için _Closure Scope_ içinde ayrı bir _Execution Context_ oluşturulmuştur. Bu sayede inner function, _Closure Scope_'taki herhangi bir değişkene erişebilmektedir.

![Closure](/source/week_2.1/img/closure.png)

### Döngü İçinde Closure Yapılar

```js
function runExample() {
  for (var i = 0; i < 4; i++) {
    setTimeout(() => {
      console.log(i);
    }, i * 1000);
  }
}
runExample();
//output : 4,4,4,4
```

Yukarıdaki yapıda aslında beklenen çıktı **"1,2,3,4"**'tü. Fakat döngü içerisindeki _var_ ile oluşturduğumuz _i_ değişkeni _runExample_ fonksiyonunun scope'unda ve _setTimeout_ fonksiyonu _i_ değişkenini bu scope'tan alıyor. Bunların yanısıra _setTimeout_ fonksiyonu asenkron bir fonksiyon ve bu fonksiyonlar çalışmaya başlayana kadar döngü tamamlanıyor, _i_ değişkeninin değeri fonksiyonlar çalışmaya başlamadan evvel çoktan 4'e ulaşıyor. Fonksiyonlar çalışmaya başladığında ise _i_'nin değerine bakıyorlar ve 4 olarak yazdırıyorlar. Bunun önüne geçmek için _closure_ bir yapı oluşturabiliriz.

```js
function runExample() {
  for (var i = 0; i < 4; i++) {
    (function (a) {
      setTimeout(() => {
        console.log(a);
      }, a * 1000);
    })(i);
  }
}
runExample();
//output: 0
//output: 1
//output: 2
//output: 3
```

Bu şekilde _closure_ bir yapı oluşturduğumuzda inner function'a parametre olarak döngüdeki _i_ değeri verilir ve döngüdeki her bir adım için _i_ değerini inner function kendi scope'una alır. Değişkenin 4 farklı kopyasını oluşturur. Böylece istediğimiz çıktıyı elde edebiliriz. Bunun dışında _let_ ile de sorunun önüne geçebiliriz.

```js
function runExample() {
  for (let i = 0; i < 4; i++) {
    setTimeout(() => {
      console.log(i);
    }, i * 1000);
  }
}

runExample();
//output: 0
//output: 1
//output: 2
//output: 3
```

Bu şekilde _let_ tanımlaması ile for döngüsü içerisinde her bir adım için yeni bir değişken oluşur.

## Objects

```js
let user = {} // object literal
let user = new Object() // constructor syntax

let user = {
    name: "John",
    age: 30
} // object with properties

let user = {
    name: "John",
    "his hobbies": [] // multi-word property yazım şekli
    books: []
}

user["his hobbies"] // multi-word bir property'e bu şekilde ulaşabiliriz

let keyword = "books";
user[keyword] // bu şekilde property'e dinamik olarak ulaşabiliriz.

console.log(age in user); // user içinde age property'si var mı ifadesi. true döner.
```

## this keyword

### 1.Implicit this

JavaScript'in kendi kararı olan durumlar.

```js
const user = {
    name: "John",
    age: 27,
    greet(){
        console.log("Hello, my name is" + this.name)
    }
    mother: {
        name: "Mary",
        greet(){
            console.log("Hello, my name is" + this.name)
        }
    }
}

user.greet(); // noktanın soluna bak: user. Sonuç olarak this bizim için user'dır.
user.mother.greet() //noktanın soluna bak: mother. Sonuç olarak this bizim için mother'dır.
```

### 1.Explicit this

Dışarıdan müdahale olduğunda yani bizim müdahale ettiğimiz durumlar.

```js
function greet() {
  console.log("Hello, my name is " + this.name);
}

const user = {
  name: "Joe",
  age: 27,
};

// bind, apply, call

// call
// call metodu ile fonksiyonun this'i şu olsun diyoruz
greet.call(user); // Hello, my name is Joe

// bind
let myFunc = greet.bind(user); // bind'ın call'dan farkı direkt çağırmıyoruz. Bir değişkene atıyoruz.

// apply
// call gibi direkt olarak çağırır. call'dan farkı parametreleri array olarak istemesidir
function greet(l1, l2, l3) {
  console.log(
    "Hello, my name is " + this.name + " I know" + l1 + ", " + l2 + ", " + l3
  );
}
greet.apply(user, ["JavaScript", "Python", "Rust"]);
greet.call(user, "JavaScript", "Python", "Rust"); // call da array olarak değil de tek tek parametre verilir. bind'da da aynı şekilde.
```

# Ders İçerisinde Önerilen Kaynaklar

- https://ui.dev/ultimate-guide-to-execution-contexts-hoisting-scopes-and-closures-in-javascript/
- https://ui.dev/this-keyword-call-apply-bind-javascript/
- https://github.com/leonardomso/33-js-concepts
- https://frontendmasters.com/dis

# Ek Kaynaklar

- https://serbestemre.medium.com/execution-stack-ve-execution-context-nedir-ee99dd9ed925
- https://ui.dev/ultimate-guide-to-execution-contexts-hoisting-scopes-and-closures-in-javascript/
- https://dev.to/ale3oula/the-hor-r-o-r-scope-global-local-and-block-scope-in-js-37a1
- http://hakan.in/2019/Javascript-closures-functions/
