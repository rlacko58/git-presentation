# Alapok és SSH kulcs

### Alapok

Hogyan lehet a mi `.git` mappánkat megosztani a nagyvilággal?

Több módja is van ennek, például akár egy .git mappát
is lelehet magunkhoz klónozni.

A `git clone <elérési út>` parancsot használjuk ahhoz, hogy
valahonnan lehúzzunk egy git repo-t.

Ha például csak egy mappára utalunk, aminek a neve
`awesome_project.git`:

```
$ git clone --bare awesome_project awesome_project.git
Cloning into bare repository 'awesome_project.git'...
done.
```

Ezt akár kirakhatjuk egy fájlszerverre és onnan megoszthatnánk.

Következő szint, mikor weben keresztül szedjük le a nekünk
kellő repo-t. Ilyenkor HTTP protokollt használhatunk,
ami hasonló az előzőhöz és olyan, mint mikor egy fájlt
töltünk le egy oldalról.

```
$ git clone https://github.com/rlacko58/git-presentation.git
Cloning into 'git-presentation'...
remote: Enumerating objects: 173, done.
remote: Counting objects: 100% (173/173), done.
remote: Compressing objects: 100% (114/114), done.
remote: Total 173 (delta 65), reused 155 (delta 47), pack-reused 0
Receiving objects: 100% (173/173), 291.34 KiB | 1.22 MiB/s, done.
Resolving deltas: 100% (65/65), done.
```

Ez szimplán fogja és készít nekünk egy olyan repository-t, mint
amit az előzőkben csináltunk kézzel.
Lemásolja a .git mappát, majd pedig onnan kiszedi a megfelelő
dolgokat a mi `work tree`-nkbe.

### SSH

Másik protokoll amit tudunk használni az az SSH.
Ez azért nagyon jó, mert ha ezen keresztül szedünk le,
akkor nem kell minden alkalommal a github, gitlab
felhasználónkba belépnünk egy repo-n való dolgozáskor.

Ahhoz, hogy ilyet tudjunk csinálni viszont SSH kulcsot kell
készítenünk.
Ennek annyi a trükkje, hogy rakunk a saját gépünkre egy
privát kulcsot és a távoli szerverre egy publikus kulcsot,
vagy bárhova ahova szeretnénk.
Ezután mikor az adott szerverre felakarunk menni, valamit
csinálni ott, akkor előtte a szerver leauthentikál minket
és egy kulcs csere után elkezdhetünk vele kommunikálni.
Az authentikáció lépése nagy vonalakban:

- Szerver publikus kulcsal letitkosít valamit és elküldi nekünk
- Ezt amit elküld csak a privát kulccsal lehet kibontani,
  ezt mi elvégezzük
- A kibontott adatok közt lesz egy másik kulcs, mellyel
  titkosítva elküldjük a szervernek ami kell az authentikációhoz
- A szerver kibontja, látja hogy minden oké és
  megtörténik a kulcs csere

Ezután áttérnek egy másik módszerre amihez már mindkét oldalt
megvan a megfelelő kulcs és titkosítva küldik egymásnak
az adatokat

Nagyon király dolog, mivel gyors, hatékony és nem kell
jelszót beírogatni minden művelet közt

#### Saját SSH kulcs

###### Linuxon / MacOS-en

1. Terminált megnyitjuk
2. Generálunk egy SSH kulcsot  
   `ssh-keygen -t rsa -b 4096`
3. Alapértelmezett helyre mentjük
4. Megadunk egy jelszót ha szeretnénk  
   _Ha valaki megszerzi a privát kulcsunk nem tudja használni
   a jelszó beírása nélkül_

Adjuk hozzá az SSH agent-ünkhöz.

1. Elindítjuk az agent-et ha még nem ment volna
   `$ eval "$(ssh-agent -s)"`  
   `Agent pid 59566`
2. Hozzáadjuk az új kulcsunk
   `ssh-add ~/.ssh/id_rsa`

Végezetül a publikus kulcsot feltöltjük például githubra

A publikus kulcs helye: `~/ssh/id_rsa.pub`

###### Windowson

1. Git Bash-t megnyitjuk
2. Generálunk egy kulcsot
   `ssh-keygen -t rsa -b 4096`
3. Elmentjük az alapértelmezett helyre
4. Jelszó adunk hozzá ha szeretnénk

Hozzá adjuk az SSH agent-ünkhöz

1. Elindítjuk az agent-et ha még nem ment volna
   `$ eval $(ssh-agent -s)`
   `> Agent pid 59566`
2. Hozzáadjuk az új kulcsunk
   `ssh-add ~/.ssh/id_rsa`

Végezetül a publikus kulcsot feltöltjük például githubra

A publikus kulcs helye: `~/ssh/id_rsa.pub`

Forrás: [docs.github.com](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

[Előző](workshop/3_branch?id=branch-ek-elágazás)
