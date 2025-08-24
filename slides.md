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

- Performant, fiable, productif
- Langage système
- Memory safe

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

Qui ici a fait du C/C++ ?
Normalement si vous avez fait un peu de C/C++ vous connaissez les segfaults (sinon vous êtes trop fort)

Un langage est dit "memory safe" s'il empèche les manipulations invalide de la mémoire.
Les accès invalide à la memoire sont la source de nombreux problème:
- crash (segfault memory core dumped)
- bugs
- [failles de sécurité](https://cwe.mitre.org/data/definitions/1399.html)

Quelques failes de sécurité courante qui sont impossible dans les langage memory safe: use after free, buffer overflow, out of bounds access
[source](https://media.defense.gov/2023/Dec/06/2003352724/-1/-1/0/THE-CASE-FOR-MEMORY-SAFE-ROADMAPS-TLP-CLEAR.PDF)

---

## Hello, World!

Note:
Pour ce familiariser avec le langage je vous propose d'écrire ensemble le traditionel Hello, World!
Et pour commencer, nous allons créer notre projet avec cargo

---
## Cargo

- Gestion de projet
- Extensible

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
Les arguments sont au format "nom: TYPE"
Si une valeur est retourné on l'indique après la flèche

Très simple: il n'y a pas d'argument par défaut/optionel/nommé, pas de varargs/variadic

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
Une variable est déclaré avec le mot-clé "let" suivie de son nom, par défaut le type est inféré.
Attention les variables sont quand même strictement typé (ne peuvent pas changer de type) et en plus les variables ne sont pas nullable. 

---

```rust[3]
fn main() {
    let name  = "Sfeir";
    println!("Hello, {}!", name);
}
```

Note:
Si vous suivez attentivement vous devriez avors des questions sur `println!`:
- Pourquoi elle a un point d'exclamation ?
- Pourquoi elle peut prendre 1 ou 2 argument ?

Ce n'est pas vraiment une fonction: c'est une fonction-like macro
Les macros sont un mécanisme de métaprogramming qui génère du code.

C'est un sujet un hors sujet donc retenez juste que les macros peuvent ignorer des règles de syntaxe puisqu'une étape d'expansion va les réécrire  

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

## Références
- adresse vers une valeur
- toujours valide

![](reference.svg)

Note:
Les références sont des adresses vers des objets.
En rust les références sont explicite (comme en C++), signalé par "&" et pointent toujours vers un objet valide

On fait des références pour éviter de dupliquer nos valeurs, la plupart des langages avec GC font eux-même le choix de passer par référence ou de copier les valeurs

---

## Rappels sur la mémoire

![Schema de la mémoire de notre programme](memory_layout.svg)

Note:
Pour pouvoir être exécuté notre programme à besoin de mémoire (Du point de vue de l'application la mémoire est continue, c'est la mémoire virtuelle fournit par l'OS).

Chaque bloc décrit ici à un rôle précis:

---

## Blocs statiques

- Text contient les instructions
- Data contient les variables statiques initialisés
- Bss contient les variables statiques non initialisés

Note:
Les block text, data et bss sont de taille fixe et sont donc très simple à gérer.
Il faut juste initialliser les variable dans Bss avant d'y accéder

---

## Stack

```rust
let var1 = 58;
{
    let var2 = 39
} // var2 n'est plus accessible 
```
![Schema de l'utilisation d'une stack](stack.svg)

Note:
La stack est le bloc qui va contenir toutes nos variables, elle est géré automatiquement par le compilateur.
Comme sont nom l'indique c'est une pile où sont inséré des valeurs de manière continue, les éléments doivent donc être LIFO
Elle a une taille maximale de quelques Mo (dépasser cette limite déclenche une erreur stack overflow) et contient uniquement des données de taille fixe

[Pour plus de détails](https://yuriygeorgiev.com/2024/02/19/x86-64-cpu-architecture-the-stack/)

---

## Heap

- Gestion manuelle
- Allocation lente

![Schema de la heap](heap.svg)

Note:
Le tas est le bloc le plus puissant. C'est lui qui va contenir nos données de taille dynamique ou trop grosse pour rentrer dans la stack.
En contrepartie la heap à 2 inconvénients:
Pour allouer de la mémoire, l'allocateur doit trouvé un espace libre ou en redemander l'OS -> Ça prend un peu de temps et rend la heap plus lente que la stack lors de l'allocation.
Généralement le temps d'allocation reste très faible mais il faire beaucoup d'allocation/déallocation très rapidement peut ralentir considérablement le programme

Le plus gros inconvénient c'est qu'elle doit être géré manuellement soit par le dev (C/C++: malloc/free) soit par le GC durant l'exécution

La gestion manuelle de la mémoire est la source de nombreux bugs ou failles de sécurité (ex: 70% des CVE Chromium sont des erreurs de memory safety)
Le garbage collector est beaucoup plus fiable mais au prix d'un peu de performance

---

## Surprise!

```rust
fn main() {
    let name: &str = "Sfeir";
    println!("Hello, {}!", name);
}
```

![](reference.svg)

Note:
J'espère que vous avez été attentif !
Dans quelle partie de la mémoire est situé la variable "name" ? Stack
Dans quelle partie de la mémoire est situé la valeur "Sfeir" ? .data (C'est une constante -> variable statique initiallisé)

---

## Et la memory safety ?

- Ownership
- Borrow checker

Note:
C'est bien joli tout ça mais à la base on devait résoudre le problème de la mémoire safety !
Pour l'instant ce qu'on a vu de Rust est plutôt proche du C++ alors que ce n'est vraiment pas un bon example en terme de memory safety !

La solution est en 2 parties:
- L'ownership pour l'allocation/déallocation de la mémoire
- Le borrow checker pour valider les références

C'est deux eléments sont assez unique à Rust et sont souvent cité comme le principal obstacle à l'apprentissage de Rust

---

## Ownership

- Chaque valeur a un seul proriétaire
- Si le owner est *out-of-scope* la valeur est **dropped**

Note:

Les mots sont abstaits mais globalement ici une valeur désigne une structure ou une enumération
Le propriétaire est une variable qui possède une structure (cad: ce n'est pas une référence).
Lorsque le propriétaire est hors de porté, la valeur est supprimé immédiatement, contrairement au langage avec GC.

petit rappel: porté des variables

---

```rust
{
    let s: &str = "Hello, world!";
} 
```

Note:
Lorsque s sort de la porté, l'ownership ne fait rien puisque s n'est *pas* un owner (c'est une référence)

---

```rust
{
    let s: String = String::from("Hello, world!");
} 
```

Note:
Ici on a remplacé &str par String: s est le owner de la String.
Maintenant lorsque s sort de la porté, l'ownership nous dit que la String possédé par S doit être drop

---

```rust
{
    let s1: String = String::from("Hello, world!");
    let s2 = s1;
} 
```

Note:
Que se passe t-il quand "s2 = s1" s'exécute ? 
Dans beaucoup de langage avec GC s2 est une référence à s1 mais en Rust les références sont explicites.

---

```rust
{
    let s1: String = String::from("Hello, world!");
    let s2 = s1;
} 
```

![Schema clone](clone.svg)

Note:
On pourait cloner s1. C'est possible avec la fonction "clone()" mais ce n'est pas très performant: il faut refaire une allocation et copier toutes la chaîne !

---

```rust
{
    let s1: String = String::from("Hello, world!");
    let s2 = s1;
}
```

![Schema double ownership](double_owner.svg)

Note:
On pourrait copier seulement les métadatas de s1 et partager le buffer ?
Non, on brise une règle de l'ownership: une valeur n'as qu'un propiétaire

---

```rust
{
    let s1: String = String::from("Hello, world!");
    let s2 = s1;
    drop(s1);
    println!("{}", s2);
}
```

![Schema dangling pointer](dangling.svg)

Note:
Si s1 est drop, en tant que owner de la chaîne de charactère, il libère la mémoire
Mais maintenant s2 a un "dangling pointer": un pointer qui ne mêne à rien.
C'est extrèmement dangeureux car si on essaye de lire le contenu de s2 c'est un "use after free" et si s2 est drop la libération du pointer va provoquer un double free

---

## Move
```rust
{
    let s1: String = String::from("Hello, world!");
    let s2 = s1;
    drop(s1);
    println!("{}", s2);
}
```

![Schema move](move.svg)

Note:
"s2 = s1" est un move ! On déplace la propriété de s1 vers s2 et s1 devient invalide. Plus de problème de mémoire

---

## Surprise 2

```rust
let x = String::from("Sfeir");
let y = x;

println!("Hello, {}", x);
```

Note:
Que va afficher le programme ? Une erreur "x" n'est plus valide à cause du move "y = x"

---

```rust
let x = String::from("Sfeir");
let y = x;

println!("Hello, {}", x);
```
![Erreur de compilation a cause d'un move](move_error.png)

Note:
Je vous invite à admirer la beauté du message d'erreur du compilateur
Il nous explique parfaitement la cause du problème

La valeur "Sfeir" a été déplacé vers de 'x' vers 'y'. Comme une valeur ne peut avoir qu'un propiétaire 'x' n'est plus une variable valide.
Le compilateur propose une solution: utiliser la methode `clone()` qui va dupliquer la chaîne de caractère. Après la duplication 'x' garde sa chaîne de caractères et
'y' obtient une chaîne identique à celle de 'x'
Comme le compilateur le dis juste au dessus cloner à un coût: vous faîtes une nouvelle allocation mémoire qui peut prendre du temps et de la mémoire en fonction de la taille de la valeur dupliqué.

Vous avez peut-être remarqué que le compilateur nous dit aussi que le move à lieu parce que le type n'implémente pas le trait `Copy`.
Pour les types très petit le coût d'un clone est minuscule, le trait `Copy` est un marqueur qui indique au compilateur qu'il peut cloner tous le temps, ce qui permet de ne pas avoir à ce soucier des moves.

---

## Borrow

```rust
let x = String::from("Sfeir");
let y = &x;

println!("Hello, {}", x);
```

Note:
Une solution que le compilateur n'a pas proposé est de faire un *emprunt* (on prend la valeur par reference)
Comme le nom l'indique plus que de prendre possésion de la valeur, on va temporairement l'emprunter.
Ici, `x` est l'*owner* et `y` *borrow* la valeur de `x`. Comme `y` est une référence (et donc pas propriétaire), `x` est toujours valide !

---

## Dangling ?
```rust
let x = String::from("Sfeir");
let y = &x;
drop(x);

println!("Hello, {}", y);
```

Note:
On ne brise aucune règle de l'ownership et pourtant y est une référence invalide !
Et oui l'ownership sert à vérifier que la mémoire est déalloué seulement une fois ne garentie pas la validité des reférences.

---

## Borrow checker

```rust
let x = String::from("Sfeir");
let y = &x;
drop(x);

println!("Hello, {}", y);
```

![Message d'erreur move d'une valeur borrow](move_borrow.png)

Note:
Heuresement rust à le **borrow checker** !

---

## Borrow checker
- Une reference est toujours valide
- Une valeur à une seule référence mutable **ou** plusieurs références immutables

Note:
La première règle est nécéssaire pour obtenir la *memory safety*: il ne faut pas que la valeur soit *dropped* pendant la *durée de vie* de la référence,
sinon on s'expose a des bugs comme le use after free qui au mieux provoque des segfault, au pire des undefined behaviors et des failles de sécurités

La deuxième règle évite les *data race* et facilite grandement la *thread safety*.

---

## Surprise 3

```rust
let s1 = String::from("Sfeir");
let s2 = &s1;
let s3 = &s1;
```

Note:
Est-ce que ce code respecte le borrow checker ? Oui -> plusieurs référence non mutable

---

## Surprise 3

```rust
let s1 = String::from("Sfeir");
let s2 = &mut s1;
```

Note:
Est-ce que ce code respecte le borrow checker ? Oui mais il y a une faute -> Il ne peut pas y avoir référence mutable vers une valeur immutable (contrairement à Java)

---

## Surprise 3

```rust
let mut s1 = String::from("Sfeir");
let s2 = &mut s1;
```

Note:
Contrairement à d'autre langage (Java, Typescript), une variable immutable est (par défaut) vraiment immutable. Il n'est pas possible de push() une valeur sur une liste immutable par exemple

---

## Surprise 3

```rust
let mut s1 = String::from("Sfeir");
{
    let s2 = &mut s1;
}
let s3 = &mut s1;
```

Note:
Est-ce que ce code respecte le borrow checker ? Oui -> La référence `s2` est relaché avant que `s3` ne soit déclaré !

---

## Félicitation

Note:
Bon, c'était compliqué tous ça ! Si vous n'avez pas tous saisi, c'est normal ! Il m'a fallut plusieurs jours pour vraiment comprendre et connaître ces rêgles
Ce sont les 2 concepts qui différencie vraiment rust des autres langages, le reste de la présentation seraa beaucoup plus simple, promis !

---

## Struct

```rust
struct Person {
    name: String,
    role: Role
}
```

Note: 
Les structs sont l'équivalent des classes, elles rassemblent des données dans un type et permette d'y associer des fonctionnalités 

---

```rust[2|3]
struct Person {
    pub name: String,
    pub(crate) role: Role,
}
```

Note:
Comme dans beaucoup d'autre langage un mécanisme d'encapsulation existe
Par défaut les champs d'une structure sont privé (accessible uniquement depuis le module actuel)
Avec `pub` on peut rendre un champ public dans n'importe quel module
Avec `pub(crate)` on rend le champ accessible dans la crate actuel

---

```rust
let speaker = Person {
    name: String::from("Fabien"),
    role: Role::DevFullstack,
};
```

Note:
Pour instancier une structure, on donne le type suivit de toutes les clé-valeurs `champ: value` de la structures.
la syntaxe permet d'éviter les constructeurs avec beaucoup d'arguments et les objects à moitié initiallisé
Si vous n'avez pas accès à un champs d'une structure il faudra utiliser **un constructeur**

---

```rust
impl Person
    pub fn manager() -> Person {
        Self {
            name: String::from("Joshua Liaud"),
            role: Role::DevFront, //Angular
        }
    }
}
```

```rust
let em = Person::manager();
```
Note:
Il n'y a pas de constructeur en rust, mais il y a des fonctions associées (fonction statique en bon Java) qui peuvent returner des instances
Les fonctions associé et les méthodes se déclare dans un bloc "impl Type" séparé de la déclaration de la structure (mais dans même le module TODO?)

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

Pour rappel le passage par valeur provoque un move à cause de l'ownership !

TODO: Ajouter des fonction avec `self` et `&mut self` ?

---

## Enumération

```rust
enum Role {
    DevFront,
    DevBack,
    DevFullstack,
    DevOps,
}
```

Note:
Les énumérations en Rust fonctionne comme celle de la plupart des langages 

---

## Enumération

```rust
enum Role {
    DevFront(TechFront),
    DevBack(TechBack),
    DevFullstack {
        front: TechFront,
        back: TechBack
    },
    DevOps,
}
```

Note:
Mais elles peuvent aussi avoir des données associées ! Les données peuvent être hétérogène entre les variantes de l'enum sans aucun soucis
Les données associées peuvent être sous la forme d'un tuple, ou d'une stucture

---

```rust
Person {
    name: String::from("Joshua Liaud"),
    role: Role::DevFront(TechFront::Angular),
}
```

---

```rust
impl Role {
    fn fullstack_java() -> Self {
        Self::DevFullstack {
            front: TechFront::GWT,
            back: TechBack::Java
        }
    }
}
```

Note:
Les enumérations peuvent définir des fonctions associées et des méthodes de la même manière que les structs

---

## Pattern matching

Note:
TODO

---

## Traits

Note:
A peu près équivalent aux interfaces, les traits permettent de déclarer des comportements communs à plusieurs types.

Les traits sont utilisé pour beaucoup de chose:
- Ownership: Clone, Drop, Copy
- Operateur: Add, Sub, Mul, Div, BitAnd, BitOr, BitXor, Shr, Shl, Neg, Not et leurs équivalent Assign
- Comparaison: PartialEq, Eq, PartialOrd, Ord
- Conversion: From, Into, TryFrom, TryInto
- Affichage: Debug, Display, ToString
- Iterateur

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
La fonction clone n'a pas d'implementation par défaut, elle est doit être implémenté 
La fonction clone_from a une implémentation par défaut et peut optionellement être override

Les plus observateurs ont remarqué ': Sized'. Il s'agit d'un bound: "une contrainte". Pour pouvoir implémenter Clone une structure doit implémenter Sized

---

```rust
impl Clone for Person {
    fn clone(&self) -> Self {
        Self {
            name: self.name.clone(),
            role: self.role.clone()
        }
    }
}
```

Note:
Un trait est implémenté de manière très similaire à une structure.
Comme les fonctions associées, l'implémentation d'un trait par une structure est séparé de la déclaration du type.

---

```rust
#[derive(Debug, Clone)]
pub struct Person {
    name: String,
    role: Role
}
```

Note:
Certain trait comme Clone, Debug, sont extrèmement courant et répétitif a implémenter. Et la solution à ce problème, c'est les derive macros.
Une derive macro permet d'implémenter un trait de manière simple et rapide.

---

```rust
trait IsPalindrome {
    fn is_palindrome(&self) -> bool;
}

impl IsPalindrome for &str {
...
}
```

```rust
assert("kayak".is_palindrome());
```

Note:
Il y a quelques instant j'ai dis que l'implémentation d'un trait était séparé de la déclaration du type. Mais est-ce que ça veut dire que je peux implémenter des traits sur des types externes (types importé de la std ou d'une autre crate, que je n'ai pas défini) ?
Oui, mais avec une restriction: une implémentation ne peut pas être orpheline càd elle doit être dans la même crate que la déclaration de la struct **ou** du trait

---

## Struct abstraite et héritage

Note:
Vous connaissez les paroles: après les interfaces on voit les structures abstraites, l'héritage et le polymorphisme... Vive l'OOP
Et bien non ! Il n'y a pas de classes abstraite en rust ni d'héritage ! Il y a bien du polymorphisme mais on ne va même pas en parler !

---

## Genérique

---

## Option

---

## Result

---

## Test unitaire

---

## Rustdoc

---

## Rustfmt

---

## Clippy

---

## Conclusion

Note:
Inconvénients:
Vous l'avez constaté par vous même c'est un langage qui demande plus d'effort au dev -> Plus lent (A apprendre, comme à écrire)
Les temps de compilations: Le compilateur fait beaucoup de vérification et il peut prendre beaucoup de temps (surtout si vous abusez des macros)

Avantages:
Performances: Pas forcément le plus pertinent dans une application mais ça peut répondre à un besoin particulier
Fiabilité: Pas de null, exception explicite, typage fort et expressif
Qualité des outils: Cargo est merveilleux, les messages du compilateurs sont clair et les API sont très expressive.
Rustfmt a une excellente configuration par défaut, Rustdoc est très agréable, ajouter des tests unitaires est ridiculement simple

---

## Resources

- [The rust book](https://doc.rust-lang.org/stable/book/)
- [Rustlings](https://rustlings.rust-lang.org/)
- [The rustdoc book](https://doc.rust-lang.org/rustdoc/index.html)
- [The cargo book](https://doc.rust-lang.org/cargo/)

---

## Des Questions ?

---

## Merci pour votre attention
