# Git története

Még mielőtt belemerülnék a git telepítésébe, használatába,
szeretnék némi sztorizást is megejteni.

Annó a Linux kernel fejlesztése során okozott nagy fejtörést
az egész verziókezelés megoldása.
1991-től 2002-ig, tehát 11 éven át patchekben
(pl.: e-mailben elküldött szöveg a módosításokkal)
és tömörített fájlokban küldözgették a verziókat a fejlesztők.
Aztán 2002-től egy zárt licenszű verzió kezelőre, a
[BitKeeper](http://www.bitkeeper.org/)-re váltottak.

Ezt a Linux fejlesztői ingyen használhatták egészen 2005-ig,
mikorra annyira elromlott a kapcsolat a fejlesztők és a cég
között, hogy elvették tőlük a licenszt.
Az i-re a pontott az tette le, mikor az egyik
kernel fejlesztő
[reverse engineer-elte](https://lwn.net/Articles/132938/)
a BitKeeper-t.  
Ekkoriban
[Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds)
úgy döntött, hogy egy új megoldást kell találnia, mely

- Gyors
- Egyszerű
- Támogatja a többszálú fejlesztést
- Teljesen elosztott
- Nagy projekteket is képes kezelni (pl.: Linux kernel)

Így hát megírta a [Git-et](https://en.wikipedia.org/wiki/Git), mely a mai napig a legelterjedtebb, leggyorsabb és
legkényelmesebb verzió kezelő rendszerünk.

[Előző](intro/2_versioning) | [Következő](workshop/1_installation)
