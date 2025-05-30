#  Events and navigating DOM

*******************
#### Vsuvka k CSS:
* pokud místo konkrétní barvy zadám do CSS `currentColor`, nastaví se to na barvu textu - buď automatickou, pokud jsem žádnou nezadal, nebo tu, kterou jsem nastavil:
  ```CSS
  .title {
    font-size: 48px;
    color: blue;
    border-bottom: 5px solid currentColor;
  }
  ```
*******************

## Manipulating with elements' classes

* pomocí `className` můžu zjistit třídu prvku:

  ```javascript
  console.log(title.className);
  ```
  * nemůžu jen použít slovo `class`, to má v javascriptu jinou funkci

* potom můžu taky pomocí `className` přenastavit třídu na něco jiného, třeba jinou předpřipravenou třídu:

  ```javascript
  title.className = "highlight";
  ```

* pokud bych chtěl těch tříd mít víc, a jen přidat nějakou další, tak buď to nastavím vše najednou, nebo i přes `+=`:

  ```javascript
  title.className = "title highlight"; // nastavim od zacatku vic trid
  title.className += " highlight"; // pridam dalsi tridu, nezapomenout na mezeru!
  ```
  * nevýhoda toho `+=` přidávání je, že pokud tam ta stejná třída třeba už je, tak tam bude stejná třída víckrát

* další (a asi lepší při větším množství tříd) vlastnost, kterou můžu použít, je `classList`, ta má 3 metody - `add()`, `remove()`, `toggle()` : 

  ```javascript
  title.classList.add("highlight"); // přidá třídu, kdyby tam už náhodou byla, tak se neduplikuje

  title.classList.remove("highlight"); // odstraní třídu

  title.classList.toggle("highlight"); // když tam ta třída není, tak ji přidá a když tam je, tak ji odebere

  ```
* 

## Manipulating with elements' atributes

```javascript
console.log(odkaz.href); // můžu se odkazovat na atributy prvků jen takhle přes tečku

console.log(odkaz["href"]); // nebo do hranatych zavorek a uvozovek
```
* ale existuje set metod, které přímo pracují s atributy:
  * `getAttribute("atribut")`
  * `setAttribute("atribut", "novaHodnotaAttribute")`
  * `removeAttribute("atribut")`

  ```javascript
  console.log(odkaz.getAttribute("href")); // zjisti attribute

  odkaz.setAttribute("href", "https://seznam.cz"); // nastavi attribute, druha hodnota je, na co to nastavuju

  odkaz.removeAttribute("href"); // odstrani attribute
  ```

* to odkazování na atributy přes tečku nefunguje v hodně starých prohlížečích, takže třeba banky nebo státní instituce můžou vyžadovat používání metody `getAttribute`, která je v JavaScriptu odjakživa a funguje úplně všude

* `removeAttribute` nemůžu nahradit `delete`, protože `delete` na DOM nefunguje:
  ```javascript
  delete odkaz.href; // tohle nejde!
  ```

## Events in DOM

* na webové stránce dochází k nějakým událostem a já na ně pomocí JavaScriptu můžu reagovat

* událost - event - může být např. kliknutí na element

* můžu právě třeba změnit CSS vlastnost elementu v návaznosti na kliknutí

* `event` = událost
* `event listener` = sleduje, zda se nestala událost ("posluchač události")
* `event handler` = spouští javascript, pokud se událost stala ("obsluha události")

* potřebuju si vytvořit funkci `event handler`, která bude mít jako parametr `event listener`:

```javascript
const btn = document.querySelector("button");

function priKliknuti() {
  console.log("klikity klak");
}

btn.addEventListener("click", priKliknuti); // pozor, k te funkci priKliknuti, nebo jine event listener funkci nepridavam kulate zavorky, to bych rikala, ze ji ma hned spustit! jen rikam event handlerovi, ze "tady je nazev funkce, kterou spustis pozdeji, kdyz dojde ke kliknuti"
```

* mohl bych tam místo toho jména zadat anonymní funkci, obzvlášť pokud tu funkci použiju přesně jednou:

  ```javascript
  const btn = document.querySelector("button");

  btn.addEventListener("click", function () {
    console.log("klikity klak");
  }); 
  ```
* pokud ale budu chtít tu funkci použít víckrát, hodí se mi ji mít pod názvem

* můžu takhle kliknutím nastavit něco nějakému odstavci - třeba jiný text:
  ```javascript
  const btn = document.querySelector("button");

  btn.addEventListener("click", function () {

  const content = document.querySelector(".content");
  
  content.textContent = "Ahahahaa";
  });
  ```
* nebo jiný formát:
  ```javascript
  const btn = document.querySelector("button");

  btn.addEventListener("click", function () {

  const content = document.querySelector(".content");
  
  content.style.background = "yellow";
  });
  ```
* můžu nastavit, aby se mi konkrétní obsah schoval/rozbalil až třeba po kliknutí - připravím si předem v CSS třeba třídu `.hidden`, tu pak přidám v JS a tím prvek zmizí:
  ```CSS
  .hidden {
    display: none;
  }
  ```
* to potom použiju v javascriptu - tady se mi po kliknutí na tlačítko schová celý odstavec s třídou `.content` (ale může to být asi klidně jakýkoliv jiný prvek s tou třídou):
  ```javascript
  const btn = document.querySelector("button");

  btn.addEventListener("click", function () {

  const content = document.querySelector(".content");
  
  content.classList.add("hidden");
  });
  ```
* ale pro případy, kdy chci něco ukazovat a pak zase schovávat po kliknutí se mi spíš hodí `.toggle`:

  ```javascript
  const btn = document.querySelector("button");

  btn.addEventListener("click", function () { // ve chvíli, kdy se klikne na tlačítko

  const content = document.querySelector(".content"); // najdi prvek s třídou .content
  
  content.classList.toggle("hidden"); // a přepni na něm třídu .hidden
  });
  ```
* tímhle způsobem tedy neměním HTML elementy jako takové, jen je vybírám a něco s nimi dělám

* taky je možnost to vybírání prvku dát úplně na začátek ještě před funkci - záleží na situaci
  * někdy se mi hodí mít všechno obsažené ve funkci a mít ji nezávislejší na zbytku
  * někdy je naopak lepší to dát na začátek a "ušetřit tím prohlížeči práci":

  ```javascript
  const btn = document.querySelector("button"); // najdi tlačítko
  const content = document.querySelector(".content"); // najdi prvek s třídou .content

  btn.addEventListener("click", function () { // ve chvíli, kdy se klikne na tlačítko
  content.classList.toggle("hidden"); // a přepni na prvku content třídu .hidden
  });
  ```

### Události, na které můžeme reagovat

* [Event index - Mozzila](https://developer.mozilla.org/en-US/docs/Web/Events)
* [HTML DOM Events - W3Schools](https://www.w3schools.com/jsref/dom_obj_event.asp)

* ne všechny události mohou nastat na jakémkoliv prvku, některé jsou specifické pro určité prvky nebo pro stránku jako takovou

* `mouseover` - najetí kurzorem myši na prvek
```javascript
  const btn = document.querySelector("button");

  btn.addEventListener("mouseover", function () {

  const content = document.querySelector(".content");
  
  // content.classList.add("hidden");
  content.classList.toggle("hidden");

  });
```
* `mouseout` - odstranění kurzoru myši z prvku
* `dblclick` - dvojité kliknutí myší
* `mousedown` - stisknutí myši (nemusím ji následně pustit, aby se něco stalo, spustí se hned po stisku tlačítka myši)
* `mouseup` - puštění tlačítka myši poté, co jsem ho stiskl

* do té funkce, která je jako parapetr funkce `event listener`, si javascript do jejího parametru automaticky doplňuje tzv. `event object`
  * `event object` dává popis události, ke které došlo a na kterou ta funkce reaguje
  * pokud bych chtěla, můžu tam dát parametr `event` nebo `e`:
  ```javascript
  const btn = document.querySelector("button"); 
  const content = document.querySelector(".content");

  btn.addEventListener("click", function (event) { 
  content.classList.toggle("hidden"); 
  });
  ```
  * do toho parametru - proměnné - `e` nebo `event` nám Javascript pošle spoustu vlastností, které tu událost popisují
    * můžu si ten objekt zkusit vypsat do konzole:
```javascript
  const btn = document.querySelector("button");

  btn.addEventListener("click", function (event) {

  const content = document.querySelector(".content");

  console.log(event);// tady chci vypsat vlastnosti objektu event
  content.classList.toggle("hidden");

  });  
```
* do konzole se mi pak vypíše toto:
```javascript
PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: 1, pressure: 0, …}
isTrusted: true
altKey: false
//atd., spoustu ruznych vlastnosti
type: "click" // tady vidim, typ eventu - bylo to kliknuti, takze je tam "click"
screenX: -1784 // souradnice, kde k eventu doslo v ramci obrazovky
screenY: 159 // souradnice, kde k eventu doslo v ramci obrazovky
pageX: 136 // souradnice, kde k eventu doslo v ramci stranky
pageY: 38 // souradnice, kde k eventu doslo v ramci stranky
offsetX: 110 // souradnice, kde k eventu doslo v ramci prvku jako takoveho
offsetY: 12 // souradnice, kde k eventu doslo v ramci prvku jako takoveho
altKey: false // informace, zda jsem při kliknutí taky držel klávesu Alt
ctrlKey: false // informace, zda jsem při kliknutí taky držel klávesu Ctrl
shiftKey: false // informace, zda jsem při kliknutí taky držel klávesu Shift
target: button // obsahuje prvek, na kterem k eventu došlo 
```
  * tohle zatím nepotřebujeme, ale budeme potřebovat později, ale využiju toto:

  * `target: button` obsahuje prvek, na kterem k eventu došlo
    * kdybych měl funkci, kterou bych napojil jako reakci na kliknuti na 4 různé tlačítka, tak bych pomocí `event.target` mohl zjistit, na jakém z těch tlačítek k události došlo, a na jakou událost tedy reagujeme

```javascript
function vyberTlacitko(event) {
  console.log("klik na tlacitko");
  console.log(event.target);// vytiskne se mi ten element, ktery event vyvolal, napriklad "<button>1</button>"
}

const tlacitka = document.querySelectorAll(".tlacitka button"); // vybirame vsechna tlacitka uvnitr neceho, co ma tridu .tlacitka

tlacitka.forEach( function(tlacitko) {//nemuzu na tu skupinu jako takovou primo pridat addEventListener, musim je projit treba forEach a pro kazde tlacitko to udelat samostatne
  tlacitko.addEventListener("click", vyberTlacitko);
});
```

* v CSS si taky můžu nastavit, aby se změnil vzhled tlačítka, respektive připravím si tam class, který pak můžu přes `classList.toggle` přidávat a odebírat:
  ```CSS
  .selected {
    background-color: blue;
    color: white;
  }
  ```

  ```javascript
  function vyberTlacitko(event) {//může být lepší si funkci nazvat, nedělat ji anonymní - pokud bych ji udělal anonymní, tak ji bude prohlížeč zbytečně nově vytvářet pro každý z těch prvků, na který ji pomocí forEach přidávám
    event.target.classList.toggle("selected");
  }

  const tlacitka = document.querySelectorAll(".tlacitka button");

  tlacitka.forEach( function(tlacitko) {
    tlacitko.addEventListener("click", vyberTlacitko);
  });
  ```

* tlačítko s počítadlem kliknutí:
  ```javascript
  const pocitadlo = document.querySelector(".pocitadlo");

  let pocet = 0; // je dulezite promennou zalozit mimo tu funkci - kdyby byla uvnitr funkce, tak by to znamenalo, ze pokazde, kdyz se ta funkce spusti, tak se promenna znovu zalozi a zacne na 0

  pocitadlo.addEventListener("click", function() {

    pocet++;
    pocitadlo.textContent = "Pocet kliknuti: " + pocet;
  })
  ```
  * můžu chtít například, aby to počítadlo napočítalo třeba jen do 5 a pak přestalo fungovat:
  ```javascript
  const pocitadlo = document.querySelector(".pocitadlo");

  let pocet = 0; // je dulezite promennou zalozit mimo tu funkci - kdyby byla uvnitr funkce, tak by to znamenalo, ze pokazde, kdyz se ta funkce spusti, tak se promenna znovu zalozi a zacne na 0

  pocitadlo.addEventListener("click", function() {

    if (pocet < 5) { // akoratze pokud to udelam takhle, tak se ta funkce bude spoustet dal pri kliknuti, jen se nebude nacitat to cislo
    pocet++;
    pocitadlo.textContent = "Pocet kliknuti: " + pocet;
    }
  })
  ```
* **můžu chtít `event listener` i odebrat** - musím do toho uvést na jakou událost se ten event listener pridal a jaka funkce se volala:

  ```javascript
  tlacitko.removeEventListener("click", priKliknuti);
  ```
  * tohle musím zadat proto, že tam může být víc různých `event listener`ů na stejném prvku, ale s jinou funkcí apod
  * musí být identicky zadaný název eventu i název funkce, která se provádí, jinak se `eventListener` neodebere
  * tohle může být problém, kdybych používal anonymní funkce - nemám jméno funkce, podle které bych mohl `eventListener` odebrat
  * **pokud bych zkusil to odebrat zadáním anonymní funkce místo názvu funkce, tak se ta anonymní funkce vždycky jen znovu založí, neodebere se!**
  * jinými slovy, pokud někam přidám `eventListener` a bude obsahovat anonymní funkci, tak už ho nikdy neodeberu - nemám, jak
  * **to je proto, že anonymní funkce je funkce, pro kterou neexistuje žádné jméno, na které bych se mohl později odkázat - což je v pohodě, dokud se na ni v budoucnu nebudu už nikdy chtít odkazovat**

* takže u toho počítadla bych to spíš potřeboval udělat takhle:
```javascript
const pocitadlo = document.querySelector(".pocitadlo");

let pocet = 0;

function zvetsiPocet() { //zalozim funkci, ale hned ji priradim jmeno
  console.log("klik");

  pocet++;
  pocitadlo.textContent = "Pocet kliknuti: " + pocet;
  
  if (pocet === 5) {
    pocitadlo.removeEventListener("click", zvetsiPocet); // diky tomuhle uz se nebude funkce vubec provadet, protoze jsem odebral eventListener
  }
}

pocitadlo.addEventListener("click", zvetsiPocet); // pridavam eventListener, ale pouzivam jmeno funkce, co se ma provest, mam moznost pak eventListenera zase odebrat
```


