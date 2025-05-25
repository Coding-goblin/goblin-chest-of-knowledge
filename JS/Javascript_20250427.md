# Filter

dopsat si sem vyklad z druheho okna s kodem


## Objects

```javascript
const osoba = {
  jmeno: 'Alena',
  prijmeni: 'Novakova',
  vek: 27,
  ulice: 'Pod dubem 1',
  mesto: 'Mestecko nad Rekou',
  psc: '123 45',
  maRidicak: true,
}
```

* můžu dát objekt do objektu - například objekt adresa: muze obsahovat ulice, mesto, psc

* referencni datove typy a jak v nich funguje const
 muzu menit hodnoty const protoze nemenim tu const jako takovou, ale jen jeji obsah?

* v objektu muze byt vlastnost, jejiz hodnota je funkce = "metoda"

dodelat cviceni v Objects

* this keyword & Constructor function asi probereme na extra lekci mimo

## DOM introduction and DOM elements

* DOM je strom objektů na webu, kdykoliv se nějak změní, tak ho prohlížeč překreslí

* když chceme na webu cokoli pomocí javascriptu měnit, tak musím v tom stromu objektů najít objekt, který je třeba změnit
  * to se dělá pomocí příkazu: 

  ```javascript
  document.querySelector("h1");
  ```
  * tohle nám řekne, ve kterém objektu se ten prvek nachází
  * my si pak ten objekt uložíme do proměnné

```javascript
const nadpis = document.querySelector("h1"); // hledam html element h1

nadpis.textContent = "Ahoj"; // měním obsah objektu
```

```javascript
const odstavec = document.querySelector(".odstavec"); // hledam kde je objekt s tridou .odstavec??

odstavec.textContent = "Beží liška k táboru"; // měním textový obsah objektu
```

```javascript
const citace = document.querySelector("#citace"); // hledam kde je objekt s id 

citace.textContent = "To je asi jedno"; // měním textový obsah objektu
```

* takhle ale můžu měnit i třeba CSS vlastnosti (oddělenou tečkou)

```javascript
nadpis.style.color = "red"; 
nadpis.style.border = "2px solid blue";
nadpis.style.padding = 
nadpis.style.backgroundColor = 
nadpis.style.fontFamily = 
```

* ale nejde tam napsat vlastnosti s pomlčkami jako ```background-color```, protože pomlčku bere javascript jako mínus
  * v javascriptu se to píše dohromady, první písmeno slova za pomlčkou bude velké
    * tomuhle zápisu se říká camelCase a používá se k nazývání víceslovných věcí v javascriptu i v jiných situacích
    * zápisu s podtržítkem se říká snake_case

* existuje i spousta dalších metod, ale všechny jde nahradit tím ;```querySelector```em, v materiálech jsou uvedené
* 
  ```javascript
  document.getElementsByTagName('h1')
  document.getElementsByClassName('odstavec')
  document.getElementById('citace')
  ```

* querySelector vždycky vybere jen jeden objekt, první, který odpovídá selectoru
* pokud chci vybrat všechny věci, které tomu selectoru odpovídají, tak musím použít ``querySelectorAll``, ale nemůžu jim najednou všem nastavit vlastnost, pokud se jedná o pole - pak musím použít například ```forEach```
