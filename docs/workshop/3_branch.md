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

[Előző](workshop/2_basics) | [Következő](remote/1_basics)
