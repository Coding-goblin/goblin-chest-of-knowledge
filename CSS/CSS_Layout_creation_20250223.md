# Layout creation

## Good practices

* before starting to code, *plan the project*
  * create mock-ups (low fidelity): pencil & paper or programs like:
    * [x] [Pencil project](https://pencil.evolus.vn/)
    * [x] [AdobeXD](https://www.adobe.com/products/xd.html)


často chceme některé vlastnosti úplně vynulovat, nebo normalizovat na lepší hodnoty - například:
* {
   margin: 0;
   padding: 0;
}
je celá řada uznávaných resetů v CSS, například: https://html5doctor.com/html-5-reset-stylesheet/


trochu modernější způsob je ne reset, ale normalize na nějaké chytřejší hodnoty - například podle tohohle: https://necolas.github.io/normalize.css/
podle něj to není nutné na každý projekt, ale můžeme to potřebovat


types of layouts - fully centered, centered content - a k tomu doporučení


first steps
budu mít vždycky sekci, v té container, v tom potom objekty, které pomocí flexboxu budou různě uspořádané
ad workshop & projekt day 4:

nekopírovat vlastnosti z návrhu, většinu z nich vůbec nemusím nastavovat - například souřadnice apod


když si v návrhu ve vývojářském módu kliknu na věc, uvidím její vlastnosti


ale opět ne všemu je třeba třeba nastavovat jeho velikost - naopak pokud to jde, tak to nechci nastavovat


když mám nějaký prvek nakliknutý a ukážu myší na vedlejší, uvidím vzdálenost v px, což naopak využiju
