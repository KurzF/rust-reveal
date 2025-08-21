# Introduction a Rust
![Ferris, la mascotte de rust](happy-ferris.png)

Note:
Bonjour...

---

## Présentation

![QR code vers le repo](qrcode.svg)

[https://github.com/KurzF/rust-reveal](https://github.com/KurzF/rust-reveal)

Note:
La présentation est disponible sur github, il y a le fichier markdown source et un export PDF
Si vous avez la moindre question durant ma présentation n'hésitez pas à m'interrompre de vive voix.

---

## À propos

Note:
Chez Sfeir depuis février 2021
Dev fullstack Java - Spring / Angular

- Envie de changement
- Remplacer du C++

En 2022, j'ai envie d'apprendre un nouveau language pour découvrir un langage moderne.
Très rapidement mon choix se réduit à Go ou Rust, mais comme je veux aussi remplacer le C++ que je fais sur mon temps libre donc je choisis rust.

(Si ça vous intérèsse je voulais remplacer le C++ à cause de l'absence de gestion de dépendances, des 4-5 constructeurs différents et des segfaults)

---

## Objectifs

- Écosystème
- Langage

Note:

L'objectifs du talk est plutôt transparent, c'est une introduction du langage et de l'écosystème.

La partie écosystème va aborder les outils. On va voir le gestionnaire de dépendences, les tests unitaire, la documentation technique
On ne parlera pas des librairies, ce sera surement le sujet d'un prochain talk

La partie langage va rapidement passer sur la syntaxe pour se concentrer sur les points distinctif de Rust et comment ces différences créé un langage de programmation unique


Normalement, si cette présentation est réussie, vous pourrez déterminez vous même si rust est un choix pertinent pour votre projet (pro ou perso) 

---

## Mots clés

- Langage système
- Memory safe
- Performant, fiable, productif

Note:
On va commencer par une définition très vague et biaisé (les 3 adjectifs viennent du [site officiel](https://www.rust-lang.org/fr)) 

---

## Langage système

- Bas niveau
- Compilé

Note:
Rust est un langage vraiment compilé et sans garbage collector (génère du code machine pas comme les tricheurs Java, C#).
Ça a 2 avantages:
- Pas de runtime
- Performant

Le gros désavantage est qu'on perd la portabilité cross platform offerte par les langages interprétés.

En pratique ce n'est pas vraiment un problème puisque tous les OS grand public sont supportés
[Liste complète](https://doc.rust-lang.org/nightly/rustc/platform-support.html)

La liste des platformes supportées contient des grand classique comme le ARM64 macOS, x86_64 linux ou encore SPARC V9 Solaris ;p.
Dans les architectures plus rare on retrouve des architectures démodé, des microcontrolleurs (Comme ce petit microcontrolleur ARMV7 STM32f401 avec 96kO de ram et 512kO de stockage) et le web assembly

---

## Memory safety

- Assure la validité des accès à la mémoire

Note:
Si vous êtes un dev Java/C#/Javascript félicitation vous utilisez déjà un langage memory safe.
La particularité de rust, c'est que le langage est memory safe sans GC

Un langage est dit "memory safe" s'il empèche les manipulations invalide de la mémoire.
Les langages memory safe sont donc apprécié puisqu'il limite considérablement les [failles de sécurité](https://cwe.mitre.org/data/definitions/1399.html), les bugs et les crashs (segfault: memory core dumped)

Quelques failes de sécurité courante qui sont impossible en safe rust: use after free, buffer overflow, out of bounds access
Note:
[source](https://media.defense.gov/2023/Dec/06/2003352724/-1/-1/0/THE-CASE-FOR-MEMORY-SAFE-ROADMAPS-TLP-CLEAR.PDF)

---

## Rappels sur la mémoire

![Schema de la mémoire de notre programme](memory_layout.svg)

Note:
Pour pouvoir être exécuter notre programme à besoin de mémoire. Chaque bloc décrit ici à un rôle précis:
- text: C'est le code machine de l'exécutable
- data: Toutes les constantes de l'exécutable (chaîne de charactères)
- bss: Variables statiques qui seront assigné plus tard
- stack: Comme sont nom l'indique c'est une pile qui contient toutes les variables de taille connue de notre programme
- heap: Le tas contient toutes les variables de taille dynamique

Les block text, data et bss sont de taille fixe

La stack est géré automatiquement par le compilateur.
Elle a une taille maximale de quelques Mo (dépasser cette limite déclenche une erreur stack overflow) et contient uniquement des données de taille fixe

La tas est géré par le dev (C/C++: malloc/free) ou le GC
C'est la partie de la mémoire qui gère les données de taille dynamique
Et c'est ce dernier bloc qui est la source de beaucoup de nos problèmes:
La gestion manuelle de la mémoire est la source de nombreux bugs ou failles de sécurité (ex: 70% des CVE Chromium sont des erreurs de memory safety)
Le garbage collector est plus fiable mais au prix d'un peux de performance

En pratique la memory safety en rust est appliqué:
- à la compilation: Ownership, Borrow Checker (détecte les use after free, double free)
- à l'exécution: bound check, null check (Option::unwrap)

Nous verrons plus en détails tous ça plus en détails au fur et à mesure de la présentation
Petit fait rigolo: il n'y a aucune garentie sur les fuites mémoire

---

## Boîte à outil

Cargo est votre interface pour tous vos outils

Note:
Cargo est l'équivalent de npm 
une cli pour toutes les tâches communes (new, run, test, add dependencies, doc, fmt) et peut être étendu (clippy, flamegraph)

---

Créer une application
```bash
cargo new projet
```

---

Créer une librarie
```bash
cargo new projet --lib
```

---

```
project/
├── src
│   └── main.rs
└── Cargo.toml
```

Note:
Le dossier src/ contient le code rust
Le fichier main.rs est particulier, c'est dans ce fichier que le compilateur va chercher le point d'entré de notre application 
Si notre application est une librairie, le fichier main.rs sera renommé lib.rs

Enfin le fichier Cargo.toml contient la configuration de notre package

---

```toml
[package]
name = "project"
version = "0.1.0"
edition = "2024"

[dependencies]
```

Note:
Configuration de cargo au format [toml](https://toml.io/fr/)

Le fichier est séparé en 2 sections: les métadata de notre package et les dépendances du package

---

```toml[7]
[package]
name = "project"
version = "0.1.0"
edition = "2024"

[dependencies]
rand = "0.9.1"
```

Note:
Une dépendance peut être ajouté manuellement avec un nom et un numéro de version ...

---

```bash
cargo add rand
```

Note:
... ou bien avec une ligne de commande
Pas besoin de faire un équivalent de "npm install" les dépendances seront téléchargé juste avant le build de notre application

---

```rust
fn main() {
    println!("Hello, world!");
}
```

---

```rust
fn name(arg1: T1) -> T2 {}
```

Note:
Une fonction est déclaré par le mot-clé 'fn'
Le nom de la fonction est unique (impossible d'avoir plusieurs fonction avec le même nom mais des paramètres différents)
Les arguments sont au format nom: TYPE, il n'y a pas d'argument par défaut/optionel, pas de varargs/variadic
Si une valeur est retourné on l'indique après la flèche

---

```rust[1,3|2]
fn main() {
    println!("Hello, world!");
}

```

Note:

La fonction main dans le fichier main.rs est le point d'entré de notre application, comme vous pouver le voir il ne prend aucun argument et ne retourne rien.

Notre main appelle une fonction qui va afficher "Hello, world!" dans la console

---

```console
$ cargo run
   Compiling example v0.1.0 (~/rust_introduction/example)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.17s
     Running `target/debug/example`
Hello, world!
```
Note:
On peut vérifier que le programme fonctionne comme prévu avec la commande `cargo run` qui va télécharger le dépendances si besoin, compiler notre crate et l'éxécuter

---

```rust[2]
fn main() {
    let name = "Sfeir";
    println!("Hello, {}!", name);
}

```

Note:
Une variable est déclaré avec le mot-clé "let" suivie de son nom, par défaut les variables sont immutables et leurs types sont inférés.
Attention les variable ont un type et ne peuvent pas changer de type.

---

```rust[2-3]
fn main() {
    let mut name = "World";
    name = "Sfeir";
    println!("Hello, {}!", name);
}

```

Note:
Une variable est immutable par défaut, il suffit de faire suivre un `mut` à `let` pour la rendre mutable

---

```rust[2]
fn main() {
    let name: &str = "Sfeir";
    println!("Hello, {}!", name);
}
```

Note:
Parfois vous devrez préciser le type manuellement si le compilateur n'arriva à le trouver lui même.

Le type de name est "&str" qu'on peut découper en deux parties "&" et "str". Le "&" signifie que notre variable est une référence et "str" est communément appellé "string slice".

TODO: c'est quoi une référence
TODO: pourquoi une string slice

---

```rust[3]
fn main() {
    let name: &str = "Sfeir";
    println!("Hello, {}!", name);
}
```


Note:
Si vous suivez attentivement vous devriez avors des questions sur `println!`:
- Pourquoi elle a un point d'exclamation ?
- Pourquoi elle peut prendre 1 ou 2 argument ?

Ce n'est pas vraiment une fonction: c'est une fonction-like macro
Les macros sont un mécanisme de métaprogramming qui génère du code.

C'est un sujet un peu compliqué donc retenez juste que les macros peuvent ignorer des règles de syntaxe puisqu'une étape d'expansion va les réécrire  

---

```rust
struct Person {
    name: String,
    role: String
}
```

Note:
On va stocker deux infos dans notre structure: le nom et le role de la personne
(Si vous avez remarquez que le type de chaîne de caractère à changé: félicitation ! String nous permet de travailler avec des chaînes mutable et de longueur arbitraire)

Comme dans beaucoup d'autre langage un mécanisme d'encapsulation existe
Par défaut les champs d'une structure sont privé (accessible uniquement depuis le module actuel)

--- 

```rust[2|3]
struct Person {
    pub name: String,
    pub(crate) role: String,
}
```

Note:
Avec `pub` on peut rendre un champ public dans n'importe quel module
Avec `pub(crate)` on rend le champ accessible dans la crate actuel

---

```rust
let speaker = Person {
    name: String::from("Fabien"),
    role: String::from("développeur fullstack"),
};
```

Note:
Pour instancier une structure, on donne le type suivit de toutes les clé-valeurs `champ: value` de la structures.
la syntaxe permet d'éviter les constructeurs avec trop d'arguments et les objects à moitié initiallisé
Si vous n'avez pas accès à un champs d'une structure il faudra utiliser *un constructeur*

---

```rust
impl Person
    fn directeur_general() -> Person {
        Self {
            name: String::from("Didier Girard"),
            role: String::from("Directeur Général SFEIR Groupe"),
        }
    }
}
```

```rust
let dg = Person::directeur_general();
```
Note:
Il n'y a pas de constructeur en rust, mais il y a des fonctions associées (fonction statique en bon Java) qui peuvent returner des instances

---

```rust
impl Person {
    fn welcome(&self) {
        println!(
            "Bonjour, je m'appelle {} et je suis {}",
            self.name, self.role
        );
    }
}
```

Note:
Pour faire des méthodes, il suffit de créer une fonction associée avec en premier argument `self`, `&self` ou `&mut self`
Ces trois arguments permette de faire la distinction entre passer par valeur, passer par référence et passer par référence mutable

---

## Traits

---

```rust[2|4-6]
pub trait Clone: Sized {
    fn clone(&self) -> Self;

    fn clone_from(&mut self, source: &Self) {
        *self = source.clone()
    }
}
```

Note:
L'équivalent des interfaces, les traits permettent de déclarer des fonctions qui seront implémenter par les structs
La fonction clone n'a pas d'implementation par défaut, elle est doit être implémenté 
La fonction clone_from a une implémentation par défaut et peut optionellement être override

Les plus observateurs ont remarqué ': Sized'. Il s'agit d'un bound: "une contrainte". Pour pouvoir implémenter Clone une structure doit implémenter Sized

---

```rust
impl Clone for Greet {
    fn clone(&self) -> Self {
        Self {
            name: self.name.clone()
        }
    }
}
```

Note:
Un trait est implémenté de manière très similaire à une structure 

---

```rust
#[derive(Debug, Clone)]
pub struct Greet {
    name: String
}
```

Note:
Certain comme Clone sont extrèmement courant et répétitif a implémenter. Et la solution à ce problème, c'est les derive macros.
Une derive macro permet d'implémenté un trait de manière simple et rapide.

---

## Ownership
- Chaque valeur a un seul proriétaire
- Si le owner est *out-of-scope* la valeur est **dropped**

Note:
L'ownership est **la** caractéristique distinctive du langage et c'est grâce à elle que Rust est memory safe sans avoir besoin de GC.
C'est un concept qu'on ne retrouve pas dans d'autre langage et qui prends un peu de temps à assimiler

Les mots sont abstaits mais globalement ici une valeur désigne un morceau de mémoire alloué (une structure ou une enum) et
le propriétaire est une variable qui est chargé de nettoyé cette mémoire

petit rappel: porté des variables

---

```rust
{
    // s n'est pas encore accessible
    let s = String::from("Hello, world!");
    // s est accessible !
} // s n'est plus accessible 
```

Note: Rien de nouveau !

---

## Ownership
- Chaque valeur a un seul proriétaire
- Si le owner est *out-of-scope* la valeur est **dropped**

Note:
Lorqu'une valeur est *relâché* sa mémoire est libéré
La libération de la mémoire (et l'exécution de `Drop::drop()` si implémenté) se fait immédiatement contrairement au nettoyage par GC 

---

```rust
let x = String::from("x");

dbg!(x);
```

Note:
A vôtre avis qu'affiche ce code ?

---

```
x = "x"
```
---

```rust
let x = String::from("x");
let y = x;

dbg!(y);
```

Note:
Si j'ajoute une nouvelle variable `y` et que j'assigne `x` à `y` on fait un *move*
On vient de changer le propiétaire de la chaîne de caractères.

Qu'est ce qui va se passer si on affiche le contenu de la variable `y` ?

---

```
y = "x"
```

---

```rust
let x = String::from("x");
let y = x;

dbg!(x);
```

Note:
Et encore la même question: Qu'est qui s'affiche si on regarde le contenu de `x` ?

---

```console
error[E0382]: use of moved value: `x`
 --> src/bin/day01.rs:5:5
  |
2 |     let x = String::from("x");
  |         - move occurs because `x` has type `String`, which does not implement the `Copy` trait
3 |     let y = x;
  |             - value moved here
4 |
5 |     dbg!(x);
  |     ^^^^^^^ value used here after move
  |
  = note: this error originates in the macro `dbg` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
  |
3 |     let y = x.clone();
  |             ++++++++
    
For more information about this error, try `rustc --explain E0382`.
```

Note:
Je vous invite à admirer la beauté du message d'erreur du compilateur
Il nous explique parfaitement la cause du problème

La valeur de x a été déplacé vers y. Comme une valeur ne peut avoir qu'un propiétaire `x` n'est plus une variable valide.
Le compilateur propose une solution: utiliser la methode `clone()` qui va dupliquer la chaîne de caractère. Après la duplication `x` garde sa chaîne de caractères et
`y` obtient une chaîne identique à celle de `x`
Comme le compilateur le dis juste au dessus cloner à un coût: vous faîtes une nouvelle allocation mémoire qui peut prendre du temps et de la mémoire en fonction de la taille de la valeur dupliqué et du nombre de clone.

Si vous êtes observateur vous avez remarquer que le compilateur nous dit que le move à lieu parce que le type n'implément pas le trait `Copy`.
Pour les types très petit le coût d'un clone est minuscule, le trait `Copy` indique au compilateur qu'il peut cloner tous le temps, ce qui permet de ne pas avoir à ce soucier des moves.
Les types qui implémentes `Copy` sont toutes les entiers et float 

---

## Borrow

```rust
let x = String::from("x");
let y = &x;

dbg!(x);
dbg!(y);
```

Note:
Une solution que le compilateur n'a pas proposé est de faire un *emprunt* (on prend la valeur par reference)
Comme le nom l'indique plus que de prendre possésion de la valeur, on va temporairement l'emprunter.
Ici, `x` est l'*owner* et `y` *borrow* la valeur de `x` 

---

## Borrow Checker
- Une reference est toujours valide
- Une valeur à une seule référence mutable *ou* plusieurs références immutables

Note:
L'incovénient de cette solution c'est qu'il faut respecter le **borrow checker**
La première règle est nécéssaire pour obtenir la *memory safety*: il ne faut pas que la valeur soit *dropped* pendant la *durée de vie* de la référence,
sinon on s'expose a des bugs comme le use after free qui au mieux provoque des segfault, au pire des undefined behaviors et des failles de sécurités

La deuxième règle évite les *data race* et facilite grandement la *thread safety*,

---

## Test unitaire

---

## Rustdoc

---

## Rustfmt

---

## Clippy

---

## Resources

- Rust book
- Rustling

---

## Des Questions ?

---

## Merci pour votre attention
