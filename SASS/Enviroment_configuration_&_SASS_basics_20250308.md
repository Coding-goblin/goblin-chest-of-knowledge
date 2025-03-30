# Enviroment configuration and SASS basics

* SASS = CSS preprocessor
* nástroj, který nám umožní psát do CSS věci, co tam jinak psát nejdou - rozšiřuje syntaxi, a kód potom převede do správné CSS syntaxe
* node.js = interpreter javascriptu

* budeme ignorovat *Parcel* i *Webpack*, a bude nám říkat o bundleru, který se jmenuje **Vite** [vít] - používá většina vývojářů, je moderní

___

* bundlery se instalují per project, do každého jednotlivého projektu
  * tím se zajistí, že kdyby nastala nějaká breaking change kvůli nové verzi toho nástroje, tak to dál bude používat tu starou, která tam byla při první instalaci
* standardně se instaluje nástroj ke každému setu `index.html` a `css`
* my budeme instalovat do té složky, kde máme všechno, a pokaždé řekneme, která složka je aktuální

* soubor `.json` říká, jaké nástroje se v projektu používaji
`package.json` je základní výchozí soubor
  * obsahuje popis projektu, verze, popis, autora (my nepotřebujeme)
* scripts: obsahují skripty, které budeme chtít spouštět, tam si je předpřipravíme
* `devDependencies`: nástroje, které projekt potřebuje ke svému chodu
* `dependencies`: nástroje nebo části kódu

* npm = [node packages manager](https://www.npmjs.com/)
  * součást node.js
  * přes to přidáváme balíčky, nástroje, ikony, fonty atp.
    * **Vite** i **SASS** jsou taky balíčky

* příkazový řádek v editoru mi automaticky zvolí složku projektu, kdybych ho otevřela zvlášť, tak si dát pozor, abych tam měla tu správnou složku

* zadám do příkazového řádku pro instalaci balíčků vite a SASS:
  ```
  npm install -D vite  # The `-D` flag installs the package as a development dependency.
  npm install -D dart-sass  # The `-D` flag installs the package as a development dependency.
  ```

* automaticky přibyde složka `node_modules`, kde budou ty nainstalované balíčky - programy, skripty, fonty, balíčky
* já jsem chtěla jen vite, ale vývojář vite si do vite nainstaloval balíčky, které potřeboval on, takže vite závisí na těch balíčcích, pak ti vývojáři těch balíčků to samé...je to na sebe navázané -> doinstalovalo se třeba 30 balíčků kvůli tomu
* už jen tímhle má ta složka node_modules asi 22 MB

* když posílám na github, a něco tam uploadovat nechci, to není můj kód a bylo by to moc změn -> dám to do `.gitignore`
  * ty `node_modules` se automaticky ignorují, nemusím je tam dávat
  * ale kdybych repozitář clonovala na jiný počítač z Githubu, tak tam tu složku mít nebudu!!!

* `package.json` tam zůstává, to je důležitá informace
* jelikož mám ale `package.json`, tak napíšu do příkazového řádku `npm install`, a podle toho se vše nainstaluje
  * balíčky můžu kdykoli doinstalovat
* do vite.config dávám u každé složky:
  ```
  // vite.config.js
  import { defineConfig } from 'vite'

  export default defineConfig({
    // Replace with the path to your folder containing index.html
    root: './01_Day_1/01_Sass_basics/01_First_steps_in_SCSS',
  })
  ```
* pak jen vždycky změním `root`
* když budu chtít dělat na jiné složce, tak přidám nový root a zakomentuju přes `//` tu předchozí - radí nám, abychom ty předchozí nemazali

___

* náhled html si zobrazím přes NPM scripts - start (nebudu už používat live preview nebo live server - to by už nedokázalo spustit všechny ty předprocesy)
  * tady se spustí bundler
  * přes `localhost` link si otevřu adresu, kde uvidím náhled webu
  * funguje to jako live server, ale trochu jinak - například se to automaticky refreshuje podle změn
  * pokud ukončím proces toho startu v `command`, tak se ukončí live server a přestanou se refreshovat změny

___

### `gitignore`
  * pokud tam není už vytvořený, resp. pak u každého nového projektu, tak vytvořím nový soubor .gitignore a tam pak na každý řádek přidám název folderu, který se nemá uploadovat na github

### node_modules
* dost často třeba nástroj jako vite použiju k tomu, aby mi to samo nastartovalo projekt a všechno nastavilo, včetně `.gitignore` apod. (to přijde později)


* **sass file &#8594; sass compiler &#8594; css file**
  * css file nebudeme nikdy upravovat, budeme upravovat sass soubor - podle něj se překompiluje ten css file

* dvě syntaxe: sass a scss - my budeme používat scss - ta je teď nejrozšířenejší
* do scss můžu normálně psát i basic css, nejen tu speciální syntaxi

* když na konci projektu chci někam vystavit na web všechno, spustím `build` v `package.json` a to vytvoří složku `dist` se soubory výsledného webu - `assets, index.html`
* vite vzal scss soubor a vyrobil soubor css, který je pak ve složce `assets`
  * tohle nemusím teďkon dělat, budu používat start

___

## SASS variables

``` scss
$primary-color: #ff3311;
```
  * dávám nahoru do scss
  * ale pokud nemám nějaký extrémní důvod použít SASS proměnné, doporučuje se používat normálně variables css

```css
:root {
--primary-color: #ff3311;
}
```
* pokud použiju sass proměnné, tak pak se to do toho vyprodukovaného css dá jako ručně napsaný kód - napevno se to tam přepíše, ne jako proměnná 
  * narozdíl od CSS proměnných, které zůstanou jako proměnné, nebudou tam natvrdo napsané
  * nativní css proměnné fungují dynamicky, což je výhodnější
* do sass proměnných můžu dát cokoliv, i víc než do těch klasických (ale to mě teď nemusí trápit)

___

## SASS comments
> (spešl, fungují i normální CSS)

``// comment in Sass``

* jde to jen na jeden řádek
* je ale úplně jedno, jestli budu psát css nebo sass komentáře (dokud dělám v sass - v css mi sass komentáře nepůjdou :))
* ale ty sass comments jsou lehčí a rychlejší, a stejně je pak napíšu i v javascriptu, takže jsou oblíbené

___

## SASS compilation

* sass compiluje css tak, aby zabíralo co nejmíň dat, narve to všechno do jednoho dlouhého řádku - mezery apod. prohlížeč nevyžaduje, to je pro mě, aby se to dobře četlo
* sass taky během kompilace odstraní komentáře v css
* celkově ten soubor optimalizuje

___

## SASS: working with partials

* díky sass můžu pracovat s více css soubory, které potom sass spojí do jednoho
* takže každá jednotlivá sekce webu bude mít svoje css & bude jeden společný css soubor
* třeba nastavení variables bude mít často svůj vlastní soubor


### Partial(s)
* budu mít ve složce scss jednotlivé partials, jejiž jméno bude vždy začínat _
* takže takhle si ten projekt rozkouskuju do sekcí, například `_variables.scss`
* přes 
  ``` scss
  @import "_variables";
  ```
  to potom napojím do `main.scss`
* tohle mi u větších projektů pomůže k přehlednosti apod.
potom sass sežere ten můj soubor `main.scss` a kde mám ty importy, tam to nahradí kódem z těch jednotlivých partials
dneska už probíhá přechod z `@import` na `@use` (ale to má pak jiné postupy)

* v budoucích verzích bude pak už jen `@use`:

___

### `@use` vs. `@import` in SCSS

#### `@import` (Old Method)
- Used to import other SCSS files.
- Merges styles, which can lead to duplication and performance issues.
- Causes global scope pollution, making it harder to manage variables and mixins.

  ```scss
  @import "variables";
  @import "mixins";
  ```

#### `@use` (Modern Approach)
- Introduced in Dart Sass to replace `@import`.
- Automatically **namespaces** imported files, preventing conflicts.
- Loads each file only once, improving performance.

##### Using `@use` with `as *`
- Imports a file and **removes** its namespace, making all variables, mixins, and functions globally accessible.
- **Use with caution** to avoid conflicts.

  ```scss
  @use "variables" as *; // Now all variables can be used directly
  ```

##### Using `@use` without `as *`
- Keeps the imported file **namespaced**, requiring explicit reference.
- Encourages modularity and avoids naming conflicts.

  ```scss
  @use "variables";

  body {
    color: variables.$primary-color;
  }
  ```

#### Key Differences

| Feature        | `@import` (Old) | `@use` (New) |
|---------------|---------------|-------------|
| Performance   | Slow (multiple loads) | Fast (loads once) |
| Scope         | Global (pollution risk) | Namespaced (unless `as *` is used) |
| Maintenance   | Harder (conflicts) | Easier (modular) |
| Future-proof  | Deprecated | Recommended |

___

* je i nějaká doporučená struktura složek a souborů, jak ty partials organizovat, např.:

scss/
|-- settings/
|    |-- _all.scss
|    |-- _colors.scss
|-- generic/
     |-- _reset.scss
|    |-- _print.scss
|-- elements/
|    |-- _base.scss
|    |-- _buttons.scss
|    |-- _fonts.scss
|-- vendor/
|    |-- _jquery.ui.scss
main.scss
asi lepší doporučení zde: https://sass-guidelin.es/#architecture
NESTING

teď se ještě používá v SASS, teď už je to v podstatě implementováno do CSS, neumí to ještě pár mobilních prohlížečů


místo:
.menu li {
    list-style: none;
    border: 2px solid blue;
    padding: 10px;
 }
můžu udělat tohle:

.menu {
    display: flex;
    gap: 10px;
    padding: 0;

    li {
        list-style: none;
        border: 2px solid blue;
        padding: 10px;
    }
}
 takhle zanořené styly se pak ve výsledném css ale stejně rozdělí do těch klasických takhle:

.menu li {
    list-style: none;
    border: 2px solid blue;
    padding: 10px;
}


je to pohodlnější v tom, že je z toho na první pohled vidět, jaká je struktura stylu
podobně jako u klasických css selektorů je dobré si rozmyslet, kdy to zanořování dělat, protože tím zvyšuju někdy i zbytečně specificitu těch selektorů
stejně tak můžu používat znaky jako + > apod:
.menu {
    display: flex;
    gap: 10px;
    padding: 0;

    > li {
        list-style: none;
        border: 2px solid blue;
        padding: 10px;
    }
}
pokud použiju v nestingu &, tak se tam doplní název selektoru rodiče (?) - můžu takhle vytvořit název dle metodologie BEM a nemusím pořád opakovat ten začátek
.article {
    display: flex;
    gap: 10px;
    padding: 0;

    &__title {
        border: 2px solid blue;
        padding: 10px;

    &__text {
        color: blue
    }
}


taky třeba:
.link {
   display: inline-block;
   padding: 10px;
   text-decoretion: none;
   color: black;

  &:hover,
  &:focus {
    background: lightblue;
  }
  
  &:active {
    background: blue;
    color: white;
  }
}

> vsuvka: Bestshop project - je v poradku nastavit napevno napr. hodnoty sirky elementu, ale pak udelam casem treba pomer procentualni
> nastavim napevno `623px` a `440px` a pak dam `flex-shrink` a `flex-grow` obema 1 1
> v realu bych treba poresila s grafikem, ze obrazek bude fixne velky a text vedle se bude zuzovat a roztahovat dle stranky - preferenci si rekne treba grafik
>nastavovat `min-height`, ne `height` napevno, protoze treba firma prida vic textu a rozhaze se tim ten boxik