# Flexbox and forms

* přibylo do CSS asi před 12 lety
* umožňuje dávat elementy na webu vedle sebe místo používání floatu
* container, který obsahuje dva a více elementů:

  ``` css
    .container {
      display: flex;
    }
  ```

* dá se to vedle sebe a smrskne se to na co nejmenší šířku vedle sebe

* pokud chci mezi nimi rozestupy, můžu nastavit přes ``gap: [počet]px``

___

## Justify-content

* flexbox samotný se chová jako element, položky uvnitř něj můžeme rovnat v rámci jeho velikosti

  * ```justify-content```: výchozí hodnota je ```flex-start```

  * ```center flex-end```
  * ```space-between``` nejčastější - rozdělí rovnoměrně položky, aby první byla nalevo a poslední napravo

  * ```space-around``` rozdělí to tak, že dva díly jsou mezi položkami, jeden díl na začátku a konci

  * ```space-evenly``` rozdělí tak, že jsou mezi nimi, na začátku i konci, stejné mezery

___

## Flex-direction, align-items, stretch

* **flexbox má 2 osy, podle kterých rovná položky:** hlavní osa směřující zleva doprava + vedlejší osa, která je kolmá na ni a vede shora dolů

* směr osy můžeme změnit: ```flex-direction:```
standardně je to ```row```, ale můžu to prohodit přes ```row-reverse``` změnit na ```column``` (hlavní osa pak bude shora dolů a položky taky), ```column-reverse``` (hlavní osa bude zdola nahoru a položky taky)

* flexbox container nemá určenou výšku, ale můžu mu ji nastavit přes height:
  * pak se ale položky natáhnou podle ní, aby vyplnily celý flex - jinak je flexbox tak vysoký, jako položky
  * to roztahování je ovlivněno vlastností ```align-items:```, které určuje zarovnání vertikálně

    * základní poloha je ```stretch```: při stretch se natáhnou na výšku flexboxu a ten je tak vysoký, jako nejvyyšší položka
      * ```flex start```  = fixuje se to k horní hraně
      * ```flex-end``` = fixuje se na dolní hranu
      * ```center``` = přesně uprostřed

___

## Flex order

* prvky se ve flexboxu skládají primárně v pořadí, v jakém jsou v html kódu

* můžu měnit jejich pořadí pomocí order:
  * pokud jsem to nenastavila, mají všechny prvky hodnotu 0
    pak se řadí od nejmenší po nejvyšší hodnoty, musím tím pádem nastavit hodnotu všem, pokud chci něco měnit

  ``` css
  .item3 {order: 3}
  .item3 {order: 1}
  .item3 {order: 4}
  .item3 {order: 2}
  ```

* takhle to můžu srovnat nezávisle na pořadí položek v html, tím měním vzhled stránky

* ale v html by i tak měl být nejdůležitější věci první - například content, protože to pak čtečky čtou slepcům podle toho

___

## Flex grow & flex shrink

* pro každou položku si můžu nastavit vlastnosti:

  ``` css
    flex-grow: 0;
    flex-shrink: 1;
    flex-basis: auto;
  ```

  * zkratka:
    * `flex: 0 1 auto;`
    * `flex-grow` určuje, zda se položka smí natáhnout do zbývajícího volného prostoru a vyplnit ho, základně je to 0 a nesmí se natahovat, můžu nastavit na 1

* pokud mají hodnotu 1 dvě položky zároveň, tak všechny položky, které mají jinou hodnotu než 0, flexbox sečte hodnoty a rozdělí ten zbývající prostor podle těch hodnot ke všem položkám v poměru podle těch hodnot - takže třeba položka s 1 dostane jeden díl, položka s 4 dostane čtyři díly

* `flex-shrink` funguje podobně jako flex-grow

* `flex-basis` šířka, kterou si ideálně přeju, aby ta položka ve flexboxu měla, pokud je to možné, může mít různé jednotky, i procenta

* `auto` automaticky spočítá, pokud má určenou zároveň `width`, tak to vezme tuto šířku
* můžu tuhle vlastnost teoreticky ignorovat, vždycky je to na začátku nastaveno na auto a přebírá to `width`
* pokud jsem si nastavila u konkrétní položky flex-basis třeba na 35px a zároveň mám povoleno `shrink` a `grow` u všech položek na stejné hodnotě, tak stejně budou všechny položky stejně široké
* pokud bych mermomocí chtěla konkrétní šířku, tak musím nastavit `shrink` a `grow` na 0 a tím je zakázat

* ultimátní cíl flexboxu je, aby všechny položky byly vedle sebe a všechny se tam vešly

* pokud se tam všechny nevejdou, trčí položky ven - flexbox se podívá, které položky mají dovoleno `shrink`, sečte prostor navíc, a rozdělí ho mezi položky s povoleným ke `shrink` a o to ty položky zmenší, aby se v závěru všechny položky vešly
pokud nejde `shrink`, tak to bude trčet ven

___

## Flex wrap

* pokud chci, aby se položky zalamovaly na další řádek, tak nastavím vlastnost ``flex-wrap: wrap;``
  * nezávisle na hodnotách grow a shrink se nastaví položky podle mojí flex-basis a přebytek se zalomí na další řádek

* `flex-basis: auto;` bez zapnutého `flex-grow` způsobí, že se velikost položek nastaví podle obsahu v nich

* pomocí wrapu můžu nastavit, aby byly třeba 3 položky tak, že dvě jsou na prvním a třetí na dalším řádku

  ``` css
  .container {
    display: flex;
    flex-wrap: wrap;
  }

  .item1 {flex: 1 1 50%;
  }

  .item2 {flex: 1 1 50%;
  }

  .item3 {flex: 1 1 100%;
  }
  ```

___

## Other remarks

* `align-content`: řídí to, jak se alignuje blok všech položek ve flexboxu uvnitř prostoru flexboxu
  * skoro nikdy to v praxi nepoužiju

* ještě existuje `CSS grid`, to je alternativa flexboxu, tam je pak i možnost `align-items`

* obrázky jako přímé potomky ve flexboxu je dobré dát do svého `<div>`, aby se neroztahovaly

  * nebo nedám hodnotu `align-items: stretch;` ale třeba center nebo tak
  * ale to jenom pokud tam nechci mít v html navíc `<div>`
