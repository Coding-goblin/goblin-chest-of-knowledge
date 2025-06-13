# Forms

* potřebujeme přistupovat k hodnotám, které jsou napsané ve formulářových polích

* pokud budeme k hodnotám přistupovat přes Javascript, tak nepotřebujeme přiřazovat prvkům formuláře vlastnost `name` v HTML, protože ta se používá pro přiřazení hodnoty v případě odesílání dat z formuláře někam

* k hodnotám většiny polí můžeme přistupovat pomocí `pole formuláře.value`
  * jsou i výjimky, ale většina jde takhle

* ideálně by měly být formulářová pole uzavřená ve značce `<form></form>`
  * ta značka má dva atributy:
    * `action` = kam se data odesílají (adresa serveru)
    * `method` = jakou metodou se odesílají

  * i když nemám ty atributy uvedené, tak se formulář uzavřený do značky `<form>` odešle - akorát se data odešlou na stejnou adresu stránky, na které zrovna formulář je

  * kliknutí na tlačítko typu submit nebo stisknutí klávest ENTER po vyplnění políčka data odešle podle atributů té značky `<form>` a přiřadí je podle `name`

  * my nechceme ta data nikam odesílat, ale chceme je zpracovat - tyhle atributy nepotřebuju

  * když je formulář uvnitř té značky, tak se mi po kliknutí na submit refreshuje stránka a console logy mi tam jen probliknou - můžu si v nastavení konzole zaškrtnout Preserve log, pak mi tam zůstanou
    * takže jakoby můžeme něco přes console.log udělat, ale po odeslání o data přicházíme kvůli refreshi
      * tomuhle potřebujeme nějak zabránit
      * **ideálně nebudu v Javascriptu reagovat na událost `"click"` na odesílacím tlačítku, nýbrž na událost `"submit"`, která je na tom formuláři jako takovém**
        * protože to odeslání nemusí být způsobené kliknutím na tlačítko, ale i právě stisknutím ENTER nebo třeba pomocí hlasové čtečky zadáním příkazu, že chci odeslat formulář
  ```javascript
  const formular = document.querySelector("form");
  const jmeno = document.querySelector("#jmeno");

  function zpracovatFormular() {
    console.log("odeslani formulare");
    console.log(jmeno.value);
  }

  formular.addEventListener("submit", zpracovatFormular);
  ```
  * zároveň chceme zabránit výchozí akci prohlížeče - odeslání dat a refreshi po odeslání dat
    * tomuhle můžu zabránit pomocí `event.preventDefault();`
  ```javascript
  const formular = document.querySelector("form");
  const jmeno = document.querySelector("#jmeno");

  function zpracovatFormular(event) {
    event.preventDefault();
    console.log("odeslani formulare");
    console.log(jmeno.value);
  }

  formular.addEventListener("submit", zpracovatFormular);
  ```

* pokud mám například drop-down selector ve formuláři a mám u něj attribute `value` uvedený, tak se jako hodnota vypíše to, jinak se jako hodnota vypíše obsah pole
  * přes `.value` můžu hodnotu i nastavit

* trošku jinak to funguje, když mám jako input type `checkbox`:
  * u checkboxů není hodnota `value` ta aktuální hodnota zaškrtnutí, ale jen určuje, jestli se ten checkbox odešle na server nebo ne - pokud je zaškrtnutý, tak se pošle na server například "souhlas = on", pokud zaškrtnutý není, tak se neodešle na server nic
    * můžu ten atribut u checkboxu změnit třeba na `value="ano"`, pak to `"ano"` nahradí `"on"`

  * attribute, který potřebuju volat v případě checkboxu je `checked`, zjistím ho přes `.checked` - dá mi to boolean (true nebo false)

* někdy chceme data z formuláře nejen zpracovat, ale v javascriptu reagovat na to, že se to pole nějak mění
  * na jednotlivé prvky můžeme přidat události:

  ```javascript
  souhlas.addEventListener("change", function() {
    console.log("Zmenil se checkbox na:", souhlas.checked); // tohle mi jen v konzoli ukaze, zda je checkbox zaskrtnuty - true, nebo ne - false, reaguje to na zaskrtnuti checkboxu, ne na tlacitko submit
  })
  ```
  * v závislosti na zaškrtnutí checkboxu můžu chtít například nějaký element skrývat nebo odkrývat
  * můžu třeba chtít, aby formulář nešel odeslat, dokud není zaškrtnutý nějaký checkbox
    * na prvek `<checkbox>` můžu přidat attribute `disabled` - pak v javascriptu nastavím, aby když `checked = true`, tak `disabled = false`
  ```javascript
  souhlas.addEventListener("change", function() {
    console.log("Zmenil se checkbox na:", souhlas.checked); // tohle mi jen v konzoli ukaze, zda je checkbox zaskrtnuty - true, nebo ne - false, reaguje to na zaskrtnuti checkboxu, ne na tlacitko submit

    odeslat.disabled = !souhlas.checked; // kdyz neni souhlas checked, pak je odeslat disabled - zjednodussene napsano

    // rozepsane je to takhle:
    if (souhlas.checked === true) {
    odeslat.disabled = false;
    } else {
      odeslat.disabled = true;
    }
      })
  ```
    * nebo můžu třeba skrývat a odkrývat `<div>` s dalšími políčky třeba když se liší fakturovací a doručovací adresa pomocí `display: none;` apod.

* na událost `change` můžu reagovat například když se změní počet hostů v hotelu, že se mi na základě toho přepočítá cena apod.

* událost `change` funguje i na `input` elementy (do kterých se dá psát), ale zafunguje to až při tom, když odejdu z toho pole, které se změnilo
  * tenhle level interaktivity většinou stačí - můžu reagovat na to, že uživatel napsal třeba nesprávně PSČ - poté, co odklikne pryč z toho input pole

* pokud bych chtěl udělat reakci na každé písmenko, co uživatel napíše, můžu reagovat na události:
  * `keypress` zmáčknutí a následné puštění klávesy - tohle je vždycky o jednu reakci pozadu
  * `keydown` zmáčknutí klávesy
  * `keyup` puštění klávesy - na to se reaguje nejčastěji, nejvíc se to používá - reaguje to bez zpoždění

  * například na vypsání malým písmem do `<p><small></small></p>`, kolik už mám napsaných znaků:
  ```javascript
  jmeno.addEventListener ("keyup", function() {
    console.log("Jmeno se zmenilo na:", jmeno.value);

    document.querySelector("p small").textContent = "Váš text má " + jmeno.value.lenght + " znaků." // nemusím ten querySelector mít vždycky v proměnné, ale je to lepší
  })
  ```

* mohl bych chtít sám javascriptem vyvolat událost `submit`
  * pomocí `formular.submit();`

* můžu vrátit formulář do výchozího stavu
  * pomocí `formular.reset();`
  * můžu na webu mít třeba tlačítko "Začít znovu", které to resetuje

* basic kontrola správného vyplnění formuláře:
```javascript
  const email = document.querySelector("#email");
  const name = document.querySelector("#name");
  const surname = document.querySelector("#surname");
  const pass1 = document.querySelector("#pass1");
  const pass2 = document.querySelector("#pass2");
  const agree = document.querySelector("#agree");

  const form = document.querySelector("form");

  const successMsg = document.querySelector("#success-message");

  const errorMsg = document.querySelector("#error-message");

  form.addEventListener("submit", function(event) {
    event.preventDefault();

    errorMsg.innerHTML = ""; // chci tam smazat predchozi chybove hlasky, i kdyby tam nekde byly skryte - mohly by to treba cist ctecky displeju, nebo tak
    errorMsg.classList.add("d-none");
    successMsg.innerHTML = "";
    successMsg.classList.add("d-none");

    let error = "";

    if ( !email.value.includes("@") ){
      error += "<li>e-mail musí obsahovat zavináč @</li>"
    }

    if ( name.value.length < 2) {
      error += "<li>jméno musí mít minimálně 2 znaky</li>"
    }

    if ( surname.value.length < 2) {
    error += "<li>příjmení musí mít minimálně 2 znaky</li>"
    }

    if ( pass1.value.length < 0) {
    error += "<li>heslo nesmí být prázdné</li>"
    }

    if ( pass2.value.length < 0) {
    error += "<li>zopakované heslo nesmí být prázdné</li>"
    }

    if ( pass1.value !== pass2.value) {
    error += "<li>Heslo a zopakované heslo musí být stejné</li>"
    }

    if ( !agree.checked ) {
    error += "<li>musíte souhlasit s obchodními podmínkami</li>"
    }

    if (error) { // kdyz je neco v error
    errorMsg.innerHTML = "<p>Ve formuláři došlo k následujícím chybám:</p><ul>" + error + "</ul";
    errorMsg.classList.remove("d-none");
    } else
    successMsg.innerHTML = "<h3>Odesláno</h3><p>Formulář se úspěšně odeslal.</p>";
    successMsg.classList.remove("d-none");
  })
```

* vždycky když k hodnotám formulářových přistupuju přes `.value`, tak se mi hodnoty vrací jako string, i když to bylo původně číslo - pokud bych pak třeba s tou hodnotou chtěl dělat početní operace, musím si ho nejdřív převést na číslo, třeba takhle:

  ```javascript
  const cislo = Number(cislo.value);
  ```

* 