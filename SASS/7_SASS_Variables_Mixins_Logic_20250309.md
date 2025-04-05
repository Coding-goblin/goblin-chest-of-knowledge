# SASS: Variables, mixins & logic

* je potřeba čím dál míň, hodně funkcí se přesouvá do nativních CSS fcí
* ale partials jsou jednou z těch unikátních věcí, které se hodně hodí a jsou důležité

___

## Variables

* názvy proměnných - každý má svoji metodu, někdy třeba:
  * `$color-bg`
  * barvy začínají "color" nebo "c" třeba

* name spaces = jména platí jen v rámci toho partial, takže musím u `@use` vždycky u proměnné zadat, odkud je ta proměnná
můžu přidat as *  a pak nemusím psát např. colors.$color-bg


* **SASS proměnné fungují trochu jinak než CSS proměnné**: 
  * ve výsledném kódu, který se zkompiluje ze SASS souborů, nebudu mít nikde definice těch jednotlivých proměnných, nýbrž se tam na všechna místa doplní ty hodnoty, který jsem proměnným určila
  * oproti tomu v CSS ty proměnné definuju a ony tam zůstanou nahoře v kódu (`:root { --color-container-bg: white;}`), plus se pak zobrazí variable a ne jeho hodnota

* naprostá většina prohlížečů už podporuje CSS proměnné, takže odpadá potřeba používat SASS proměnné - ALE:

> "While modern browsers support CSS custom properties, making them a powerful tool for dynamic and responsive design, Sass variables remain valuable for their compile-time advantages and complex styling capabilities.In real-world coding, the choice between them — or the decision to use both — depends on the specific requirements of your project.
> For instance, if you need runtime theming or user-interactive styles, CSS variables are the way to go.For consistent design systems and advanced styling logic, Sass variables are beneficial.**By understanding and utilizing both, you can write more efficient, maintainable, and dynamic stylesheets tailored to your project's needs."**

* v některých dalších SASS funkcích tyhle proměnné použijeme, ale třeba na barvy apod. až tak ne

* můžu dělat třeba: `$spacing/2` místo `calc()`
* můžu i udělat tohle:
  ``` scss
  $spacing: 20px;
  $spacing-half: $spacing / 2;
  $spacing-double: $spacing * 2;
  ```
  * a pak budu měnit jen `$spacing`

___

## Mixins

* **SASS mixins** are **reusable CSS code snippets** that let you apply styles easily
* instead of repeating styles, create a mixin once and reuse it
* They can also accept input values for customization, saving time and keeping your code cleaner


``` scss
@mixin button-style($color) {
  background-color: $color;
  border: none;
  color: white;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
}

.button-primary {
  @include button-style(blue);
}

.button-secondary {
  @include button-style(green);
}
```

* **argumenty v mixinu** 
  * třeba pokud bych chtěla do mixinu pokaždé nastavit jinou velikost fontu - zadám proměnnou do definice mixinu &#8594 pak když ho použiju, tak do té závorky dosadím velikost pro to konkrétní použití:
  ``` scss
  @mixin webFont($fontSize) {
    font-family: "Open Sans", sans-serif;
    font-weight: 700;
    font-style: normal;
    font-size: $fontSize;
  }
  
  header {
    @include webFont(2rem);
  }
  
  body {
    @include webFont(1rem);
  }
  ```
  * těch parametrů jako `$fontSize` tam můžu zadat do té závorky víc, a oddělit je čárkou

* pokud bych stejnou proměnnou měla definovanou nahoře v tom CSS globálně, tak se stejně v rámci toho konkrétního mixinu aplikuje to, co jsem určila pro ten mixin (lokální proměnná)

* můžu do místa, kde mixin použiju, přidat další styly a pak do mixinu napsat `@content`, čímž tam to CSSko přidám

* ale tohle asi ne nutně budu chtít dělat, prostě to přidám individuálně u některých, kde potřebuju dát CSSko nad rámec toho mixinu

* v SASSu jde programovat i nějaká logika, a během toho to použiju, ve specifických případech - například za nějakých podmínek se tam to CSSko přidá do toho mixinu

* když vytvářím lokální proměnné pro mixin, pro vlastnost, která může mít víc parametrů - třeba `gradient`, tak tam můžu zadat ten mixin takhle:
    ``` scss
    @mixin gradient ($colors...) {
      background-image: blabla;
    }
  ```
  a pak ten mixin použiju:

  ``` scss
    .box {
      height: 200px;
      @include gradient (lime, blue);
  }
  ```
* ve výsledném CSS se pak namísto `$colors...` doplní seznam těch barev
  * ty tři tečky značí, že tam může doplnit libovolné množství parametrů (v tomhle případě barev), oddělených čárkou
  * takže tohle by se dalo použít i třeba na `box-shadow`:
  ``` scss 
  @mixin box-shadow($shadows...) {
    box-shadow: $shadows;
  }

  .box {
    @include box-shadow(2px 2px 5px black, 5px 5px 10px gray);
  }
  ```
  * ten parametr, kterému pak můžu přiřadit neomezený počet hodnot, musí být v pořadí v definici mixinu jako poslední:
  ``` scss
  @mixin gradient ($height, $colors...) {
  }

  .box {
    @include gradient (200px, lime, blue, red, yellow, purple);
  }
  ```
* **typický mixin** by ale byl třeba takhle:
  ``` scss
  @mixin box($borderColor: blue) {
    background: white;
    padding: 20px;
    margin: 50px;
    border: 2px solid $borderColor;
    border-radius: 15px;
  }

  .box1 {
    @include box();
  }

  .box2 {
    @include box(red);
  }
  ```
* tzn. v definici mixinu budu mít určenou klidně jednu hodnotu, která se aplikuje na většinu elementů, které ten mixin použijí, ale zároveň přes tu lokální proměnnou např. `$borderColor` si nechávám možnost vytvořit si varianty, kterým změním jednu z těch vlastností, třeba barvu `border`u

* něco chci používat často, ale ne vždycky identicky, zároveň je to uzpůsobitelné na jednom místě, když jde o větší projekt

___

___

### Element transformation

>vsuvka

* je to fajn, když tím animuju, protože se to akceleruje na gragické kartě a tím je to míň náročné na výkon PC (?)

``` scss
.box {
transform: translate(300px,200px); // posun - můžu napsat i v procentech, pak se to posune o procenta šířky elementu oproti jeho původní pozici
transform: translateX(); // posun po ose x
transform: translateY(); // posun po ose y
transform: rotate(); // otočení
transform: scale(); // zvětšení
transform: skew(); // naklonění
}
```

* často se používá ve chvíli, kdy chci prvek animovat například při `:hover`
  * spolu s vlastností `transition: transform 0.3s` k tomu prvku
  * tím bude přechod pomalejší a hezčí
  * jde to použít zároveň - rotate, scale etc.

``` scss
transform-origin // to určí, po jaké ose se to bude otáčet
```
  * pokud budu dělat animace jako třeba reakci na uživatelovu akci jako `:hover`, tak čím pomalejší jsou, tím to uživatele víc sere a stránka působí pomale - doporučuje se 0.2s - 0.3s

___
___

**Inheritance, Placeholders, Lists and maps a Logic použijeme prý jako začátečníci extrémně málo často - třeba v CSS, co má tisíce řádků** 
___
___

### Lists and maps

### Interpolation

* příklad - budu mít box a na jedné straně to bude mít zvýrazněný rámeček
* do mixinu přidám vlastnost, která mi určí, na které straně má být zvýrazněný, ale chci, abych to mohl pokaždé určit flexibilně přes mixin:

  ``` scss
  @mixin box ($side) {
    background: #e5e5e5;
    padding: 20px;
    font-family: sans-serif;
    border-right: 10px solid red; 

  }

  .odstavec1 {
    @include box();
  }

  ```
* nelze použít proměnnou v názvu vlastnosti jen tak - např: ~`border-$side:`~

* ale existuje fce, kterou řeknu, že chci proměnnou tzv. `interpolovat` na text, aby se v té vlastnosti dala použít, například `border-#{$side}`:

  ``` scss
    @mixin box ($side) {
      background: #e5e5e5;
      padding: 20px;
      font-family: sans-serif;
      border-#{$side}: 10px solid red;
  // takhle sem použiju interpolaci a pak když určím stranu, tak se doplní proměnná $side jako text
    }

    .odstavec1 {
      @include box(right);
    }    
  ```
* těch parametrů si tam můžu takhle vytvořit, kolik chci, například:
  ``` scss
        @mixin box ($side1: right, $side2: bottom) {
        background: #e5e5e5;
        padding: 20px;
        font-family: sans-serif;
        border-#{$side1}: 10px solid red;
        border-#{$side2}: 10px solid red;
  // takhle mi to dá tu border vpravo i dole, ale třeba bych si mohla dle situace vybrat, kterou z těch proměnných použiju
      }

      .odstavec1 {
        @include box(right, bottom);
      }    
  ```
___

## Logic in SASS

* můžu například udělat podmíněné formátování pomocí `@if - @else`

  ``` scss
  @mixin border($color, $side) {
    @if $side == all {
      border: 3px solid $color;
    } @else if $side == none {
      border: none;
    } @else {
      border-#{$side}: 3px solid $color;
    }
  }
  
  p {
    @include border(red, all);
  }
  div {
    @include border(blue, none);
  }
  header {
    @include border(green, top);
  }
  ```

### Cycles

* podobné jako for v Javascriptu

* cyklus `for` - potřebuju něco xkrát opakovat
* třeba chci vyprodukovat automaticky několik `item` s konkrétními vlastnostmi - místo toho, abych to psal rukou takhle:

  ``` scss
    .item1 {}
    .item2 {}
    .item3 {}
    ....
    .item9 {}
  ```
tak si napíšu cyklus:

  ``` scss
  @for $i from 1 through 9 {
    .box#{$i} {background: gray;}
  }// $i počítá, na které té položce zrovna je, přes interpolaci ještě zajistím, aby se ke každému boxu přidalo to číslo jeho pořadí do názvu
  ```

  * proměnnou `$i` můžu použít i uvnitř nastavení vlastnosti:
  ``` scss
    @for $i from 1 through 9 {
    .boxWidth#{$i} {width: 10px * $i;}
  }
  ```
___

### Lists

* pro automatické dosazování barev nebo jiných hodnot
* vytvořím si seznam s hodnotami třeba ve `_variables.scss`, a ty potom pomocí funkce `@each` budu procházet a používat z nich ty hodnoty
* použiju, když třeba vytvářím set výchozích hodnot pro web, které potom buď využiju já, nebo pošlu kolegovi - když budou chtít třeba barvy změnit, stačí, aby je přidali do toho seznamu `$colors`
* takhle může ušetřit čas, pokud vytvářím dlouhé bloky CSS na základě nějakých hodnot

  ``` scss
  $colors: red, green, blue, yellow, pink, black;
  $sizes: 20, 50, 100, 200; // tady nedám jednotku, ale v tom cyklu @each dám každou hodnotu vynásobit 1px - takhle se mi vytvoří hezčí ten název třídy, bude obsahovat jen w/h a číslo a NEBO můžu udělat interpolaci přes #{$ }

  @each $color in $colors {
    .bg#{$color} {
      background: $color;
      color: white;
    }
  }
  ```

  * to samé můžu udělat třeba se `$sizes`
 
  ``` scss
    @each $size in $sizes {
      .w#{$size} { width: $size * 1px; }
    }

    @each $size in $sizes {
      .h#{$size} { height: $size * 1px; }
    }
  ```
  * nebo přes interpolaci bez násobení pixely:
  ``` scss
    @each $size in $sizes {
      .h#{$size} { height: #{$size}px; } // nemůžu napsat třeba $sizepx
    }
  ```
> je úplně jedno, jak nazvu tu proměnnou - jestli `$size`, `$color`, nebo `$abc` - hlavní je název seznamu, ten musí odpovídat - ale hodí se to nazývat smysluplně

___

### Maps
* tohle je užitečné spíš pro velké weby
* `mapa` je taky seznam, ale obsahuje nejen hodnoty, ale i jejich názvy

  ``` scss
  $barvy: (
    primary: hotpink,
    secondary: blue,
    error: red,
    success: green,
    // kdycbych pak třeba zapomněl, že na webu jsou nějaké další messages, které naskakují uživateli, tak je normálně dodám sem do seznamu, a tím se pak můžou hned generovat jejich třídy:
    warning: orange,
    info: dodgerblue
  );
  ```
* budu chtít generovat třídy například `.bg-primary` ne `bg-hotpink`

  ``` scss
    @each $name, $value in $barvy {

      .message-#{name} {
        background: $value;
      }
    }
  ```
* Lists a Maps použiju obzvlášť třeba na hodnoty, které na webu potřebuju použít na spoustě různých míst
  * třeba na button:
    ``` scss
      @each $name, $value in $barvy {
          .button-#{name} {
            border: 2px solid $value;
          }
        }
    ```
  

> Inheritance a Functions přeskočil, že to moc nevyžijeme a bylo by to matoucí

___

## Introduction to Responsive Web Design and units

* RWD - vytváření CSS, které se přizpůsobí tomu, na jakém zařízení se to prohlíží
* cílem je co nejlepší user experience na všech zařízeních
* **responsive web design vs. adaptive web design**:
  * RWD: nejčastěji řeší šířku displeje, ale nejen to
    * nastavuje se přesně podle šířky displeje, i po pixelu
  * AWD: přednastavené rozpětí šířek, a podle nich individuální design
  * ale dneska se už dělá kombinace obojího - mám pevně daná rozpětí, ale v okolí těch hranic se to plynule přizpůsobuje změnám
  * už se moc neřeší, jestli je to RWD nebo AWD

* dnes už **responsive web design = web design**

* např. pro mobile devices nemá moc cenu nastavovat `:hover`, protože nemívají myš, kterou by někdo na nějaký element mohl najet

___

### Units

* **absolutní jednotka** = `px`:
  ``` scss
  .parent {
    font-size: 20px;
  }
  ```
* **relativní jednotky:**

  * `em` = počet jednotek `font-size` rodiče
    * `2em` = velikost 2x větší než `font-size` rodiče
    * pokud není `font-size` v rodiči nastavená, dědí se vždy o úroveň výš, až k defaultní `font-size` v prohlížeči, což bývá `16px`
    * u jakékoli jiné vlastnosti než `font-size` počítá jednotka `em` s `font-size` prvku, ve kterém se ta vlastnost nachází:
  
    ``` scss
    .child {
      font-size: 2em; // počítá se z font-size rodičů
      border: 2em solid gray; // počítá se z font-size elementu samotného
      }

  * `rem` = počet jednotek velikosti `root element = html` (`2em` = 2x větší než velikost písma nebo vlastnosti v `html`)

  * `%`
  * `vw`
  * `vh`

* obecně je dobré **nenastavovat velikost písma v `px`**, protože nedává uživateli možnost zvetšit si písmo v prohlížeči - resp. i když to udělá, tak tam, kde jsem nastavil velikost písma v `px`, tak to nastavení nic nezmění

* `font-size` raději nastavovat v `em` nebo `rem`
* vlastnosti písma se dědí z `parent` na `child` element, pokud jsem je někde nezměnil

* když chci nastavit svému webu konkrétní velikosti písma, ale i tak dát uživateli možnost zvětšit si písmo v prohlížeči, tak best practice je tohle:

  ``` scss
  html {
    font-size: 150%; // nastavím v procentech hlavní velikost písma - bude to 1,5 x 16px v tomhle případě, počítá se to z prohlížeče
  }

  .parent {
    font-size: 2rem; // nastavím v relativní jednotce rem, která se počítá z html
  }

  .child {
    font-size: 2rem; // nastavím v relativní jednotce rem, která se počítá z html
  }
  ```
  **takhle má uživatel pořád možnost nastavit si v nastavení prohlížeče větší písmo, a já mám možnost zachovat si poměry velikostí písma tak, jak jsem zamýšlel**

* `height` se nastaví podle výšky obsahu, pokud ji nenastavím napevno
  * např. `height: 50%` můžu nastavit jedině pokud rodič toho prvku má napevno nastavenou `height`, jinak to nic nenastaví

* **viewport units:**
  |  UNIT  |  DEFINITION  |
  |-------|--------------------------------------|
  | 1vw | 1% of viewport width |
  | 1vh | 1% of viewport height |
  | 1vmin | 1vw or 1vh, whichever value is lower |
  | 1vmax | 1vw or 1vh, whichever value is higher |

  These properties indicate that for a viewport (or 'workspace') of a screen that is 1000px wide, the value of 1vw equals: 1% x 1000px = 10px.