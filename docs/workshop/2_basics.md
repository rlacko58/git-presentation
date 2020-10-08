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
\$ git config -h
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

### Repository

A Git repository egy olyan mappa, mely Git verziókezelés alatt
áll.
Például az a mappa, ahol van egy .git mappa, az egy git repo.

```
\$ ls -al
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

Nézzük meg, hogy mi a helyzet a git-el ezzel a mappában.  
Ehhez a `git status` parancsot tudjuk használni.
Ez a parancs a jelenlegi git repo-nkról képes
információkat kiírni.

```
\$ git status
fatal: not a git repository (or any of the parent directories): .git

```

Teljesen üres a mappa, még nincs verzió kontroll alatt.
`ls -al` -el tudjuk is ellenőrizni.

Tegyük verziókontroll alá a `git init` paranccsal.
Ez beállítja nekünk a `master` branchet és más alap dolgokat.
A branch-ről még később beszélek, de azt jegyezzük meg addig,
hogy ide kerülnek a mentéspontjaink egymás után sorban, mint
egy fa ága.

<div style="text-align:center"><img src="workshop/img/basic-branching.png" alt="Git branch" /></div>

```
\$ git init .
Initialized empty Git repository in .../projektem/.git/

```

Ha megnézzük, mostmár van egy .git mappánk

```
\$ ls -al
total 12
drwxrwxr-x 3 rlacko rlacko 4096 okt 8 16:52 .
drwxrwxr-x 12 rlacko rlacko 4096 okt 8 16:49 ..
drwxrwxr-x 7 rlacko rlacko 4096 okt 8 16:52 .git

```

Ebben van minden adatunk a git repo-val kapcsolatban.
Gyakorlatilag mikor egy távoli git repo-t leszedünk, akkor
ezt a mappát kapjuk meg. Ebből ezután a git alkalmazásunk
előállítja nekünk a master ág legutóbbi mentéspontját.

#### Változtatások mentése

Na de hogyan mentünk ebbe az adatbázisba?

A fájljainknak különböző állapotai létezhetnek egy
repo-ban.

- Untracked: Ami még nincs verziókontroll alatt
- Staged: Ami már verziókontroll alatt van de még nem
  mentettük el
- Unmodified: Amit már elmentettünk és azóta nem változott
- Modified: Az a fájl, ami már verziókontroll alatt van
  és változott. Ezt utánna szintén Stage-be tudjuk tenni

Nézzük meg ezen az ábrán és egy példa projekten keresztül:

<div style="text-align:center"><img src="workshop/img/git_lifecycle.png" alt="Local Version Control Systems" /></div>

Mi a jelenlegi helyzet a repo-ban, miután inicializáltuk?

```
\$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

Láthatjuk, hogy még nincsenek mentéspontjaink és nincs mit
elmentenünk és egyben a git próbál segíteni, hogy hogyan
tudunk stagelni valamit.
Ezt több helyen is megfigyelhetjük a git-ben.
Próbál segíteni ahol csak tud, minél kényelmesebbé téve
a munkánkat.

Vegyünk fel egy új fájlt.

```
echo 'alma' > gyumolcskosar
```

_Itt az `echo` egy olyan parancs volt, mely kiírta nekünk a
terminálra azt, hogy "alma", de átirányítottuk egy fájlba
a `>` operátorral és mivel nem létezett a fájl, azt létre is
hozta._

Újra megvizsgálva a status-t, látni fogjuk, hogy megjelent, de
még nincs verziókontroll alatt.

```
\$ git status
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
Például a `<fájl>` lehet `*.jpg`amivel minden .jpg fájlt kijelölünk a jelenlegi mappában"_

```
\$ git add gyumolcskosar
```

Ezután megjelenik, mint új fájl a git adatbázisában.

```
\$ git status
...
Changes to be committed:
(use "git rm --cached <file>..." to unstage)
new file: gyumolcskosar
```

A fájlunk átkerült staged módba és ezt akár mostmár el is
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
\$ git commit
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
\$ git status
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
hanem átraktuk a parancsot olyan módba,
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

#### Hogyan tárolódnak a mentéseink?

[Előző](workshop/1_installation) | [Következő](workshop/3_branch)
