# 23.01.2021 Cumartesi Notları

> Konular: Intoduction to JavaScript, Philosophy and Tips, ECMAScript, TC39 and the Standardization Process

## Introduction to JavaScript

![JavaScript](/source/week_1.1/img/js.png)

### _JavaScript ne zaman çıktı? Ne amaçla çıktı? Nasıl Gelişti?_

**JavaScript** dili **1995** yılında duyuruldu. Dilin temel amacı web sitelerini interaktif hale getirebilmektir. Butona tıklama, scroll işlemi gibi interaktif işlemlerin kontrolünü sağlama amacını gütmektedir. JavaScript ile yazılan programlara script denir.

İlk çıktığında **LiveScript** ismiyle duyuruldu. Daha sonra Java dilinin var olan popülaritesinden faydalanabilmek için JavaScript ismi verilmiştir. Java ile tek ortak noktası isim benzerliğinin olmasıdır.

JavaScript ilk olarak tarayıcıda çalışması için çıktı. Şu an backendde, mobilde ve desktopta çalışabiliyor. JavaScript'in çalışması için gerekli olan şey bir JavaScript Engine'dir. JavaScript Engine örneği olarak Chrome için geliştirilen **V8 Engine** ve React Native için geliştirilen **Hermes JS** verilebilir.

## Philosophy and Tips

### _JavaScript ile neler yapılabilir?_

JavaScript ile bir sayfaya **HTML** eklenebilir, stillendirilebilir. Kullanıcı aksiyonları yönetilebilir. Uzak sunucuya istekler atılabilir. Dosya işlemleri yapılabilir. Client tarafında data tutulabilir.

### _Alternatifi var mı?_

Tarayıcıda yorumlanabilen tek programlama dili JavaScript'tir.

JavaScript isminin lisansını Oracle firması aldığı için resmi tanımlarda JavaScript'e **ECMAScript** denir. Script tagları arasında yazılan her şey JavaScript olarak değerlendirilir. Bu **internal** olarak JavaScript'i sayfaya ekleme yöntemidir. Daha sık kullanılan yöntem ise **external** olarak sayfaya eklemektir.

## ECMAScript

ECMAScript, JavaScript'in resmiyetteki adıdır. ECMA-262 standardında, JavaScript adı kullanılmamaktadır.

## TC39 and the Standardization Process

> [**TC39**](https://github.com/tc39): ECMA-262 standardı üzerinde çalışan komünite.

TC39, daha çok tarayıcı satıcıları ve web'e yoğun bir şekilde yatırım yapan büyük şirketler (Facebook, Netflix vb.) olan üyelerden oluşur. Bu şirketler kuruma kendilerini temsil etmesi için delegeler göndermektedir. Bu delegeler, yeni JavaScript özelliklerini onaylamak, geliştirmek ya da reddetmek için oylama yöntemi ile karar verirler. Her yılın Haziran ayında yeni sürümü yayınlamak için karar kılmışlardır.

> **ECMAScript Proposals**: JavaScript'te yapılacak yenilik teklifleri.

Kişiler JavaScript'e yeni bir özellik getirmek istediğinde bu kurula o özelliği öneri olarak sunar. Bu önerinin JavaScript'in yeni sürümünde kabul görmesi için 5 aşamadan geçmesi gerekmektedir.

![Proposal](/source/week_1.1/img/proposal.jpg)

### _Stage 0 - Gösterim Aşaması_

Her yeni teklif Stage 0'da başlar. Stage 0 teklifleri; bir TC39 üyesi tarafından komiteye sunulması planlanan veya komiteye sunulmuş ama kesin olarak reddedilmemiş, ancak 1. aşamaya geçme kriterlerinden herhangi birine henüz ulaşamamış tekliflerdir. Dolayısıyla, bir teklifin Stage 0'da olmasının tek şartı, belgenin bir TC39 toplantısında gözden geçirilmesi gerektiğidir. Herkes bu aşamaya teklif verebilir.

### _Stage 1 - Teklif_

Bir teklifin Stage 1'de olabilmesi için TC39'un bir parçası olan tekliften sorumlu bir **şampiyon** belirlenmelidir. Ya da teklifin bir TC39 üyesi tarafından sunulmuş olması gerekmektedir. Bunlara ek olarak teklifin tartışılmış olması ve çözdüğü sorunun tanımlanmış olması gerekmektedir. Komite teklifi daha derinlemesine analiz etmek için kaynakları incelemeye istekli olduklarını belirtirlerse teklif **Stage 2**'ye geçebilir.

### _Stage 2 - Taslak_

Bu aşamada teklifin tüm yönlerinin ortaya serilmesi gerekmektedir. Yani ana taslak ortaya çıkmış olmalıdır. Syntax'in büyük çoğunluğu belirlenmiş olmalıdır. Gelecekte üzerinde değişiklikler yapılabilir ancak bunlar küçük değişimler olmalıdır.

### _Stage 3 - Aday_

Bu aşamada teklif çoğunlukla bitmiştir ve sonraki aşamaya geçmek için kullanıcılardan geri bildirime ihtiyaç vardır.

### _Stage 4 - Onaylanma_

Teklif bu aşamada bir sonraki gelecek olan ECMAScript sürümüne dahil edilmeye hazırdır.

> **Babel** ile next generation'daki proposalları çevirip kullanabiliyoruz. Siz de [Babel REPL](https://bvaughn.github.io/babel-repl/) ile çeviri işlemleri yapıp sonuçları gözlemleyebilirsiniz.

> Ek olarak, eğitmenimiz [Mehmet Yurtar](https://github.com/yurtarmehmet)'dan kitap tavsiyesi; [The Nature of Code - Daniel Shiffman](https://natureofcode.com/)

# Ek Kaynaklar

- https://medium.com/@bilgedemirkaya/ecma-ecmascript-tc39-nedir-javascript-nas%C4%B1l-ortaya-%C3%A7%C4%B1kt%C4%B1-3ebb3b9a87ea
- https://dev.to/gethackteam/5-ecmascript-proposals-to-look-out-for-in-es2020-36hp#:~:text=The%20ECMAScript%20Process,interested%20in%20exploring%20solutions%20for.
- https://medium.com/mol42/derinlemesine-javascript-javascript-motoru-nedir-nas%C4%B1l-%C3%A7al%C4%B1%C5%9F%C4%B1r-c129b49f089c
