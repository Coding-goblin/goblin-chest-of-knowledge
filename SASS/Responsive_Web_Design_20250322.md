2. den SASS a RWD zakončili tím, že se bavili o responzivních a absolutních jednotkách



v případě em jednotka odkazuje na rodiče, v případě čehokoli dalšího na tom elementu, tak se to vztahuje na ten element samotný? zeptat se GPT...


viewport units:
1vw = 1% of viewport width;
1vh = 1% of viewport height;
1vmin = 1vw or 1vh, whichever value is lower;
1vmax = 1vw or 1vh, whichever value is higher;
These properties indicate that for a viewport (or 'workspace') of a screen that is 1000px wide, the value of 1vw equals: 1% x 1000px = 10px.


@media queries: "když platí nějaká podmínka, použij nějaké css" - tím můžeme reagovat na šířku viewportu/displeje
nebo třeba na to, jestli je to screen nebo print (vytiskne se to jinak, než když je to zobrazené na displeji) - typy medií: all, screen, tv, print, projection, speech, braille, embossed, handheld (škrtnuté se moc nepoužívají)


@media (max-width: 600px) {
  .sidebar {
    display: none;
  }
}
hranice, kde se změní layout na základě šířky viewportu = breakpoint (ale to se vztahuje na zásadní změny, chci, aby to bylo flexibilní a responzivní i mezi nimi)
@media (orientation: portrait) {
.box background: red;
}
můžu určit layout podle toho, jestli je to na šířku nebo na výšku
když dělám RWD, často se začíná od layoutu pro mobil - tzv. mobile first přístup


tedy začíná se od nejjednoduššího designu, a pak přes další a další @media queries to rozšiřuju pro větší displeje na složitější design


víc než 60% (teď už asi víc) návštěvníků webu přichází z mobilu


design pro mobil je mnohem složitější na vymyšlení, musím vymyslet, co bude a nebude vidět, většinou to bude vše pod sebou; nakódovat už je to jednodušší


.box {
  border: 2px solid black;
  padding: 20px;
  background: lightblue;
}

@media only screen (max-width: 768px) {
  
  .box {
    background: pink;
   }

  .container {
    display: flex;
    gap: 20px;
   }
}
pořadí kódu je důležitý - mobil bude první, pak přes @media budu nabalovat další styly
všechny hodnoty ze stylu mobilu zůstávají, jen přepisuju některé z nich, případně přidávám
většinou
1. mobil (nejužší zařízení)

2. tablet (prostřední)

3. PC (nejširší)

pokud bych chtěl, aby to nemělo rámeček, tak musím dát border: none; - to se pak dědí do následujícího @media


standard breakpoints: vznikly na základě nějakýho grafu s průměrnými šířkami


ve vývojářských nástrojích to můžu vidět, když posunu hranu prohlížeče (uvidím, jak to reaguje na můj design podle šířky)
můžu nestovat @media pod elementy - takhle to vypadá na mobilu, tabletu, PC:
body {
    background: green;

    @media (min-width: 400px) {
        background: violet;
    }

    @media (min-width: 800px) {
         background: orange;
    }
}
v @media query nejde použít nativní css proměnné :root {}
můžu ale udělat sass proměnné:
$tablet: 400px;

@media (min-width:$tablet).......
nebo můžu udělat @mixin, který budou ty @media obsahovat
opsat si kód od něj - ukazoval před obědem
pak můžu upravit jen ty @mixins a změní se mi to všude


SCSS Grid

SCSS grid a CSS grid nejsou to stejné
používá se pro rozvržení elementů na stránce v pomyslných (většinou) 12ti sloupcích
šířku určím tak, že každý element bude měřit x sloupců/12
Construction of grid - elements:
container,
rows,
columns,
gutters (spaces between columns)


<div class="container">

    <div class="row">
      
      <div class="col-4"></div>
      <div class="col-6"></div>
      <div class="col-2"></div>

    </div>

  </div>
to číslo u class je pro mě značka, kolik sloupců v tom gridu bude element zabírat
mezery mezi těmi sloupci je nejběžnější a nejlepší nastavovat NE pomocí gap, ale pomocí padding


automatické vygenerování gridu:
@use "sass:math";

* {
    box-sizing: border-box;
}

$columns: 12;

$column-base-width: math.div(100%, $columns);
$gutter: 24px;

.container {
    max-width: 960px;
    margin: 0 auto;
}

.row {
    display: flex;
    flex-wrap: wrap;
}


[class*="col-"] {
    min-height: 1px;
    width: $column-base-width;
    padding: 0 math.div($gutter, 2);
}

@for $i from 1 through $columns {
    .col-#{$i} {
    width: $column-base-width * $i;
    }
}

@media (max-width: 600px) {
    [class^="col-"] {
    width: 100%;
    }
}
