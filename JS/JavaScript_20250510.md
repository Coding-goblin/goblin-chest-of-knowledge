# Objects in ES5

* rozdíl mezi primitivními datovými typy a referenčními datovými typy
  * stack vs. heap
  * i když je objekt osoba ```const```, tak můžu měnit jeho obsah, ale nemůžu nastavit např. ```osoba = jmeno```

* u objektů není garantované pořadí jejich vlastností (properties), a je vlastně nedůležité - hlavní je, že přes jejich názvy k nim můžeme přistupovat

* doporučuje psát čárku za každou vlastností v objektu, i tou poslední, abychom na ni případně nezapomněli - na konci ničemu nevadí, ale kdekoliv jinde to bez čárky nebude fungovat
* 
* obvykle se to dělá tak, že nejdřív se nastavují vlastnosti, až po nich metody (funkce, které patří nějakému konkrétnímu objektu)

* zkrácený zápis metody:
  ``` javascript
  pozdrav(jmeno) {
    console.log("ahoj", "jsem", jmeno);
  }
  // je jen zkracenina tohohle:
  pozdrav: function () {
    console.log("ahoj", "jsem", jmeno);
  }
  ```

* můžu vymazat vlastnost objektu:
  ```javascript
  delete osoba.vek; // smazani property vek
  ```
  * ale neděje se tak často, že bych to potřeboval


* z objektu můžu property vypsat i takhle:
    ```javascript
    const vlastnost = prijmeni
    console.log (osoba ['prijmeni'])
    ```
  * když zavolám property přes tečku, funguje to jedině pokud přesně napíšu její název (key):
  ```javascript
  osoba.prijmeni  // tohle ukaze "Novak"

  osoba['prijmeni']  // taky ukaze "Novak"
  ```

  * takhle můžu zavolat property, jejíž jméno(key) mám ještě navíc uložené v proměnné

  * např. nevím, jak se ta vlastnost bude jmenovat, protože je to změnitelné třeba majitelem eshopu nebo uživatelem

  * například: na webu si může uživatel zvolit, které info chce vidět - "jmeno" nebo "prijmeni"
    * jejich volba se potom uloží do variable

  ```javascript
  let userChoice = prompt("What info do you want? jmeno or prijmeni?");
  console.log(osoba[userChoice]);

  // pokud uzivatel napise 'jmeno', console.log se zmeni na:
  console.log(osoba['jmeno']); // Jan

  // tohle by nefungovalo, kdyby se to zapsalo přes tečku:
  console.log(osoba.userChoice);  // ❌ javascript by hledal key doslova nazvany "userChoice"
  ```

* můžu odkazovat přes ``this.jmeno`` uvnitř objektu, čímž odkazuju na vlastnosti objektu uvnitř jeho samotného:
  ``` javascript
  pozdrav(jmeno) {
    console.log("ahoj", "jsem", this.jmeno);
  }
  // versus
  pozdrav: function () {
    console.log("ahoj", "jsem", jmeno); // tohle může teoreticky odkázat na stejně nazvanou variable mimo ten objekt
  }
  ```
* object literal - je to objekt, kterému jsem doslova určil všechny vlastnosti a hodnoty:
  ```javascript
  const osoba = {
  jmeno: 'Alena', // property = key: jmeno, value: Alena
  prijmeni: 'Novakova',
  vek: 27, 
  adresa: {
    ulice: 'Pod dubem 1',
    mesto: 'Mestecko nad Rekou',
    psc: '123 45',
  },
  maRidicak: true,
  znamky: [1, 2, 3, 4, 5],
  kamaradi: ['Eva', "Matěj", "Ivan"],
  }
  ```
  * ale dává smysl, že bych mohl chtít vytvořit i další "osoby", ale s jinými vlastnostmi, které třeba používají stejnou metodu (funkci) a nemusel jsem tu metodu pokaždé vypisovat ke každému z nich

  * mám víc objektů a napíšu obecnou funkci:
  ```javascript
  const sestra = {
    jmeno: "Alena",
  }

    const bratr = {
    jmeno: "Adam",
  }

  function ahoj() {
    console.log("Ahoj");
  }

  sestra.pozdrav = ahoj; // funkci ahoj pridam jako novou metodu do objektu sestra, pod nazvem "pozdrav"
  sestra.pozdrav() // vytiskne se ahoj
  ```
  * můžu ale použít v té obecné funkci ``this.`` a tím ta funkce vždycky vytáhne data z toho objektu, na který je zrovna aplikovaná:
  ```javascript
  function ahoj() {
  console.log("Ahoj, jsem", this.jmeno);
  }
  sestra.pozdrav() // vytiskne se se jmenem sestry
  bratr.pozdrav() // vytiskne se se jmenem bratra
  ```
  * kdybych tu funkci zavolal a nedal před ní název objektu, tak mi to hodí ```undefined```, protože to ```this.jmeno``` nemá, hodnotu, na kterou by mohla odkázat 
  * díky tomu můžu mít jednu univerzální funkci, kterou jsem přidal na dva různé objekty a bude odkazovat vždy na vlastnosti toho objektu, na který je přidána
  * chci metodu psát vždycky jen jednou, bývají to i složitější funkce

### The window object

* In JavaScript, the window object is the main object in the browser. It represents the browser window or tab where your web page is displayed.

* You can think of it as the "boss object" that holds everything related to the web page and the browser environment.

* what the window object does:

  1. **Global Scope**
  * Every global variable or function you create is actually a **property** of the window object.

  ```javascript
  var name = "Alice";
  console.log(window.name); // "Alice"
  ```
  2. **Holds Built-in Features**
  Common tools like:

  * `alert()` → shows popup messages

  * `prompt()` → asks user for input

  * `setTimeout()` and `setInterval()` → run code later or repeatedly

  * `console` → used to log messages

  * `document` → used to access the HTML on the page

  ...are all part of the window object.

  * Example:

  ```javascript
  window.alert("Hello!"); // same as just alert("Hello!");
  ```
  3. **Controls the Browser Window**
  * It gives you properties and methods to control or get info about the browser, like:

  * `window.innerWidth` → width of the browser

  * `window.location` → the URL of the page

  * `window.scrollTo()` → scrolls the page

* reálně ale těch zásadních funkcí je tolik, že před ně nemusíme psát `window.` - například nemusíme psát `window.setTimeout()`, ale stačí `setTimeout()`

* **když použijeme `this.` mimo jakoukoliv funkci, tak to odkáže na objekt window**
* když použijeme  `this.` ve funkci a neodkážeme na objekt, tak to je undefined
* když použijeme  `this.` ve funkci a odkážeme na objekt, tak to použije vlastnost objektu, na který odkazujeme

* pointa nastavování metod co nejobecněji je ta, že chceme, aby třeba metoda sama něco nastavila do toho objektu, a nemuseli jsme to nastavovat doslova ručně - například: přidej(push) datum poslední prohlídky auta a podle toho nastav do jiné vlastnosti toho auta, zda má nebo nemá platnou technickou kontrolu

* 

## Constructor function

* je to taková šablona pro to, jak bude vypadat objekt
* pak pomocí slova `new` zakládám nový objekt podle této šablony a dosazuju do něj parametry, které si zvolím

```javascript
function User () { // typ objektu, ktery budu pozdeji vytvaret, proto se to pise s velkym pismenem na zacatku
  this.name = 'Alena';
  this.type = 'basic';
}

const user1 = new User(); // novy objekt toho typu; slovo new zalozi novy objekt, ve kterem se nastavi vlastnosti z User()

const user2 = new User(); // dalsi user, ale se stejnymi vlastnostmi
```

  * `new` založí ten nový objekt a `this` ukazuje na ten nový prázdný objekt, takže mi to nehodí `undefined`

* bylo by ideální, abych mohl nějak udělat, že třeba každý nový user bude mít svoje jméno:
  ```javascript
  function User (username) { // tady mam parametr, kterej se pak dosadi i do te funkce
    this.name = username;
    this.type = 'basic';
  }

  const user1 = new User(Alena);
  const user2 = new User(Petr);
  ```
  
* můžu tomu nastavit i metodu:
  ```javascript
  function User (username) { // tady mam parametr, kterej se pak dosadi i do te funkce
    this.name = username;
    this.type = 'basic';
    this.sayHello = function() {
      console.log("Hello, I am", this.name);
    }
  }

  const user1 = new User(Alena);
  const user2 = new User(Petr);

  user1.sayHello();
  user2.sayHello();
  ```
* tenhle způsob má ale nevýhodu, že když vytvořím takových objektů třeba tisíc, tak někde v paměti se mi 1000x vytvoří každá jejich metoda (samostatně,pro každého jedna)
  * tomuhle bychom chtěli předejít
* v konzoli u těch nových objektů uvidím i položku `[[Prototype]]`, což je objekt, na základě něhož ten který objekt byl vytvořen
* je to takový řetězec - na začátku je obecný javascriptový objekt, na základě kterého jsem vytvořil svoji šablonu `User`- na základě které je vytvořen konkrétní objekt

* můžeme tu funkci přidat ne do naší šablony, ale do toho prototypu, obecného javascriptového objektu

* v JavaScriptu to funguje tak, že když zavolám metodu nějakého objektu, tak se ověří, zda v tom objektu existuje, a když ne, ověří se, zda funkce existuje v jeho prototypu, a když není ani tam, tak se zkusí, jestli existuje u prototypu toho prototypu atd.

* tím, že tu funkci přiřadím až k tomu prototypu a ne k tomu konkrétnímu objektu, tak ta funkce nebud zbytečně vytvořená tisíckrát, ale jen jednou, a přes tu dědičnost se k ní dopracuje každý z těch objektů (?)

* můžu tedy tu funkci založit jako novou vlastnost (metodu) prototypu:

  ```javascript
  User.prototype.sayHello = function() { // tady uz to neni v tom objektu, ale je to v prototypu
    console.log("Hello, I am", this.name);
  }
  ```
* tomu prototypu můžu klidně nastavit i vlastnost, třeba aby všechny objekty byly `this.type = 'basic';`
  
* ale pak mi nic nebrání konkrétnímu objektu tu vlastnost přenastavit na něco jiného - třeba `user1.type = "admin";`

* pak to javascript vždycky hodnotí od objektu k prototypu - takže když objekt má vlastnost nastavenou, vezme ji z objektu samotného, a když nemá vlastnost nastavenou, najde ji v jeho prototypu atd.

* většinou se do prototypu spíš přesouvaji metody, vlastnosti tolik ne

  ```javascript
  function Car (brand, model, color) {
    this.brand = brand;
    this.model = model;
    this.color = color;
  }

  const mojeAuto = new Car('Toyota', 'Corolla', 'grey');

  Car.prototype.printInfo = function () {
  console.log("Auto je znacky", this.brand, "model", this.model, "a barvy", this.color);
  }
  ```

* takhle si předpřipravím pomocí constructor function šablony objektů a přiřadím metody jejich prototypům - pak můžu jednoduše zavoláním metody dělat spoustu věcí

## Document Object Model (DOM)

* všechny html značky si JavaScript převede na objekty

* vytvoří si strom prvků - každý z těch prvků má vlastnosti, které můžu měnit

* pokud se vlastnosti nějakého prvku změní, tak prohlížeč detekuje změnu a strom přepíše

* každý objekt má i vlastnosti včetně CSS vlastností

* prohlížeč si načte náš kód, vytvoří si DOM - strom prvků, náš zdrojový kód zapomene, a pak už pracuje jenom s DOM - to, co vidím v Elements ve vývojářských nástrojích není ten můj původní kód, ale nějaký kód, co si ten prohlížeč zpětně vytvořil sám na základě toho sebou vytvořeného stromu prvků

* abychom mohli nastavit vlastnosti, tak musíme ten prvek v DOM najít:
  ```javascript
  const nadpis = document.querySelector("h1"); // s tim vysledkem querySelectoru chci dal pracovat, proto si ho ulozim do promenne

  nadpis.textContent = "Ahoj"; // zmenim text nadpisu h1 na "Ahoj"
  nadpis.textContent = "Ahoj <em>lidi</em>!"; // tohle neudela kolem lidi kurzivu, ale zustane tam vse doslova vcetne toho kodu tagu jako string, ne jako kod - tohle je bezpecnostni opatreni, aby treba nejaky uzivatel nemohl pridat komentar obsahujici skodlivy kod a uskodit tak nekomu, kdyz ten komentar otevre na webu - mohl by tam klidne nekdo postovat znacku <script>, a pripojit externi javascript odnekud na odposlech hesel apod.
  ```
  * nekdy tam naopak chci vložit html kód a potřebuju, aby se interpretoval (protože to dělám přímo já, nezobrazuju text někoho jiného), potom to udělám takhle:
 ```javascript
  nadpis.innerHTML = "Ahoj <em>lidi</em>!"; // tohle zinterpretuje HTML značky a "lidi" budou kurzivou, kdybych tam měl javascript, tak se ten javascript spustí
  ```

  * můžeme chtít změnit CSS vlastnosti - to je pak ještě zabalené ve vlastnosti s názvem ``style``:
  ```javascript
  nadpis.style.color = "blue";
  nadpis.style.backgroundColor = "green"; // u dvojslovnych CSS vlastností nejde použit pomlčku, to je v javascriptu mínus. Takže píšu druhé slovo s velkým počátečním písmenem
  nadpis.style.fontFamily = "Arial";
  ```
  * tímhle ale určitě nechceme nahrazovat stylování webů, tohle použijeme třeba když budeme dělat interaktivní prvek a budeme chtít, aby po kliknutí se třeba změnila barva prvku apod.
  * když změním nebo přidám CSS vlastnost prvku, tak se to přidá jako atribut `style` k tomu HTML elementu, neměním nijak CSS soubor:

  ```html
  <div class="edge" style="background-image: url("img/edge.jpg");"></div>
  ```
  
  * vložím třeba obrázek, dám mu třídu .obrazek1 (a priradim mu proměnnou obrazek) - pak ho můžu vybrat a měnit jeho atributy, když je napíšu za tu tečku:

  ```javascript
  const obrazek = document.querySelector('.obrazek1');
  obrazek.height = 200; // i kdyby ten obrazek neměl nějaký atribut nastavený v html, tak já ho můžu zadat a tím ho nastavit
  obrazek.src = 'images/obelix.png'; // takhle můžu i změnit zdroj obrázku - třeba v rámci nějaké interakce to chci udělat
  obrazek.alt = "Obelix";
  ```
  * můžu změnit, na jakou stránku vede odkaz:
  ```javascript
  const odkaz = document.querySelector("a");
  odkaz.href = "https://seznam.cz"
  odkaz.textContent = "Juchuuuu na Seznam";
  ```

* existují speciální atributy - ``data atributes``, které můžu přidat na jakýkoliv HTML element, nějak si ho nazvat a uložit si do něj jakékoliv údaje, které u té značky chci mít uložené
  
  * tohle se mi hodí, když pak chci s elementy pracovat v javascriptu - třeba chci zobrazit nápovědu při najetí kurzorem na tlačítko:
  ```html
    <button
      data-napoveda="Otevre existujici soubor"
      data-poradi="42"
    >Otevrit</button>
  ```
  * pak můžu v javascriptu nastavit, aby se tenhle text z ``data-napoveda`` zobrazil:
  ```javascript
  const tlacitko = document.querySelector("button");
  console.log(tlacitko.dataset.napoveda);
  console.log(tlacitko.dataset.poradi);
  ```
  * data atribute přidám takhle:
  ```javascript
  const tlacitko = document.querySelector("button");
  console.log(tlacitko.dataset.napoveda);
  ```
  * můžu si takhle přidávat k různým prvkům ``data-id``, a podle toho pak databáze bude vědět,jaká data změnit
    * ``<button data-id="42">Delete</button>``
      * In JavaScript, when this button is clicked, you can grab the data-id (42) and send it to your backend.
* `querySelector` najde pouze první z elementů, který odpovídá tomu selectoru
  * pokud bych chtěl vybrat všechny prvky, které odpovídají selectoru, použiju ``querySelectorAll``
  ```javascript
  const odstavce = document.querySelectorAll("p"); // tohle mi do proměnné neuloží konkrétně ten prvek, ale tzv. "NodeList" - pole elementů

  const odstavce = document.querySelectorAll("p:nth-child(2)"); // takhle můžu vybrat třeba jen druhý odstavec
  ```
* `NodeList` sice není pole, ale jako pole (array) se chová v tom, že sice nemá metody jako find, push apod., ale má metodu `forEach` a můžu odkazovat na prvky v něm pomocí indexů
* z `NodeList` můžu vytvořit skutečné pole takhle a pak s ním pracovat jako s polem:
  ```javascript
  Array.from(nodelist)
  ```

* když budeme chtít na všechny ty prvky něco nastavit:
```javascript
  odstavce.forEach(function(odstavec) {
    odstavec.style.border = '5px solid black';
    odstavec.textContent += "ahojda jahoda" // tohle prida na konec odstavce text
  })// nemuzu nastavit neco primo na ten NodeList, ale můžu použít forEach a nastavit takhle jakoukoliv vlastnost tech prvku
```
* tím, že můžu na ty prvky odkazovat pomocí indexů, tak můžu vybrat třeba třetí prvek z toho pole s proměnnou odstavce a nastavit něco jen jemu:
  ```javascript
  odstavce[2].style.borderColor = "red";
  ```

* zároveň co se týče ochrany před škodlivým kódem vloženým uživateli na web: než se obsah vložený třeba uživatelem do komentáře nahraje na server, tak to stejně prochází filtrem, aby se tam nic nedostalo = například může někdo nahrát kód, který smaže všechna data o uživatelích v databázi - takže filtr předtím, než to jde na server, a zároveň taky filtr než to jde zpět na web, aby tam někdo nemohl nahrát škodlivý kód

* taky můžu hledat element dle jeho id za použití `getElementById`:
  ```javascript
  const home1 = document.querySelector("#home");
  console.log(home1);

  const home2 = document.getElementById("home");
  console.log(home2);
  // tyhle dve veci delaji to same
  ```
    * ale doporučuje se používat spíš `query.selector`, to je modernější

* po vytvoření proměnné, kterou jsem vybral nějaký element můžu pomocí `querySelector` dál hledat jen v rámci toho elementu:
  ```javascript
  const firstArticle = document.querySelector("article.first");
  console.log(firstArticle);

  const headings = firstArticle.querySelectorAll("h2");// tady hledam uz jen v elementech, ktere spadaji pod ten element article.first
  console.log(headings.length);
  ```

* `querySelector` můžu za sebe řetězit přes tečku, ale reálně to můžu dělat i jednoduššeji:
  ```javascript
  const links = document.querySelector(".links").querySelectorAll("li"); // rikam mu: "najdi element s tridou links a pak v nem najdi vsechny elementy li"

  const links = document.querySelectorAll(".links li"); // tohle je přímočařejší, nepotřebuju  to vypisovat přes tu tečku
  ```