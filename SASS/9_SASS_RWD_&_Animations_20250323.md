# Flexible media

* nastavení medií tak, aby se přizpůsobovaly prostoru, kde se nachází
* **2 způsoby:**
  * `max-width: 100%` zůstane stejné (ve svojí největší velikosti) v případě většího prostoru
  * `width: 100%` roztáhne se to v případě většího prostoru

* **image compression** - je třeba optimalizovat obrázky, aby se zmenšily na rozumnou velikost, která odpovídá velikosti, kterou budou zabírat na webu, a zároveň aby se kvalita moc nesnížila - například přes [https://squoosh.app/]

* PNG se dlouhodobě používá jako nejlepší formát - nejmenší velikost s nejlepší kvalitou

* **CSS sprites** - nastavovaly se tak třeba ikony, že se ikony spojily do jednoho velkého obrázku, nastavily se jako pozadí prvku a upravila se jejich pozice - tím se načítá pořád jen ten jeden obrázek

* **responsive background images** - občas se hodí, kdybych chtěla optimalizovat na vysokou kvalitu grafiky pro desktop a rychlé načítání pro mobil
* pro každou šířku monitoru nastavím jinak optimalizovaný obrázek
  * ale zabere to dost času dělat
  ``` scss
  .example {
    height: 400px;
    background-repeat: no-repeat;
    background-size: contain;
  }
  
  @media (max-width: 499px) {
    .example {
      background-image: url("small.png");
    }
  }
  
  @media (min-width: 500px) {
    .example {
      background-image: url("large.png");
    }
  }
  ```

* **device-pixel:** můžou být třeba mnohem menší px (např. na Retina displejích), takže to vytvoří mnohem kvalitnější obraz, ale zároveň to nemusí odpovídat tomu, co jsem nastavil v CSS (není to 1:1)
* přes `@media` query můžeme nastavit, aby se něco zobrazovalo ve větší velikosti pouze na zařízeních, co mají ty jiné velikosti pixelů (ale musím pak mít dva různé obrázky)

* **css-pixel:** to jsou px, co používám v CSS - na normálních běžných obrazovkách odpovídá 1px na obrazovce 1px v CSS

* **`srcset` method**
  * srcset lets you tell the browser: *“Hey, here are different versions of the same image. Choose the best one based on the screen size or resolution.”*
  ``` html
  <img src="image-default.jpg" 
      srcset="image-480.jpg 480w, image-800.jpg 800w, image-1200.jpg 1200w"
      sizes="(max-width: 600px) 480px, (max-width: 1000px) 800px, 1200px"
      alt="A cool landscape">
  ```
  * image-default.jpg = fallback and default image when no `srcset` condition is met
  * `srcset` = images and information about their width
  * `sizes` = tells the browser when to use which picture
  * `1200px` is to be used when none of the mentioned conditions are met - it's the default
  * the browser picks the closest match

* **picture method**
  ``` html
  <picture>
    <source media="(min-width: 45em)" srcset="large.jpg">
    <source media="(min-width: 32em)" srcset="med.jpg">
    <img src="small.jpg" alt="The president giving an award.">
  </picture>
  ```
  * takhle můžu i zvolit třeba jiný výřez obrázku pro zobrazení na mobilu a na desktopu
  * ale i tohle hrozně dlouho bude trvat

* **vektorová grafika**
  * dá se zvětšovat donekonečna a neuvidím pixely, vždycky se to přepočítá na větší velikost

* **vector icons:**
  * use them as fonts and `@font-face` declaration
  * this allows the icons to be defined with vectors and can be scaled to any size without losing quality
  * hodí se třeba na ikony, loga
  * na fotky se musí používat bitmapová grafika (pixely)

* pro iframe (například video z youtube) musím nastavit `width: 100%;` a `aspect-ratio: width/height;`, který byly v originálním kódu a tam je smazat v `index.html`
* na `img`, které chci přizpůsobovat, tak chci nastavovat `display: block;` a `width: 100%;`
  * a pokud nastavuju jako `background` a chci, aby byl vidět celý, tak nastavím `aspect-ratio` s poměrem stran jako má obrázek

___

## Animations

* `transform: rotate(30deg);` otočím objekt o počet stupňů
* `transform: scale(150% nebo 1.5, 0.5);` zvětším nebo zmenším objekt
* `transform: translate (100px, 200px)` posunu objekt, `translate` se vztahuje k té úplně původní pozici
  * pokud jich chci víc najednou, píše se to do jednoho transform a odděluje mezerou

* můžu určit střed animace přes `transform-origin: center` nebo `50%`
* `50%` je default, může být třeba `right`

* **transition vs animation**
* **transition:** hodnota, kterou jsem měnila třeba v :hover 2s (nebo jiný čas);

  ```scss
  transition: 
    transform 2s,
    background-color 1s;
  ```

* obecně čas animace by měl trvat krátce, dlouhé štvou uživatele - takže třeba mezi 0.2s - 0.3s, max 0.4s třeba u větší animace

* **animation** se bude dít sama od sebe, ne v reakci na akci uživatele (to dělá transition)
* musím nadefinovat keyframes:
  ```scss
  @keyframes posun {
    from {transform: translateX(0)};
    to {transform: translateX(200px)};
  }
  ```
* do prvku, kterým hýbu, dám:
  ``` scss
  animation: posun 2s forwards; //(tzn., ze kdyz se provede animace, zustane objekt na miste, kam dosel tou animaci - jinak se to vrati zpatky napuvodni pozici)
  animation: posun 2s infinite alternate; // objekt se bude animovat tam a zpátky donekonečna
  animation: posun 2s infinite alternate linear; // nebude se brzdit na konci animace
  animation: posun 2s infinite alternate ease-in-out; // animace se zbrzdí na konci i na začátku
  ```
* můžu mít i víc `keyframes`, a ke každému změnit i víc vlastností naráz v těch `{}` , zároveň můžu měnit jiný počet vlastností u různých keyframes:
  ``` scss
  @keyframes pohyb {
    0% {transform: translate(0, 0)};
    30% {transform: translate(200px, 200px)};
    50% {transform: translate(200px, 0)};
    75% {transform: translate(0, 300px)};
    100% {transform: translate(0, 0)};
  }
  ```
* rozměry, barvy, paddingy, velikosti písma... všechno, co jde vyjádřit číslem a dopočítat hodnoty mezi dvěma hodnotami, tak lze animovat

* pak složitější animace nějak načasované apod se dělají v javascriptu
