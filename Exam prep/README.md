# Příprava na zkoušku

* v LMS se otevre zkouska s 5 ukoly, vsech 5 prikladu ma jeden repozitar
* je tam pouzity parcel, ale on by rad, abychom pouzili `vite`
* kdyz si repozitar stahnu, bude nastaveny na `parcel` 

## ⚠️ Úprava nastavení repozitáře před započetím práce

Snad si pamatujete, že jsme si na začátku bloku o Sassu říkali, že všude v materiálech se používá a zmiňuje nástroj Parcel, ale změnili jsme to na modernější a používanější nástroj Vite.

Protože i repozitář s příklady pro zkoušku je připravený s nástrojem Parcel, musíme si nejdřív upravit konfiguraci na Vite, aby vše fungovalo správně.

Naklonujte si repozitář se zkouškou a proveďte následující kroky:
1. Nahraďte obsah souboru `package.json` následujícím:
  ```json
  {
    "name": "sass",
    "version": "2.0.0",
    "scripts": {
      "start": "vite",
      "build": "vite build"
    },
    "devDependencies": {
      "sass": "^1.85.1",
      "vite": "^6.2.1"
    }
  }
  ```

  udelam v prikazovem radku prikaz `npm install`
  pres `start` se podivam na nahled
  `build` vubec nepouziju, jen pak dam push a pak pull request

2. Vytvořte v kořenové složce repozitáře nový soubor `vite.config.js` a vložte do něj následující text:
  ```js
  // vite.config.js
  import { defineConfig } from 'vite'

  export default defineConfig({
    // nahraď/odkomentuj vždy složku pro konkrétní příklad ze zkoušky
    root: './01_Task_1',
    // root: './02_Task_2',
    // root: './03_Task_3',
    // root: './04_Task_4',
    // root: './05_Task_5',
  })
  ```
  Nezapomeň před každým příkladem napsat za `root:` složku, ve které se příklad nachází. Nebo odkomentuj jeden z připravených řádků.

## Písma

- připojení fontu z Google Fonts - je lepsi pripojit v `index.html` před <link rel="stylesheet" href="main.scss"> - web se načte správně
- použití `em` a `rem` jednotek

- vždycky si nastavit `box-sizing: border-box` a `margin` a `padding` na `0` (tohle udelam, kdyby po mne chteli ve zkousce basic reset)

## Flexbox

- základy flexboxu
- zarovnání / centrování pomocí flexboxu:
  ```scss
  .rodic {
  display: flex;
  justify-content: center;
  align-items: center;
  }
  ```
- pokud je tam těch věcí víc a nechci, aby se namatlaly třeba dvě položky vedle sebe, tak dám `flex-direction: column;`

## Flexibilní média

Podobně jako u obrázků, základem je nastavení šířky na `100%`.

```scss
.video {
  width: 100%:
  max-width: 1000px;
}
```

Pozor, že například videa vložená z YouTube, se do kódu vkládá pomocí značky `<iframe>`, takže šířku nastavujeme na tuto značku.

nastavovat `aspect-ratio: 560/315;` <- napr. pomer puvodnich velikosti iframe
ale je nutny zaroven nastavit `height: auto;`

## Sass

### Vytváření proměnných s Sassu:

```scss
$primaryColor: #ff8000;
$spacing: 20px;
$baseFont: "Lato", sans-serif;
```

* sice pokud nemam spesl duvod bych mel pouzivat css promenne pres root, ale ve zkousce jsou sass promenne
* pokud vnoruju do rodice pseudoelement jako treba ::before, tak tam musim dat &::before, aby se to aplikovalo na rodice
* 


### Moduly a funkce

Sass má v sobě zabudovaných několik [modulů](https://sass-lang.com/documentation/modules/) obsahujích tzv. *funkce*.

- **Math**: obsahuje matematické funkce
- **List**: modul pro páci se seznamy
- **Map**: modul pro pási s tzv. mapami
- **Color**: modul pro práci s barvami

Konkrétní knihovnu musíme vždy připojit do souboru, kde chceme funkce použít. Například:
```scss
@use "sass:math";
```
napriklad v sassu dlouho slo delit $variable / 2
ale tekdon uz naskakuje varovani
misto otho se ma pouzivat fce div z modulu math - naimportuju si modul `math` a pouziju `math.div($spacing,2)`

### Color - práce s barvami

Připojíme modul pro práci s barvami:
```scss
@use "sass:color";
```

Často potřebujeme vytvořit odstíny variace určité barvy (např. světlejší, tmavší). Můžeme použít funkci `color.adjust()` nebo `color.scale()` pro změnu libovolného aspektu barvy (odstín, jas, poad.).

```scss
$barva: #ff8000;
$svetlejsi: color.scale($barva, $lightness: 30%, $space: oklch; )
$tmavsi: color.scale($barva, $lightness: -30%, $space: oklch; )
```

treba muzu udelat:

```scss
a:hover {
  background: color.scale($barva, $lightness: -30%, $space: oklch; ); // ale muzu si tohle cele dat do variable treba $buttonDarker
}
```
to oklch nejak nejlip spocita prechod mezi temi odstiny barev (?)

### Mixiny

Mixin vytvoříme pomocí direktivy `@mixin` a použijeme ho pak pomocí direktivy `@include`:

```scss
@mixin square() {
  display: block;
  width: 100px;
  height: 100px;
}

.mujCtverecek {
  @include square;
}
```

Mixiny mohou mít i parametry. V místě, kde mixin použijeme pak do něho můžeme poslat. Například bych chtěl svému čtverečku říct, jak má být velký.

```scss
@mixin square($size) {
  display: block;
  width: $size;
  height: $size;
}

.maly {
  @include square(50px);
}

.velky {
  @include square(200px);
}
```
* muzu nastavit tech parametru i vic najednou do zavorky, oddelim jejich variables carkou

Protože v Sassu běžně můžeme používat běžně používat i vnořování, lze to samozřejmě udělat i v rámci mixinu. Můžeme použít znak `&` jako zástupce pro rodičovský selector.

Pokud bychom tedy k našemu čtverečku chtěli například přidat pseudoprvek `::before`, můžeme to udělat např. takto.

```scss
@mixin square($size) {
  display: block;
  width: $size;
  height: $size;

  &::before {
    content: "";
    // další vlastnosti, které chceme nastavit
  }
}
```
nebude se po nas chtit pouziti partials, ty kody nebudou dlouhe

### Seznamy

Někdy potřebujeme v Sassu mít seznam více hodnot, se kterými pak můžeme nějak pracovat. K tomu slouží `list`. Nezapomneň modul na začátku připojit.

```scss
@use "sass:list";

$myColors: #f00, #0f0, #00f;
```

Použití hodnoty ze seznamu:
```scss
p {
  color: list.nth($myColors, 1); // $nazevSeznamu, 1 - kolikata hodnota to je
}
```

Můžeme zjistit počet položek v seznamu:
```scss
list.length($myColors);
```
Samotné je nám to k ničemu, ale můžeme to použít pro další zpracování, např. procházení seznamu a práci s jednotlivými hodnotami.

### Mapy

Pokročilejší podobou seznamů jsou tzv. **mapy**. Mapa je v podstatě seznam, kde nejsou jen hodnoty, ale každá hodnota má i nějaké jméno (říkáme mu **klíč**). Pouziva se casteji nez seznamy.

```scss
@use "sass:map";

$colors: (
  "header": #b06,
  "text": #334,
  "footer": #567
);
```

Když potom chceme přistoupit k nějaké hodnotě uvnitř mapy, použijeme pro přístup k hodnotě její klíč/jméno:

```scss
.header {
  background-color: map.get($colors, "header");
}
```

### Cyklus @for

Cyklus `@for` obecně v programování používáme, když potřebujeme něco vykonat vícekrát za sebou. V Sassu nám slouží pro programové generování CSS kódu.

```scss
// od start do end včetně
@for $var from <start> through <end> {}

// od start do end, ale end tam nebude
@for $var from <start> to <end> {}
```

Například budu chtít vytvořit třídy `.item-1`, `.item-2` a `.item-3`, kde každá z nich bude mít nastavenou šířku na `100px`, `200x` a `300px`.

```scss
@for $i from 1 through 3 {
  .item-#{$i} { width: 100px * $i; }
}
```

Pozor na to, že pokud chci Sass proměnnou použít v názvu třídy, musím použít tzv. **interpolaci** pomocí `#{ $promenna }`.


## GRID

Chceme vytvořit vlastní **grid systém**, podobný např. systému z Bootstrapu.
⚠️ Poroz, neplést s nativním CSS gridem (`display: grid;`).

Jde nám o to, vytvořit si vlastní systém, kde prostor rozdělíme např. na 12 stejně širokých částí.
Budeme vytvářet řádky a sloupce. Šířku jednotlivých sloupců chceme nastavovat tak, že řekneme, kolik 1/12 bude sloupec zabírat.

**Například:**
- šířka 3 = čtvrtina stránky (3/12)
- šířka 4 = třetina stránky (4/12)
- šířka 6 = polovina stránky (6/12)
- atd.

**V CSS bychom to tedy nastavili takto:**
```scss
.col-1  { width: 8.333%;  } // 1 dvanáctina ze 100%
.col-2  { width: 16.666%; } // 2 dvanáctiny ze 100%
.col-3  { width: 25%;     } // 3 dvanáctiny ze 100%
// ...atd.
.col-11 { width: 91.666%; } // 11 dvanáctin ze 100%
.col-12 { width: 100%;    } // 12 dvanáctin ze 100%
```

**Row**
Sloupce mají být na stránce vedle sebe, takže budeme potřebovat sloupce uzavřít do rodiče - budeme mu říkat **řádek**, takže použijeme třídu `.row`. A nastavíme na ni **flexbox**.

Zároveň chceme, aby se sloupec dal na nový řádek, když součet šířky sloupců přesáhne 12. Takže na flexbox zapneme i `wrap`.

```scss
.row {
  display: flex;
  flex-wrap: wrap;
}
```

V HTML by potom použití vypadalo takto:
```html
<div class="container">
  <div class="row">
    <div class="col-3">Čtvrtina stránky</div>
    <div class="col-6">Polovina stránky</div>
    <div class="col-3">Čtvrtina stránky</div>
  </div>
</div>
```

Protože chceme, aby byl náš grid konfigurovatelný (třeba jiný počet sloupců na řádku než 12, mezery mezi sloupci, apod.), vytvoříme ho programově pomocí Sassu:

```scss
@use 'sass:math';

$columns: 12;
$column-base-width: math.div(100%, $columns);
$gutter: 24px;

.container {
  max-width: 960px;
  margin-inline: auto;
}

.row {
  display: flex;
  flex-wrap: wrap;
}

[class*="col-"] {
  min-height: 1px;
  width: 100%;
  padding-inline: math.div($gutter, 2);
}

@for $i from 1 through $columns {
  .col-#{$i} {
    width: $column-base-width * $i;
  }
}
```
