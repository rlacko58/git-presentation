# Alapok

##### Mit tegyek ha elakadok?

Git-ben van egy beépített git help parancs:

```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

Például a config-al kapcsolatos dolgok:

```
$ git help config
```

De ha csak egy gyors áttekintésre van szükséged:

```
$ git config -h
usage: git config [<options>]

Config file location
--global use global config file
--system use system config file
--local use repository config file
--worktree use per-worktree config file
-f, --file <file> use given config file
--blob <blob-id> read config from given blob object

Action
--get get value: name [value-regex]
...

```

#### Repository

A Git repository egy olyan mappa, mely Git verziókezelés alatt
áll.
Tehát az a mappa, ahol van egy .git mappa, az egy git repo.

```
$ ls -al
total 20
drwxrwxr-x 4 rlacko rlacko 4096 okt 8 12:59 .
drwxrwxr-x 3 rlacko rlacko 4096 okt 7 12:04 ..
drwxrwxr-x 5 rlacko rlacko 4096 okt 7 12:46 docs
drwxrwxr-x 8 rlacko rlacko 4096 okt 8 16:39 .git
-rw-r--r-- 1 rlacko rlacko 17 okt 7 12:09 README.md

```

Csináljunk egy sajátot!

Hozzunk létre egy tetszőleges projekt mappát és lépjünk bele

```
$ mkdir projektem
$ cd projektem

```

Nézzük meg, hogy mi a helyzet a git-el.  
Ehhez a `git status` parancsot tudjuk használni.
Ez a parancs a jelenlegi git repo-nkról képes
információkat kiírni.

```
$ git status
fatal: not a git repository (or any of the parent directories): .git

```

Teljesen üres a mappa, még nincs verzió kontroll alatt.
`ls -al` -el tudjuk is ellenőrizni, hogy nincsen `.git`
mappa.

Tegyük verziókontroll alá a `git init` paranccsal.
Ez beállítja nekünk a `master` branchet és más alap dolgokat.
A branch-ről még később beszélek, de azt jegyezzük meg addig,
hogy ide kerülnek a mentéspontjaink egymás után sorban, mint
egy fa ága.

<div style="text-align:center"><img src="workshop/img/basic-branching.png" alt="Git branch" /></div>

```
$ git init .
Initialized empty Git repository in .../projektem/.git/

```

Ha megnézzük, mostmár van egy .git mappánk

```
$ ls -al
total 12
drwxrwxr-x 3 rlacko rlacko 4096 okt 8 16:52 .
drwxrwxr-x 12 rlacko rlacko 4096 okt 8 16:49 ..
drwxrwxr-x 7 rlacko rlacko 4096 okt 8 16:52 .git

```

Ebben van minden adatunk a git repo-val kapcsolatban.
Gyakorlatilag mikor egy távoli git repo-t leszedünk, akkor
ezt a mappát kapjuk meg. Ebből ezután a git alkalmazásunk
előállítja nekünk a master ág legutóbbi mentéspontját.

#### Mentéseink tárolása

Egy logikus megoldásnak tűnhet, hogy az egyes fájlok közt
csak az eltéréseket tároljuk. Például:

```gyumolcskosar
1. mentés:
alma

2. mentés:
[1.mentés 1.sora]
alma
```

Ez annyiból jó, hogy a tárhellyel spórol, viszont nem gyors.
Képzeljünk el egy 1000 commit-ból álló repo-t.
Mi történik mikor a legutóbbit lekérjük?

Erre egyik megoldás más verziókezelőknél, hogy optimalizálnak
és mondjuk 50 mentésenként csinálnak egy teljes mentést, stb.

A Git ezt úgy oldja meg, hogy egy mentés az összes fájlt
egy az egybe lemásolja.  
Például egyik mentés alkalmával feltöltök egy 1gb-os videót,
majd a következő mentéskor pár bitet átírok, akkor már
2gb-nyi videó lesz a repóban és ha utána egy másik mentésel
ki is töröljük, az attól még megmarad.
Persze utólag tudunk olyat, hogy visszmegyünk és kitöröljük
az adott mentésből.

Oké, tehát például mappákba bemásolja a mentéseinket valamilyen
módon, de mégis hova?

```
$ tree .git/objects/
.git/objects/
├── 13
│   └── 46dac32baea8123272f60b6a04227fe3872bdb
...
├── e6
│   └── 9de29bb2d1d6434b8b29ae775ad8c2e48c5391
├── f4
│   └── 5a6ff55496414b573e97064e998f065182eeee
├── info
└── pack
```

Mégis mik ezek?  
A Git tömörítve és hashelve tárolja a fájlokat.
A hashelésnek annyi a lényege, hogy a fájl teljes tartalmán
átfuttatunk egy algoritmust, mely ezután kiköp egy 0-9a-z
szöveget és ha akár 1 bit-et is változtatunk egy fájlon, akkor
más lesz a hash-e.  
Tehát ha valaki elcommitol valamit, akkor azt már más nem tudja
megváltoztatni anélkül, hogy változna a hash.
Így tudjuk biztosítani a teljes konzisztenciát az elosztott
repo-k között.

Hash-t láthatunk például a mentéspontjainkon, fájljainkon, stb.

Például itt egy ábra, hogy hogyan társítja a git egy
commit-hoz a megfelelő fájlokat egy kis pointer mágia keretében.

<div style="text-align:center"><img src="workshop/img/commit-and-tree.png" alt="Commit and Tree" /></div>

Ennél jobban nem megyek bele a témába, de érdekes olvasmány.

#### Változtatások mentése

Hogyan néz ki egy mentés?

```
commit c45abc3d64c7840b4088b77d5a60d02198a78854
Author: Rafael László <rlacko99 [AT] gmail.com>
Date:   Thu Oct 8 17:19:28 2020 +0200

    Készítettem egy gyümölcskosarat
```

Van egy commit-nak `commit hash`-e, `készítője`,
`létrehozási dátuma`, és `commit üzenete`

Na de hogyan készítünk egy mentéspontot?

A fájljainknak különböző állapotai létezhetnek egy
repo-ban.

- Untracked: Ami még nincs verziókontroll alatt
- Staged: Ami már verziókontroll alatt van de még nem
  mentettük el
- Unmodified: Amit már elmentettünk és azóta nem változott
- Modified: Az a fájl, ami már verziókontroll alatt van
  és változott. Ezt utánna szintén Stage-be tudjuk tenni

Nézzük meg ezen az ábrán és egy példa projekten keresztül:

<div style="text-align:center"><img src="workshop/img/git_lifecycle.png" alt="Git Life cycle" /></div>

Mi a jelenlegi helyzet a frissen inicializált repo-ban?

```
$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

Láthatjuk, hogy még nincsenek mentéspontjaink, nincs mit
elmentenünk és egyben a git próbál segíteni, hogy hogyan
tudunk stagelni valamit.
Ezt több helyen is megfigyelhetjük a git-ben.
Próbál segíteni ahol csak tud.

Vegyünk fel egy új fájlt.

```
echo 'alma' > gyumolcskosar
```

_Itt az `echo` egy olyan parancs volt, mely kiírta nekünk a
terminálra azt, hogy "alma", de átirányítottuk egy fájlba
a `>` operátorral és mivel nem létezett a fájl, azt létre is
hozta._ - Linux magic #01

Újra megvizsgálva a status-t, látni fogjuk, hogy megjelent, de
még nincs verziókontroll alatt.

```
$ git status
...
Untracked files:
(use "git add <file>..." to include in what will be committed)
gyumolcskosar

nothing added to commit but untracked files present (use "git add" to track)
```

Ahhoz, hogy git alá helyezzük, a `git add <fájl>` parancsot
fogjuk kiadni.
_Ezek a parancsok mind képesek rá, hogy
Unix-os módon több fájlra is kiadhatóak legyenek.
Például a `<fájl>` lehet `*.jpg`amivel minden .jpg fájlt kijelölünk a jelenlegi mappában"_ - Linux magic #02

```
$ git add gyumolcskosar
```

Ezután megjelenik, mint új fájl a git adatbázisában.

```
$ git status
...
Changes to be committed:
(use "git rm --cached <file>..." to unstage)
new file: gyumolcskosar
```

A fájlunk átkerült staged módba és ezt akár el is
tudjuk menteni.
A mentéshez a `git commit` parancsot tudjuk
használni.
`$ git commit`-ot kiadva megnyilik a beállított szövegszerkesztőnk és némi
információ a leendő mentéspontról.

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
# new file: gyumolcskosar
```

Láthatjuk amit a `git status` parancs írna ki és pár extra
segítséget a git-től.
A # -el kezdődő sorok kommentek, ezek nem fognak a commit
üzenetbe belekerülni.
Írjuk be az első sorba, hogy `Készítettem egy gyümölcskosarat`,
majd mentsük el a fájlt és zárjuk be a szövegszerkesztőt.

```
$ git commit
[master (root-commit) c45abc3] Készítettem egy gyümölcskosarat
1 file changed, 1 insertion(+)
create mode 100644 gyumolcskosar
```

A git mikor érzékelte, hogy bezártuk a fájlt, akkor abból
kiolvasta a sorokat, ignorálva a #-el kezdődőket és hozzáadta
a mentéspontunkhoz, mint üzenet.
Ezt láthatjuk is a visszajelzésben.
Továbbá megjelent pár további hasznos információ is.

Ismételten nézzük meg mi a helyzet a reponkban:

```
$ git status
On branch master
nothing to commit, working tree clean
```

Mostmár nem szól amiatt, hogy még nincsen mentésünk, továbbá
azt is írja, hogy még semmin nem változtattunk, nem tudunk
mit elmenteni.

Nézzük meg a további két állapotot is amiben lehet egy fájl.
Írjuk bele a gyumolcskosar fájlba, hogy `korte`

```gyumolcskosar
alma
korte
```

Ezután kiadva a `git status`-t:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   gyumolcskosar

no changes added to commit (use "git add" and/or "git commit -a")
```

Látható, hogy mostmár `modified` a fájlunk, mentsük is el,
de most picit másképp. A git és általában minden Unix
parancsnak áttudunk adni úgynevezett kapcsolókat.
Például `git commit -m <message>`.
Ez annyit spórol meg nekünk, hogy nem kell szövegszerkesztőt
megnyitnia a git-nek mikor új commit-ot készítünk,
hanem átrakjuk a parancsot olyan módba,
hogy várjon egy szöveget, mint mentéshez kapcsolódó üzenet.
Általában logikusak az egy betűs rövidítések:  
`-m: message, -a: all`, de tudunk hosszabb verziókat is
használni, mint `--message, --all`.

> Ne felejtsük el először stagelni a mentendő fájljaink egy
> `git add <fájl>` parancs kiadásával.

```
$ git add gyumolcskosar
$ git commit -m "Raktam bele egy körtét"
```

Készítsünk egy új fájlt `hordo` néven és nézzük mit ír ki
a `git status -s` parancs.

```
$ git status -s
?? hordo
```

Ezzel a kapcsolóval képesek vagyunk egy rövidített státuszt
lekérni. Stageljük majd pedig commitoljuk el
`"Hoztunk egy hordót"` üzenettel. A lépések közt nézzük meg
mit ír ki a `git status -s`.

Mi van akkor, ha valami fájlt nem szeretnénk verziókontroll
alá tenni? Gondolok például a build-ek mappáira vagy
egy IDE által generált fájlokra.

Hozzunk létre egy `.gitignore` fájlt és egy `jegyzeteim.txt`
fájlt.
Ezután adjuk ki a `git status` parancsot.
Láthatjuk, hogy a `jegyzeteim` fájl megjelent.

Most írjuk be a fájl nevét a `.gitignore`-ba.

```.gitignore
jegyzeteim.txt
```

Ezt követően adjuk ki újra a `git status` parancsot.  
Oh, eltünt a fájlunk! Pont ezt akartuk, mostmár nem fogja
figyelni a git ha ilyen fájlt készítünk.
Mostmár csak stageljük és mentsük el az új fájlunk.  
`git add .`, `git commit -m "jegyzeteim ignorálása"`

#### Fájlok mozgatása

A git semmi adatot nem tárol változásokról, csak
mentéseket készít, ahogy említettem.
Próbáljuk ki, hogy mi történik ha átnevezünk egy fájlt?

```
mv gyumolcskosar gyumolcskosar.txt
```

```
git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    gyumolcskosar

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	gyumolcskosar.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Látható, hogy az eredeti fájlt "töröltük" és egy új
fájlt vettünk fel a repo-ba. Érdekes, mi lenne ha stagelnénk
a változtatásokat `git add .` -al?

```
$ git add .
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	renamed:    gyumolcskosar -> gyumolcskosar.txt
```

Most meg már látszik, hogy csak átnevezés történt?

A Git képes rá, hogy felismerje a fájlokat és eldöntse, hogy
ugyanaz volt a kettő, tehát átnevezés, mozgatás, stb. történt.
De nem tudja követni a fájlt, csak látja, hogy jött egy új
fájl ami szinte ugyanaz, mint ami törölve lett nemrég.

#### Mentési előzmények

A Git egyik legjobb parancsa a `git log` a `git status` után és
szeretném ha kipróbálnád.

```
commit 80560db1f5a83496b80f1959fbcbae2ccfff320e (HEAD -> master)
Author: Rafael László <rlacko99 [AT] gmail.com>
Date:   Thu Oct 8 21:07:25 2020 +0200

    jegyzeteim ignorálása

commit 30bf35d36399e484b03090570e13cb95da92ab8b
Author: Rafael László <rlacko99 [AT] gmail.com>
Date:   Thu Oct 8 20:11:58 2020 +0200

    Hoztunk egy hordót

commit b677a8639193479157f7a576dffe0186b0dbe2c8
Author: Rafael László <rlacko99 [AT] gmail.com>
Date:   Thu Oct 8 17:33:07 2020 +0200

    Raktam bele egy körtét

commit c45abc3d64c7840b4088b77d5a60d02198a78854
Author: Rafael László <rlacko99 [AT] gmail.com>
Date:   Thu Oct 8 17:19:28 2020 +0200

    Készítettem egy gyümölcskosarat
```

Itt azt láthatjuk, hogy milyen mentéspontjaink vannak
és a hozzájuk tartozó dolgokat, mint a commit hash vagy
a üzenet.

Picit tegyük szebbé. Adjuk ki az előző parancsot a
`--oneline` kapcsolóval.

```
80560db (HEAD -> master) jegyzeteim ignorálása
30bf35d Hoztunk egy hordót
b677a86 Raktam bele egy körtét
c45abc3 Készítettem egy gyümölcskosarat
```

Máris csak a lényeget látjuk. Az is látható, hogy csak 7
karaktert kaptunk a hashből.
Elég csak pár karakter, hogy beazonosítsunk
például egy commitot a hash-ével.

[Előző](workshop/1_installation?id=telepítés-és-konfiguráció) |
[Következő](workshop/3_branch?id=branch-ek-elágazás)
