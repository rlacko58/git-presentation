# Telepítés és konfiguráció

A Git-nek vannak grafikus kliens-ei is, de ahhoz, hogy igazán
megérthessük konzolt fogunk használni.  
Továbbá azt is hozzátenném, hogy a grafikus alkalmazások nem tudják lefedni a Git összes rendelkezésre álló funkcióját
és néha nagyon lebutítják a felhasználó felé, ezért
nem is lehet feltétlen megtanulni a működését grafikus
alkalmazásból.

## Telepítés

#### Windows

Elég egyszerű a telepítés.
Felmész a [https://git-scm.com/download/win](https://git-scm.com/download/win) címre és az ott lévő .exe fájlt letöltöd.
A telepítőben a beállításokkal nem kell foglalkozni, az
alapértelmezett mindenhol jó lesz.

Esetleg később ha szeretnél egy grafikus felület, akkor a Github
Desktop is egy jó opció. Ez magába foglalja mind a konzolos,
mind az asztali programot.
[https://desktop.github.com/](https://desktop.github.com/)

#### MacOS

Nincs sok tapasztalatom vele, de ha megpróbálod konzolból
kiadni a  
`git --version`  
parancsot, akkor fel fogja ajánlani,
hogy telepítsd. Itt kapsz egy telepítőt és az alapértelmezett
beállításain végig mész.

#### Linux

Talán a legtriviálisabb. Attól függően milyen disztrót használsz,
letölthető a megfelelő csomagkezelővel.  
RPM alapúak (Fedora, RHEL, CentOS, ...):  
`sudo dnf install git`  
Debian alapúak (Ubuntu, ...):  
`sudo apt install git`

Többi Disztróra:
[git-scm.com/download/linux](https://git-scm.com/download/linux)

## Konfiguráció

### Hol vannak a konfigok?

Legegyszerűbben ezzel a paranccsal lehet a jelenlegi konfig
fájlok helyzetét kiírni:  
`git config --list --show-origin`

Látható, hogy a konfigok valamilyen config fájlba menti
a fájlrendszerben elszórva. Erről egy pár táblázat:

##### Windows

| Scope    | Hely                         | Fájlnév         |
| -------- | ---------------------------- | --------------- |
| Rendszer | mingw32\etc vagy mingw64\etc | gitconfig       |
| Globális | C:\Users\\<felhasználónév>   | .gitconfig      |
| Lokális  | Git repo .git mappája        | config          |
| Worktree | Git repo .git mappája        | config.worktree |
| Portable | C:\ProgramData\Git\          | config          |

##### Linux (Ubuntu)

(MacOS ehhez hasonló)

| Scope    | Hely                  | Fájlnév         |
| -------- | --------------------- | --------------- |
| Rendszer | /etc                  | gitconfig       |
| Globális | ~                     | .gitconfig      |
| Lokális  | Git repo .git mappája | config          |
| Worktree | Git repo .git mappája | config.worktree |

Nálam a konfig a home mappámban például így néz ki:

```
$ cat ~/.gitconfig
[user]
        email = rlacko99 [AT] gmail.com
        name = Rafael László
```

### Állítsuk be a dolgokat magunknak

##### Saját adatok

Először is a saját adataink:

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe [AT] example.com
```

##### Szövegszerkesztő

Állítsuk be a szövegszerkesztőnket.
Erre főként a mentéspontokhoz tartozó üzenet megírásához van
szükség.

```
$ git config --global core.editor nano
```

vagy például notepad++-ra windows-on:

```
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

##### Jelenlegi konfig

Nézzük meg az összes beállításunk:

```
$ git config --list
user.email=rlacko99 [AT] gmail.com
user.name=Rafael László
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
...
```

Ha pedig csak egy adottat szeretnénk:

```
$ git config user.name
Rafael László
```

[Előző](intro/3_history?id=git-története) |
[Következő](workshop/2_basics?id=alapok)
