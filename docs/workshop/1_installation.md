# Telepítés és konfiguráció

A Git-nek vannak grafikus kliens-ei is, de ahhoz, hogy igazán
megérthessük konzolt fogunk használni.  
Továbbá azt is hozzátenném, hogy a grafikus alkalmazások nem tudják lefedni a Git összes rendelkezésre álló funkcióját
és néha nagyon lebutítják a felhasználó felé, ezért
nem is lehet feltétlen megtanulni a működését grafikus
alkalmazásból.

## Telepítés

### Windows

Elég egyszerű a telepítés.
Felmész a [https://git-scm.com/download/win](https://git-scm.com/download/win) címre és az ott lévő .exe fájlt letöltöd.
A telepítőben a beállításokkal nem kell foglalkozni, az
alapértelmezett mindenhol jó lesz.

Esetleg később ha szeretnél egy grafikus felület, akkor a Github
Desktop is egy jó opció. Ez magába foglalja mind a konzolos,
mind az asztali programot.
[https://desktop.github.com/](https://desktop.github.com/)

### MacOS

Nincs sok tapasztalatom vele, de ha megpróbálod konzolból
kiadni a  
`git --version`  
parancsot, akkor fel fogja ajánlani,
hogy telepítsd. Itt kapsz egy telepítőt és az alapértelmezett
beállításain végig mész.

### Linux

Talán a legtriviálisabb. Attól függően milyen disztrót használsz
letölthető a megfelelő csomagkezelővel.  
RPM alapúak (Fedora, RHEL, CentOS, ...):  
`sudo dnf install git`  
Debian alapúak (Ubuntu, ...):  
`sudo apt install git`

Többi Disztróra:
[git-scm.com/download/linux](https://git-scm.com/download/linux)

[Előző](intro/3_history) | [Következő](workshop/2_basics)
