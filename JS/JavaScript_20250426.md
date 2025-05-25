# JavaScript

* javascript vkládám až na konec, před ``</body>``, aby to nezačalo spouštět skript s html elementy, co se ještě nenačetly

  ```javascript
  function pozdrav () { // definice funkce
    console.log ("Ahojda jahoda");
  }

  pozdrav (); // zavolani funkce
  ```

* dokud nepotřebuju proměnnou měnit, preferuju použití ``const`` oproti ``let``

* když je proměnná zároveň funkce, tak ji můžu zavolat a spustit tím ten kód funkce:

  ```javascript
  const zdravice = function () { // anonymni funkce
    console.log ("cus bus");
  }

  const reknicau = zdravice; // priradime anonymni funkci do promenne

  zdravice (); // funkci spustim, pokud tam dam zavorky
  ```

* můžu chtít, aby se nějaký kód spustil až za nějakou dobu (jinak se spouští postupně odshora dolů), čas se uvádí v milisekundách (tisícinách sekundy)

```javascript
setTimeout ( pozdrav, 3000); // zavolani funkce po 3s, pozdrav je nazev funkce BEZ zavorek
```
  * tohle spustí asynchronní odpočítávadlo a pak se to spustí mimo to pořadí jiných funkcí, a potom zase pokračuje ostatní kód

* nebo můžu nastavit ``setInterval`` a spustit fuknci opakovaně:

  ```javascript
    setInterval (pozdrav, 1000); // zavolani funkce pozdrav každou 1s


    setInterval ( // tohle je uplne stejne jako to vyse - pokud tu funkci budu pouzivat jen jednou, nema cenu ji davat jmeno - muzu ji tam vypsat rovnou
    function () {
      console.log ("tak");
    },
    1000
  );
  ```

* nebo můžu nastavit, aby se načítalo počítadlo:

```javascript
setInterval ( // tohle je uplne stejne jako to vyse - pokud tu funkci budu pouzivat jen jednou, nema cenu ji davat jmeno - muzu ji tam vypsat rovnou
  function () {

    pocitadlo = pocitadlo + 1; // zvysim pocitadlo o 1
    pocitadlo += 1; // zvysim pocitadlo o 1
    pocitadlo++; // zvysim pocitadlo o 1

    console.log (pocitadlo); // vypis pocitadla do konzole
  },
  1000
);
```

* můžu nastavit, aby se počítadlo zastavilo na určitém čísle:

```javascript
dopsat si sem kod
```



```javascript
nechat si dovysvetlit toto:
function sayHello (count) {

  let counter = 0;

  const intervalId = setInterval (
    function() {
      counter += 1;
      console.log ("Hello", counter);

      if (counter === count) {
        clearInterval (intervalId);
      }
    }, 
    1000
  );
}

sayHello (10);
```

* scope = obor platnosti
* global scope = globální prostor
* local scope = lokální prostor

```javascript

// global scope -> globální proměnná = proměnná, která je dostupná všude v kódu
const jmeno = "Alena";

// local scope -> lokální proměnná = proměnná, která je dostupná pouze v rámci funkce
function pozdrav() {
    const jmeno = "Petr"; // lokální proměnná
    console.log("Ahoj " + jmeno); // Ahoj Petr
}


// pokud bych potom mimo tu funkci použil proměnnou jméno, tak by se mi vytisklo Alena, protože globální proměnná nejde přepsat mimo funkci

//uvnitř funkce se proměnná tzv. zastíní, ale ne přepíše

// jakmile funkce skončí, tak se proměnná jméno z funkce smaže a zůstane ta globální proměnná
```
* lokální prostory se můžou překrývat - **nested scopes**:
prirovnani v materialech - nested scopes

* **hoisting**
  * javascript se vzdycky nejdriv podiva na kod a zjisti si definice funkci a promennych, a pak je teprve spousti. Takze pokud bychom volali funkci pred jejim definovanim, tak to bude fungovat. Ale pokud bychom volali promenou pred jejim definovanim, tak to fungovat nebude.

```javascript
pozdrav();

function pozdrav2() {
    console.log("Ahoj"); // Ahoj Alena
}
```
* kvuli tomuhle je dulezite, zda nadefinujeme funkci jako promennou nebo jako funkci

* doporucuje se na hoisting zapomenout a vzdycky definovat funkce pred jejich volanim, aby to bylo prehlednejsi a aby se predeslo chybam

* z lokálního prostoru mám přístup ke globálním proměnným, ale nefunguje to naopak!


* ```callOtherFunction``` - nechat si dovysvetlit

```javascript
function callOtherFunction(nameOfFunction) {
  console.log("Hi, I am a function named 'callOtherFunction' and call a function that someone threw to me as a parameter");

  const randomNumber1 = Math.random() * 20;
  const randomNumber2 = Math.random() * 10;
  nameOfFunction(randomNumber1, randomNumber2);
}

callOtherFunction(function (a, b) {
  console.log("First number:", a);
  console.log("Second number", b);
  console.log("Result", a + b);
});
```

## Arrays

* česky pole
* mohou uchovat více různých hodnot
* ``array.pop ()`` - odebere posledni hodnotu z pole
* ``array.shift()`` - odebere prvni hodnotu z pole
* kdybych chtěl odebrat hodnotu a zjistit, co jsem odebral, případně dát do proměnné:
```javascript
const konec = jmena.pop();
console.log (odebral jsem hodnotu, konec);
```

* přidání na konec pole:

  ```javascript
  array.push ("Eva");
  console.log (array);
  ```

* přidání na začátek pole:
  
  ```javascript
  array.unshift ("Eva");
  console.log (array);
  ```

* otočení pořadí
  ```javascript
  array.reverse();
  console.log (array);
  ```

* hledání indexu hodnoty
  ```javascript
  console.log (jmena.indexOf("Karel")); // hledani od zacatku

  console.log (jmena.lastIndexOf("Karel")); // hledani od konce?
  ```
* nahrazení nebo smazání prvku uprostřed pole

  ```javascript
  jmena.splice(2,1) // (od jakeho indexu, kolik) chci smazat
  console.log (jmena);

  jmena.splice(2,0, "Katka", "Milan", "Eliska") // (od jakeho indexu, kolik) chci nahradit
  console.log (jmena);
  ```

* seřazení pole

```javascript
  array.sort(); // seradi se to abecedne
  console.log (array);
```
  * ale podle abecedy to srovná i čísla (!)
  * takže pak musím přidat funkci, kterou pak použiju jako parametr ve funkci ``sort``, aby mi zajistila, že se čísla seřadí správně, nečísla to ignoruje??
  
```javascript

```  

* procházení pole
  