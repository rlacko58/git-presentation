# Branch-ek, elágazás

#### Mik azok a branch-ek?

Talán legegyszerűbb, ha előbb készítünk egyet.

Az előző témakörben átneveztünk egy fájlt, de
nem commitoltuk el.
Ezt ne tegyük meg, hanem készítsünk egy ágat onnan,
ahol jártunk mondjuk azzal a névvel, hogy `atnevezes`.

Először nézzük meg, mi a helyzet jelenleg a
`git status` paranccsal.

```
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	renamed:    gyumolcskosar -> gyumolcskosar.txt
```

Látható, hogy a `master` nevű ágon vagyunk.
Ezt képzeljük el úgy, mint egy fa törzse, melyből
le ágazunk.
Nézzük meg jelenleg milyen branch-ek vannak a repo-nkban
a `git branch` paranccsal.

```
$ git branch

* master
```

Látható, hogy még csak egy van.
Ha a `git branch <új branch neve>` parancsot kiadjuk, akkor
készül egy új ág, de arra figyeljünk, hogy ilyenkor még nem
megyünk át rá.
Nézzük is meg, hogy tényleg elkészült a `git branch` paranccsal.

```
$ git branch

  atnevezes
* master
```

Itt a `*` azt jelöli, hogy melyiken vagyunk jelenleg.
Ha kiadnánk a `git status` parancsot, akkor is láthatnánk
melyik ágon vagyunk.

Gyerünk át az új ágra.
Ehhez a `git checkout <ág neve>` parancsot tudjuk használni.

```
$ git checkout atnevezes
D	gyumolcskosar
A	gyumolcskosar.txt
Switched to branch 'atnevezes'
```

Itt azt is látjuk azonnal, hogy a módosításainkkal nem történt
semmi.
Ezek jelenleg még `staged`-be vannak és ha elmentjük őket
a commital, akkor az épp jelenlegi ágra fogja
beilleszteni őket.

Nézzük meg `git status`-al, hogy mi a helyzet.

```
$ git status
On branch atnevezes
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	renamed:    gyumolcskosar -> gyumolcskosar.txt
```

Látszik is, hogy az `atnevezes` ágon vagyunk.
Mentsük el ide a módosításaink  
`Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz`  
üzenettel

```
$ git commit -m "Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz"
[atnevezes 7de1c94] Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename gyumolcskosar => gyumolcskosar.txt (100%)
```

Ezt követően ha kiadjuk a `git log --graph --oneline` parancsot, akkor
meg is láthatjuk a jelenlegi helyzetet a mi fánkban:

```
* 7de1c94 (HEAD -> atnevezes) Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz
* 80560db (master) jegyzeteim ignorálása
* 30bf35d Hoztunk egy hordót
* b677a86 Raktam bele egy körtét
* c45abc3 Készítettem egy gyümölcskosarat
```

#### Hogy épül fel?

Először is, ahhoz hogy tudja a git melyik mentéspont után
melyik jön, mutatókat használ ehhez az ábrához
hasonló módon:

<div style="text-align:center"><img src="workshop/img/commits-and-parents.png" alt="Commits and parents" /></div>

A mentéspontjaink az előzőre mutatnak.
Ahogy láthattuk az előző fejezetben, a mentéspontok pedig
mutatnak a megfelelő fájlokra amiket épp módosítottunk.

Vizsgáljuk meg a jelenlegi repo-nkat, hogy pontosan
mi merre is van.

<div style="text-align:center"><img src="workshop/img/tree_with_one_branch.png" alt="Tree with one branch" /></div>

A mentéspontok képesek mutatni az előzőre, ezt láthatjuk
az ábrán.
Továbbá azt is látjuk, hogy van még 3 pointerünk.
Az egyik `master`, ez az ami a legfrissebb mentésre
mutat a master ágon.
Ugyanez igaz az `atnevezes` ág esetében, de oda még mutat
egy `HEAD` mutató is.
Ez az a mutató ami azt jelzi, hogy hol vagyunk mi jelenleg.
Mikor egy új mentést hozunk létre, akkor az a mentés ide
fog mutatni.
Ezért is kellett figyelni az előzőnél, hogy mentés
előtt átváltsunk a megfelelő ágra.

#### Ágak közti mozgás

Még mielőtt vissza mennénk a master-re, nézzük meg milyen
fájljaink vannak az `ls -al` -el.

```
$ ls -al
total 20
drwxrwxr-x  3 rlacko rlacko 4096 okt    8 21:07 .
drwxrwxr-x 12 rlacko rlacko 4096 okt    8 16:49 ..
drwxrwxr-x  8 rlacko rlacko 4096 okt    9 15:01 .git
-rw-rw-r--  1 rlacko rlacko   15 okt    8 20:43 .gitignore
-rw-rw-r--  1 rlacko rlacko   11 okt    8 17:29 gyumolcskosar.txt
-rw-rw-r--  1 rlacko rlacko    0 okt    8 20:09 hordo
-rw-rw-r--  1 rlacko rlacko    0 okt    8 20:42 jegyzeteim.txt
```

Egy pár extra adatot is kapunk, de nekünk most a
`gyumolcskosar.txt` a lényeges.

Most gyerünk vissza a `master` ágra a `git checkout <ág neve>`
paranccsal.

```
$ git checkout master
Switched to branch 'master'
```

Most adjuk ismét ki az `ls -al` parancsot.

```
$ ls -al
total 20
drwxrwxr-x  3 rlacko rlacko 4096 okt    9 15:24 .
drwxrwxr-x 12 rlacko rlacko 4096 okt    8 16:49 ..
drwxrwxr-x  8 rlacko rlacko 4096 okt    9 15:24 .git
-rw-rw-r--  1 rlacko rlacko   15 okt    8 20:43 .gitignore
-rw-rw-r--  1 rlacko rlacko   11 okt    9 15:24 gyumolcskosar
-rw-rw-r--  1 rlacko rlacko    0 okt    8 20:09 hordo
-rw-rw-r--  1 rlacko rlacko    0 okt    8 20:42 jegyzeteim.txt
```

Látható, hogy vissza léptünk arra az állapotra mikor még
nem változtattunk rajta.

Ez elképesztően jó, ugyanis két teljesen külön álló munkát
így elkülönítöttünk egymástól!

Írjuk a `gyumolcskosar` fájl végére, hogy `szolo` és mentsük el
azzal az üzenettel, hogy `Tettem a kosaramba szőlőt`.

```
$ git commit -m "Tettem a kosaramba szőlőt"
[master 932cbeb] Tettem a kosaramba szőlőt
 1 file changed, 1 insertion(+)
```

Most újra kiadva a `git log --graph --oneline` parancsot,
már érdekesebb eredményt láthatunk.

```
$ git log --graph --oneline

* 15719cf (HEAD -> master) Tettem a kosaramba szőlőt
* 80560db jegyzeteim ignorálása
* 30bf35d Hoztunk egy hordót
...
```

Mégis hova tünt a másik águnk?
A git nem fogja alapból jelezni nekünk azt amit nem lát
a jelenlegi mentéstől vissza menve.
Hogy lássuk a másik ágat is, tegyük hozzá a `--all` kapcsolót.

```
$ git log --graph --oneline --all

* 15719cf (HEAD -> master) Tettem a kosaramba szőlőt
| * 7de1c94 (atnevezes) Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz
|/
* 80560db jegyzeteim ignorálása
* 30bf35d Hoztunk egy hordót
...
```

Na így már látjuk a másik ágat is.
Még vizuálisabban jelenleg így állunk:

<div style="text-align:center"><img src="workshop/img/tree_awesome.png" alt="Tree with one branch" /></div>

Menjünk vissza az `atnevezes` ágra és módosítsunk picit.

```
$ git checkout atnevezes
```

Majd pedig nevezzük át a `hordo`-t `hordo.txt`-re.

```
$ mv hordo hordo.txt
```

És ezt is mentsük el

```
$ git add .
$ git commit -m "Adtam kiterjesztést a hordónak"
```

Egy ismételt `git log`-al láthatjuk is a fánkat.

```
$ git log --graph --oneline --all

* 9dcfc79 (HEAD -> atnevezes) Adtam kiterjesztést a hordónak
* 7de1c94 Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz
| * 15719cf (master) Tettem a kosaramba szőlőt
|/
* 80560db jegyzeteim ignorálása
```

#### Mergelés

Az egyik legfontosabb dolog az ágak létrehozása után, hogy
betudjuk olvasztani az águnk valahova.
Az lenne a feladat, hogy a `master`-re beillesszük az
`atnevezes` ágon végzett módosításaink.  
Ehhez a `git merge <branch neve>` parancsot használhatjuk.
A lényege, hogy ez a parancs azt az ágat amit kiválasztottunk
megpróbálja beolvasztani oda ahol épp a `HEAD` mutatónk van.

Gyerünk is át a `master` ágra.

```
$ git checkout master
Switched to branch 'master'
```

Ezután pedig mergeljük át a `master`-re az `atnevezes` ágat.

```
git merge atnevezes
```

Ekkor meg fog nyílni a szövegszerkesztőnk, ugyanis egy
új mentéspontot fogunk készíteni a `master águnkra`.
A feladott merge üzeneten nem kell módosítanünk, teljesen
jó úgy.

A merge lefut és láthatjuk mi is történt:

```
$ git merge atnevezes
Removing hordo
Merge made by the 'recursive' strategy.
 gyumolcskosar => gyumolcskosar.txt | 0
 hordo => hordo.txt                 | 0
 2 files changed, 0 insertions(+), 0 deletions(-)
 rename gyumolcskosar => gyumolcskosar.txt (100%)
 rename hordo => hordo.txt (100%)
```

Most egy újabb `git log`-al ezt láthatjuk:

```
*   366140d (HEAD -> master) Merge branch 'atnevezes'
|\
| * 9dcfc79 (atnevezes) Adtam kiterjesztést a hordónak
| * 7de1c94 Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz
* | 15719cf Tettem a kosaramba szőlőt
|/
* 80560db jegyzeteim ignorálása
* 30bf35d Hoztunk egy hordót
```

Ez a bizonyos `merge commit` egyszerre mutat a két ág
tartalmára.

#### Merge conlict

Mi van akkor ha ketten egyszerre ugyanazt változtatjuk?  
Úgy döntöttünk, hogy pálinkát szeretnénk főzni józsival,
szóval a hordóba teszünk ízlés szerint valami
gyümölcsöt.
Gyerünk át a saját águnkra, de egy paranccsal.
Ezt a `-b` kapcsolóval tudjuk
elérni a `git checkout` mellett.

```
$ git checkout -b lacko
```

Most egy újabb `git branch` kiadásával már látható, hogy
3 ággal rendelkezünk.

```
  atnevezes
* lacko
  master
```

Tegyünk bele a `hordo.txt`-be egy nekünk tetsző gyümölcsöt.

```gyumolcs.txt
korte
```

Ezt mentsük is el, de most picit csaljunk és ne rakjuk
`staged`-be, hanem azonnal mentsük el `-a` kapcsolót használva.

```
$ git commit -a -m "Raktam a hordóba körtét"
```

Úgy döntöttünk a szomszéd Józsi, hogy ugyanebbe a hordóba
körtét szeretne rakni. Menjünk át a master ágra és
tegyük meg ott.

Azt mondta a szomszéd, hogy szilvát szeretne beletenni, szóval:

```hordo.txt
szilva
```

Ezt is mentsük el.

```
$ git commit -a -m "Raktam szilvát a hordóba"
```

Most ha megnézzük a gráfunkat újra, akkor láthatjuk is,
hogy mi a helyzet:

```
$ git log --graph --oneline --all

* 41f1c05 (HEAD -> master) Raktam szilvát a hordóba
| * 6d6d1ac (lacko) Raktam a hordóba körtét
|/
*   366140d Merge branch 'atnevezes'
|\
| * 9dcfc79 (atnevezes) Adtam kiterjesztést a hordónak
| * 7de1c94 Hozzáadtam a txt kiterjesztést a gyümölcskosar fájlhoz
* | 15719cf Tettem a kosaramba szőlőt
|/
* 80560db jegyzeteim ignorálása
* 30bf35d Hoztunk egy hordót
...
```

Na és akkor most mergeljünk be a master-re a módosításaink.

```
$ git merge lacko
Auto-merging hordo.txt
CONFLICT (content): Merge conflict in hordo.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Oh! Merge conlict keletkezett.  
Nem kell megijedni, nem a világ vége és a git segít ahol tud.

Az a helyzet, hogy egyszerre ugyanazt a sort módosítottuk és a git nem tudta
eldönteni mit tegyen. Megtartsa az egyiket vagy mindkettőt? Mégis mi legyen?

Ezeket a kérdéseket nekünk kell megválaszolnunk.

Először nézzük meg, mit a státusz.

```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   hordo.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Az van, hogy ilyenkor a git beírja a fájlunkba mind a két branch változásait
és utána azt várja tőlünk, hogy átalakítsuk a fájlt, majd pedig azt elmentsük.
Szóval nem kell megijedni, csak átírjuk mi kell nekünk és mentünk egyet.

Nézzük meg mi van a fájlunkban.

```hordo.txt
<<<<<<< HEAD
szilva
=======
korte
>>>>>> lacko
```

Mit is jelent ez? Ketté szedte az bejövő adatokat a git arra ahová mergeltünk
(`HEAD`) és amit mergeltünk (`lacko`). Köztük pedig egy sor "======="-t láthatunk.
Itt már csak átírjuk a fájlt ahogy szeretnénk, hogy kinézzen.

```hordo.txt
korte
```

Nézzük meg mi a státusz ezután:

```
git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   hordo.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Továbbra is azt írja, amit az imént. Amíg nem commitoljuk a módosításunk,
addig ezt is fogja. Hát akkor mentsünk.

```
git commit -a -m "lacko branch mergelve és konflikt megoldva"
```

Nézzük meg hogy néz ki a gráfunk ezután.

```
*   7341274 (HEAD -> master) lacko branch mergelve és konflikt megoldva
|\
| * 6d6d1ac (lacko) Raktam a hordóba körtét
* | 41f1c05 Raktam szilvát a hordóba
|/
*   366140d Merge branch 'atnevezes'
...
```

Tehát most a merge commitunk egyben a konfliktusok megoldását is tartalmazza.
Nem elképesztő? Innentől már csak együtt kell dolgozni, margeknél a konfliktusokat
megoldani.

Semmi extra effortot nem fog igényelni egy hasonló elvégzése.

[Előző](workshop/2_basics?id=alapok) |
[Következő](remote/1_basics)
