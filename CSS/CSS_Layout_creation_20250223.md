# Layout creation

## Good practices

### Plan the project before starting to code

  * create mock-ups ([low fidelity](https://www.google.com/search?q=low+fidelity&newwindow=1&source=lnms&sa=X&ved=0ahUKEwiwsLmS0srfAhWGDywKHXkvDMEQ_AUIDigB&biw=1680&bih=869&udm=2)): pencil & paper or programs like:
    * [x] [Pencil project](https://pencil.evolus.vn/)
    * [x] [AdobeXD](https://www.adobe.com/products/xd.html)
  * plan for viewing the website on different devices and the kind of users will view it
  * check for inspiration online - similar websites
  * the layout image can be created with tools:
    * [x] [Sketch](https://www.sketchapp.com/)
    * [x] [AdobeXD](https://www.adobe.com/pl/products/xd.html)
    * [x] [Figma](https://www.figma.com/)
    * [x] [Framer](https://framer.com/)

### Reset or normalize

* často chceme některé vlastnosti úplně vynulovat, nebo normalizovat na lepší hodnoty - například:

  ``` css
  {
    margin: 0;
    padding: 0;
  }
  ```

* je celá řada uznávaných resetů v CSS, například: 
  * [HTML5 Reset Stylesheet](https://html5doctor.com/html-5-reset-stylesheet/)

* trochu modernější způsob je ne reset, ale **normalize** na nějaké chytřejší hodnoty - například podle tohohle:
  * [Normalize.css](https://necolas.github.io/normalize.css/)

* podle něj to není nutné na každý projekt, ale můžeme to potřebovat

* ==co budu chtít nejspíš nastavit pokaždý je `box-sizing`:==
  ``` css
  * {
    box-sizing: border-box;
  }
```

> **sanitize** asi úplně často nevyužiju - kombinuje to normalize a k tomu to přidává další styly, které by se mi jakože mohly hodit, opravuje to bugs atd.:
> [Sanitize.css](https://github.com/csstools/sanitize.css)

* **types of layouts:**
  * fully centered: the content and individual sections are centered
  * centered content: the sections are 100% wide and the content is centered (pozad9 sekcí zabírá celou šířku, ale jejich obsah jen velikost containeru a je to centrované)


### First steps

* budu mít vždycky sekci, v té bude `<div>` s class `.container`, v tom potom další objekty, které pomocí flexboxu budou různě uspořádané

> ad workshop & projekt day 4:

> * nekopírovat vlastnosti z návrhu, většinu z nich vůbec nemusím nastavovat - například souřadnice apod
> * když si v návrhu ve vývojářském módu kliknu na věc, uvidím její vlastnosti
> * ale opět ne všemu je třeba třeba nastavovat jeho velikost - naopak pokud to jde, tak to nechci nastavovat
> * když mám nějaký prvek nakliknutý a ukážu myší na vedlejší, uvidím vzdálenost v px, což naopak využiju
