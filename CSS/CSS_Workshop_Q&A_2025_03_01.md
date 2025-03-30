# Workshop Q&A

> (hovor k projektu, ten jsem v tu chvíli ještě nezačala)

___

* tohle udělá jemnější přechod na sekce, když na ně někde linkuju:
  ```css
  html {
  scroll-behavior: smooth;
  }
  ```
* můžu odkázat na další stránku `href="otherpage.html#blabla"` a tím to skočí na konkrétní sekci s `id="blabla"`

* alternativní řešení, jak od sebe dostat položky menu v horním baru:
  * struktura html
  ``` html
  <header clas="header">
    <div class="logo">LOGO</div>

    <nav class="menu">
      <a href="#" class="menu__link">Home</a>
      <a href="#" class="menu__link">About us</a>
      <a href="#" class="menu__link">Contact</a>
      <a href="#" class="menu__link">Blog</a>
  ```
* Ten systém, kdy `<nav>` má `<ul>` a `<li>` a až v těch jsou odkazy, se dřív hodně doporučoval, protože kdyby se nenačetly styly, tak tam pořád zbyde seznam a bude to přehlednější 
* Takhle se tam načtou se odkazy jen s basic textovou mezerou
* Pak to třeba jinak čtou ty čtečky pro nevidomé, ale nevidomá komunita to spíš nemá ráda, když tam je tam seznam, protože to čte *"seznam: položka jedna...etc."*
* takhle bez toho seznamu aspoň nemusím řešit odstraňování odrážek

* nastavení vzdálenosti položek v menu v CSS:
  ``` css
  .header {
  display: flex;
  justify-content: space-between;
  }

  .menu {
  display: flex;
  gap: 30px;
  }

  .menu__link {
  display: inline-block;
  padding: 50px;
  }
  ```
  * díky tomuhle klikám na větší oblast
  * je to ale míň stabilní design, protože se to rozbije při zmenšení okna třeba na dva řádky - tohle pořeší `.menu {display:flex;}`

  * časem se naučíme responzivní design, aby se to líp přizpůsobilo podle velikosti displeje (budou různý stylesheets pro každou velikost displaye)

___

* pročíst si **metodologii BEM** na [Vzhůru dolů](https://www.vzhurudolu.cz/prirucka/bem)

* tohle je dobrý umět, protože pak mému CSS budou rozumět další lidi, je to jednotný způsob pojmenování
* ale jsou i další metodologie kdyžtak (je fajn se rozhodnout pro jednu konkrétní a podle té pak jet)
* https://getbem.com/


* the most popular sales platform... tady v ty casti je mozna snadnejsi použít nějaký "transform" na to, abych posunula ty černé ikonky po těch barevných kolečkách

> doposlechnout si pak nahrávku ze soboty případně
