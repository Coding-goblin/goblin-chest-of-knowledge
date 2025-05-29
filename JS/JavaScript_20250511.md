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

* [Event index](https://developer.mozilla.org/en-US/docs/Web/Events)
* 