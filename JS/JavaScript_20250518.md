# Creating elements

* v javascriptu můžu vytvářet HTML elementy přes:
  ```javascript
  const element = document.createElement("p");
  ```
  * tohle vytvoří ten element někde bokem, existuje v paměti - ale není zařazený do DOM stromu

* abych zařadil ten element do DOM, můžu třeba použít metody, které ho přidají k již existujícímu prvku v DOM stromu:

  * `existujiciElement.apendChild(newElement)` = přidá nový element jako posledního potomka node - tohle se asi používá nejčastěji; můžu to použít i na existující element a přesunout ho jinam na stránce, například položku seznamu do jiného seznamu
  ```javascript
  const list1 = document.querySelector("#list1");
  const list2 = document.querySelector("#list2");

  document
  .querySelectorAll(".list-group-item")
  forEach(function(item)) {
    item.addEventListener("click", function() {
      if (this.parentElement === list1) { // pokud je to potomek list1
        list2.appendChild(this); // prendej ho pod list2
      } else { // pokud ne - teda pokud je to potomek list2
        list1.appendChild(this); // prendej ho pod list1
      }
    })
  }
  ```
  * `existujiciElement.insertBefore(newElement, child)` = přidá nový element před jednoho daného potomka
  * `existujiciElement.replaceChild(newElement, child)` = nahradí novým elementem daného potomka

  ```javascript
  const element = document.createElement("p");

  element.textContent = "Ahoj, ja jsem novy element";

  element.style.color = "red";

  const box = document.querySelector(".box"); // vybiram element, ke kteremu priradim novy element

  box.appendChild(element); //prirazuju novy element jako potomka
  ```
  * pokud je stránka prázdná, můžu element přidat do `body`

* můžu potřebovat i zkopírovat už existující element - pomocí `element cloning`

```javascript
const original = document.querySelector(".original");

const copy = original.cloneNode(true); // udelal jsem kopii, ale neni nikde zarazena

copy.style.color = "orange"; // prenastavuju barvu naklonovaneho elementu

const body = document.querySelector("body");

body.appendChild(copy); //pripojuju kopii elementu k body
```
* tím, že napíšu true jako argument ke cloneNode, vytvoří se tzv. deep clone = tzn., že se zduplikuje celý ten element, včetně obsahu a všech nastavení i stylů a všech vnořených elementů
  * pokud to nepoužiju
* než ten naklonovaný element připojím do DOM, můžu mu klidně přenastavit styly, aby vypadal jinak než originál

## Removing elements from DOM

* můžu taky odstraňovat elementy přes `

* abych mohl element smazat, musím nejdřív najít jeho rodiče, abych mu řekl, že má smazat svého potomka:
  ```javascript
  const rodic = document.querySelector(".rodic");
  const potomek = document.querySelector(".potomek");

  rodic.removeChild(potomek);
  ```

* mazání s využitím `this`:
  ```javascript
  const buttons = document.querySelectorAll(".deleteBtn"); // vybiram vsechna tlacitka s class .deleteBtn

  function deleteRow() {//ze kdyz na nej kliknu
    const rowToDelete = this.parentElement.parentElement; // najdu radek, ktery tlacitko obsahuje - this odkazuje na to tlacitko, na ktere jsem zrovna kliknul
    const parent = rowToDelete.parentElement // najdu rodice radku
    parent.removeChild(rowToDelete); // a reknu rodici(vetsinou tabulka), at smaze cely radek
  }

  buttons.forEach( function(button) { //pro kazdy nastavuju eventListener
    button.addEventListener("click", deleteRow);
  })
  ```
  * místo `this` bych klidně mohl použít `event.target`
  ```javascript
  button.addEventListener("click", function(event){//ze kdyz na nej kliknu
      const rowToDelete = event.target.parentElement.parentElement;
  })
  ```

* nativní vlastnost DOM API: `childElementCount` - spočítá počet potomků elementu