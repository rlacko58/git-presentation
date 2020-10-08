# Verziókezelés

Mi is az a verzió kezelés?  
Talán a legegyszerűbb egy példán keresztül szemléltetni.
Tegyük fel egy docx fájlt szerkesztünk és ezt hetente frissítve
rendszeresen el kell küldenünk emailben valakinek.
Ilyenkor különböző verziók keletkeznek a fájlból és ezeket
a postafiókunkból könnyedén előtudjuk szedni.

Na ugyanazen a példán tovább mehetünk.
Mi van ha lokális kezdjük ezeket a fájlokat tárolni?
Gondolom mindenki találkozott már hasonló fájlnevekkel:

```
elso_beadasom.docx
masodik_beadasom.docx
masodik_beadasom (1).docx
masodik_beadasom (1) Javított.docx
masodik_beadasom (1) Javított másolata.docx
asd.docx
asdasd.docx
asdasdasdasd.docx
```

Ha ügyesek vagyunk még mappákat is készítünk és dátumot is hozzá cimkézünk.

```
2020/
  09/
    13/
      elso_beadasom.docx
    19/
      masodik_beadasom_felkesz.docx
    20/
      masodik_beadasom.docx
      masodik_beadasom_javitott.docx
      masodik_beadasom_vegleges.docx
    ideiglenes/
      asd.docx
      asdasd.docx
      asdasdadsads.docx
```

Persze az operációs rendszer képes dátum alapján rendezni,
és megspórol nekünk pár lépést, de mi van ha ezt valakinek
el is szeretnénk küldeni? Mi van ha a módosítás dátuma közben
módosul? Hogyan biztosítjuk, hogy közben nem sérülnek a fájlok?

Ezekre adnak megoldást a különböző verziókezelő rendszerek.  
Ezeknek egy dolguk van, fájljaink számon tartása, mint amit
kézileg elvégeztünk. Továbbá arra is lehetőséget ad, hogy
ha valamit kitörlünk, azt visszanyerhessük.

### Milyen verzió kezelő rendszereink lehetnek?

#### Helyi

Erre a fenti példa legjobb példa.
Vannak különböző verziói a fájlunknak és ezeket valamilyen
adatbázisban rögzítjük.

<div style="text-align:center"><img src="intro/img/vcstype_local.png" alt="Local Version Control Systems" /></div>

Ilyen az [RCS](https://www.gnu.org/software/rcs/)

### Központosított

Ez már egy fokkal okosabb.
A különböző verziókat a központi szerverre rakjuk fel és
onann szedjük le.
Például van egy Fájlszerverünk amit minden gépről elérnek
az emberek és oda dolgoznak közösen.  
Érezhető probléma, hogy így ha meghal a központi szerver,
akkor mindent elvesztünk.
Továbbá probléma lehet, hogy egymás munkáját felülírjük,
szerencsére egy jó rendszernél erről értesítést kapunk,
hozzá és nem felülírjuk a módosításaink.

<div style="text-align:center"><img src="intro/img/vcstype_central.png" alt="Centralized Version Control Systems" /></div>

### Megosztott

Na és itt lépünk be ma is használt Git világába.
Ennél a megoldásnál már az a trükk, hogy mindenkinek meg van
a teljes projekt az összes verziójával. Felmerül, hogy na
de akkor honnan szedjük le a legújabb verziót?
Különböző megoldások léteznek, például a fejlesztők a
módosításokat azonnal megosztják egymással (pl.: p2p Torrenthez hasonló módon) vagy
kijelölnek egy központi szervert amivel mindenki szinkronban
van.
Ilyen például a Github vagy a Gitlab.  
Csak megjegyzem, de akár a módosításokat Emailben is ellehet
küldeni és a szoftver automatikusan megcsinálja a többi a mi részünkön.

<div style="text-align:center"><img src="intro/img/vcstype_distributed.png" alt="Distributed Version Control Systems" /></div>

[Előző](intro/1_intro) | [Következő](intro/3_history)
