---
layout: Publier
title: Cinq ans de rouille
author: "L'équipe Rust Core"
---

Avec tout ce qui se passe dans le monde, vous seriez pardonné d'oublier qu'à ce jour, cela fait cinq ans que nous avons sorti la version 1.0 en 2015! Rust a beaucoup changé ces cinq dernières années, nous voulions donc revenir sur l'ensemble du travail de nos contributeurs depuis la stabilisation de la langue.

Rust est un langage de programmation à usage général permettant à chacun de créer des logiciels fiables et efficaces. Rust peut être conçu pour s'exécuter n'importe où dans la pile, que ce soit en tant que noyau de votre système d'exploitation ou de votre prochaine application Web. Il est entièrement construit par une communauté ouverte et diversifiée d'individus, principalement des bénévoles qui donnent généreusement de leur temps et de leur expertise pour aider à faire de Rust ce qu'il est.

## Changements majeurs depuis la version 1.0

#### 2015

**[1.2] - Parallel Codegen: les** améliorations au moment de la compilation sont un thème important pour chaque version de Rust, et il est difficile d'imaginer qu'il y a eu une courte période où Rust n'avait pas du tout de génération de code parallèle.

**[1.3] - The Rustonomicon:** Notre première version du fantastique "Rustonomicon", un livre qui explore Unsafe Rust et ses sujets environnants est devenu une excellente ressource pour tous ceux qui cherchent à apprendre et à comprendre l'un des aspects les plus difficiles de la langue.

**[1.4] - Prise en charge de Windows MSVC Tier 1:** La première promotion de plate-forme de niveau 1 apportait un support natif pour Windows 64 bits à l'aide de la chaîne d'outils Microsoft Visual C ++ (MSVC). Avant la version 1.4, vous deviez également installer MinGW (un environnement GNU tiers) afin d'utiliser et de compiler vos programmes Rust. Le support Windows de Rust est l'une des plus grandes améliorations de ces cinq dernières années. Tout récemment, Microsoft a [annoncé un aperçu public de sa prise en charge officielle de Rust pour l'API WinRT!] Il est maintenant plus facile que jamais de créer des applications natives et multiplateformes de qualité supérieure.

**[1.5] - Cargo Install:** L'ajout de la possibilité de créer des binaires Rust en plus de la prise en charge des plugins préexistants de cargo a donné naissance à tout un écosystème d'applications, d'utilitaires et d'outils de développement que la communauté adore et dépend. Un bon nombre des commandes que cargo a aujourd'hui étaient les premiers plugins que la communauté a construits et partagés sur crates.io!

#### 2016

**[1.6] - Libcore:** `libcore` est un sous-ensemble de la bibliothèque standard qui ne contient que des API qui ne nécessitent pas d'allocation ou de fonctionnalités au niveau du système d'exploitation. La stabilisation de libcore a apporté la possibilité de compiler Rust sans allocation ni dépendance au système d'exploitation a été l'une des premières étapes majeures vers le support de Rust pour le développement de systèmes embarqués.

**[1.10] - Bibliothèques dynamiques C ABI:** Le type `cdylib` permet à Rust d'être compilé comme une bibliothèque dynamique C, vous permettant d'intégrer vos projets Rust dans n'importe quel système prenant en charge l'ABI C. Certaines des plus grandes réussites de Rust parmi les utilisateurs sont de pouvoir écrire une petite partie critique de leur système dans Rust et de l'intégrer de manière transparente dans la base de code plus large, et c'est maintenant plus facile que jamais.

**[1.12] - Espaces de travail Cargo: les** espaces de travail vous permettent d'organiser plusieurs projets de rouille et de partager le même fichier de verrouillage. Les espaces de travail ont été inestimables dans la construction de grands projets à plusieurs niveaux.

**[1.13] - L'opérateur Try:** Le premier ajout de syntaxe majeur était le `?` ou l'opérateur "Try". L'opérateur vous permet de propager facilement votre erreur à travers votre pile d'appels. Auparavant, vous deviez utiliser l' `try!` macro, qui vous obligeait à envelopper l'expression entière chaque fois que vous rencontriez un résultat, vous pouvez maintenant facilement enchaîner des méthodes avec `?` au lieu.

```rust
try!(try!(expression).method()); // Old
expression?.method()?;           // New
```

**[1.14] - Rustup 1.0:** Rustup est le gestionnaire de la chaîne d'outils de Rust, il vous permet d'utiliser de manière transparente n'importe quelle version de Rust ou l'un de ses outils. Ce qui a commencé comme un humble script shell est devenu ce que les responsables appellent affectueusement une *"chimère"* . Être capable de fournir une gestion de version de compilateur de première classe sur Linux, macOS, Windows et les dizaines de plates-formes cibles aurait été un mythe il y a à peine cinq ans.

#### 2017

**[1.15] - Dériver des macros procédurales: Les macros** dérivées vous permettent de créer des API puissantes et étendues fortement typées sans toutes les règles standard. C'était la première version de Rust que vous pouviez utiliser des bibliothèques comme les `serde` ou `diesel` sur stable.

**[1.17] - Rustbuild:** L'une des plus grandes améliorations pour nos contributeurs au langage a été de déplacer notre système de construction du système initial basé sur la `make` à l'utilisation de la cargaison. Cela a permis à `rust-lang/rust` d'être beaucoup plus facile pour les membres et les nouveaux arrivants de construire et de contribuer au projet.

**[1.20] - Constantes associées:** auparavant, les constantes ne pouvaient être associées qu'à un module. En 1.20, nous avons stabilisé les constantes d'association sur les structures, les énumérations et surtout les traits. Faciliter l'ajout de riches ensembles de valeurs prédéfinies pour les types de votre API, tels que des adresses IP courantes ou des nombres intéressants.

#### 2018

**[1.24] - Compilation incrémentale:** Avant la 1.24, lorsque vous apportiez une modification à votre bibliothèque, rustc devait recompiler tout le code. Maintenant, rustc est beaucoup plus intelligent sur la mise en cache autant que possible et n'a besoin que de régénérer ce qui est nécessaire.

**[1.26] - Trait impl:** L'ajout de `impl Trait` vous donne des API dynamiques expressives avec les avantages et les performances de la distribution statique.

**[1.28] - Global Allocators:** Auparavant, vous étiez limité à l'utilisation de l'allocateur fourni par rust. Avec l'API d'allocateur global, vous pouvez désormais personnaliser votre allocateur en fonction de vos besoins. C'était une étape importante pour permettre la création de la bibliothèque d' `alloc` , un autre sous-ensemble de la bibliothèque standard contenant uniquement les parties de std qui ont besoin d'un allocateur comme `Vec` ou `String` . Il est maintenant plus facile que jamais d'utiliser encore plus de parties de la bibliothèque standard sur une variété de systèmes.

**[1.31] - Édition 2018:** La sortie de l'édition 2018 était de loin notre plus grande version depuis la 1.0, ajoutant une collection de changements de syntaxe et d'améliorations à l'écriture de Rust écrits de manière complètement rétrocompatible, permettant aux bibliothèques créées avec différentes éditions de fonctionner ensemble de manière transparente.

- **Durées de vie non lexicales** Une énorme amélioration du vérificateur d'emprunt de Rust, lui permettant d'accepter un code sécurisé plus vérifiable.
- **Améliorations du système de modules Améliorations** importantes de l'UX dans la façon dont nous définissons et utilisons les modules.
- **Fonctions** Const Les fonctions Const vous permettent d'exécuter et d'évaluer le code Rust au moment de la compilation.
- **Rustfmt 1.0** Un nouvel outil de formatage de code spécialement conçu pour Rust.
- **Clippy 1.0** Rust's linter pour attraper les erreurs courantes. Clippy permet de s'assurer que votre code est non seulement sûr mais correct.
- **Rustfix** Avec tous les changements de syntaxe, nous savions que nous voulions fournir les outils nécessaires pour rendre la transition aussi simple que possible. Désormais, lorsque des modifications sont nécessaires à la syntaxe de Rust, elles ne sont plus qu'un `cargo fix` .

#### 2019

**[1.34] - Registres de caisses alternatifs:** Comme Rust est de plus en plus utilisé en production, il est de plus en plus nécessaire de pouvoir héberger et utiliser vos projets dans des espaces non publics, alors que la cargaison a toujours permis des dépendances git distantes, avec des registres alternatifs votre organisation pouvez facilement créer et partager votre propre registre de caisses qui peuvent être utilisées dans vos projets comme elles l'étaient sur crates.io.

**[1.39] - Async / Await:** La stabilisation des mots-clés async / await pour la gestion des Futures a été l'une des étapes majeures pour faire de la programmation async dans Rust un citoyen de première classe. Même six mois seulement après sa sortie, la programmation asynchrone s'est épanouie dans un écosystème diversifié et performant.

#### 2020

**[1.42] - Modèles de sous-tranches:** Bien que ce ne soit pas le changement le plus important, l'ajout du motif `..` (repos) a été une fonctionnalité de qualité de vie attendue depuis longtemps qui améliore considérablement l'expressivité de la correspondance de motifs avec des tranches.

## Diagnostics d'erreur

Une chose que nous n'avons pas beaucoup mentionnée est à quel point les messages d'erreur et les diagnostics de Rust se sont améliorés depuis la version 1.0. Regarder les anciens messages d'erreur donne maintenant l'impression de regarder une autre langue.

Nous avons mis en évidence quelques exemples qui illustrent le mieux à quel point nous nous sommes améliorés en montrant aux utilisateurs où ils ont commis des erreurs et en les aidant surtout à comprendre pourquoi cela ne fonctionne pas et en leur apprenant comment y remédier.

##### Premier exemple (traits)

```rust
use std::io::Write;

fn trait_obj(w: &Write) {
    generic(w);
}

fn generic<W: Write>(_w: &W) {}
```

<details>
 <summary>1.2.0 Message d'erreur</summary></details>

```
   Compiling error-messages v0.1.0 (file:///Users/usr/src/rust/error-messages)
src/lib.rs:6:5: 6:12 error: the trait `core::marker::Sized` is not implemented for the type `std::io::Write` [E0277]
src/lib.rs:6     generic(w);
                 ^~~~~~~
src/lib.rs:6:5: 6:12 note: `std::io::Write` does not have a constant size known at compile-time
src/lib.rs:6     generic(w);
                 ^~~~~~~
error: aborting due to previous error
Could not compile `error-messages`.

To learn more, run the command again with --verbose.
```



![Une capture d'écran du terminal du message d'erreur 1.2.0.](/images/2020-05-15-five-years-of-rust/trait-error-1.2.0.png)

<details>
 <summary>Message d'erreur 1.43.0</summary></details>

```
   Compiling error-messages v0.1.0 (/Users/ep/src/rust/error-messages)
error[E0277]: the size for values of type `dyn std::io::Write` cannot be known at compilation time
 --> src/lib.rs:6:13
  |
6 |     generic(w);
  |             ^ doesn't have a size known at compile-time
...
9 | fn generic<W: Write>(_w: &W) {}
  |    ------- -       - help: consider relaxing the implicit `Sized` restriction: `+  ?Sized`
  |            |
  |            required by this bound in `generic`
  |
  = help: the trait `std::marker::Sized` is not implemented for `dyn std::io::Write`
  = note: to learn more, visit <https://doc.rust-lang.org/book/ch19-04-advanced-types.html#dynamically-sized-types-and-the-sized-trait>

error: aborting due to previous error

For more information about this error, try `rustc --explain E0277`.
error: could not compile `error-messages`.

To learn more, run the command again with --verbose.
```



![Une capture d'écran du terminal du message d'erreur 1.43.0.](/images/2020-05-15-five-years-of-rust/trait-error-1.43.0.png)

##### Deuxième exemple (aide)

```rust
fn main() {
    let s = "".to_owned();
    println!("{:?}", s.find("".to_owned()));
}
```

<details>
 <summary>1.2.0 Message d'erreur</summary></details>

```
   Compiling error-messages v0.1.0 (file:///Users/ep/src/rust/error-messages)
src/lib.rs:3:24: 3:43 error: the trait `core::ops::FnMut<(char,)>` is not implemented for the type `collections::string::String` [E0277]
src/lib.rs:3     println!("{:?}", s.find("".to_owned()));
                                    ^~~~~~~~~~~~~~~~~~~
note: in expansion of format_args!
<std macros>:2:25: 2:56 note: expansion site
<std macros>:1:1: 2:62 note: in expansion of print!
<std macros>:3:1: 3:54 note: expansion site
<std macros>:1:1: 3:58 note: in expansion of println!
src/lib.rs:3:5: 3:45 note: expansion site
src/lib.rs:3:24: 3:43 error: the trait `core::ops::FnOnce<(char,)>` is not implemented for the type `collections::string::String` [E0277]
src/lib.rs:3     println!("{:?}", s.find("".to_owned()));
                                    ^~~~~~~~~~~~~~~~~~~
note: in expansion of format_args!
<std macros>:2:25: 2:56 note: expansion site
<std macros>:1:1: 2:62 note: in expansion of print!
<std macros>:3:1: 3:54 note: expansion site
<std macros>:1:1: 3:58 note: in expansion of println!
src/lib.rs:3:5: 3:45 note: expansion site
error: aborting due to 2 previous errors
Could not compile `error-messages`.

To learn more, run the command again with --verbose.

```



![Une capture d'écran du terminal du message d'erreur 1.2.0.](/images/2020-05-15-five-years-of-rust/help-error-1.2.0.png)

<details>
 <summary>Message d'erreur 1.43.0</summary></details>

```
   Compiling error-messages v0.1.0 (/Users/ep/src/rust/error-messages)
error[E0277]: expected a `std::ops::FnMut<(char,)>` closure, found `std::string::String`
 --> src/lib.rs:3:29
  |
3 |     println!("{:?}", s.find("".to_owned()));
  |                             ^^^^^^^^^^^^^
  |                             |
  |                             expected an implementor of trait `std::str::pattern::Pattern<'_>`
  |                             help: consider borrowing here: `&"".to_owned()`
  |
  = note: the trait bound `std::string::String: std::str::pattern::Pattern<'_>` is not satisfied
  = note: required because of the requirements on the impl of `std::str::pattern::Pattern<'_>` for `std::string::String`

error: aborting due to previous error

For more information about this error, try `rustc --explain E0277`.
error: could not compile `error-messages`.

To learn more, run the command again with --verbose.
```



![Une capture d'écran du terminal du message d'erreur 1.43.0.](/images/2020-05-15-five-years-of-rust/help-error-1.43.0.png)

##### Troisième exemple (vérificateur d'emprunt)

```rust
fn main() {
    let mut x = 7;
    let y = &mut x;

    println!("{} {}", x, y);
}
```

<details>
 <summary>1.2.0 Message d'erreur</summary></details>

```
   Compiling error-messages v0.1.0 (file:///Users/ep/src/rust/error-messages)
src/lib.rs:5:23: 5:24 error: cannot borrow `x` as immutable because it is also borrowed as mutable
src/lib.rs:5     println!("{} {}", x, y);
                                   ^
note: in expansion of format_args!
<std macros>:2:25: 2:56 note: expansion site
<std macros>:1:1: 2:62 note: in expansion of print!
<std macros>:3:1: 3:54 note: expansion site
<std macros>:1:1: 3:58 note: in expansion of println!
src/lib.rs:5:5: 5:29 note: expansion site
src/lib.rs:3:18: 3:19 note: previous borrow of `x` occurs here; the mutable borrow prevents subsequent moves, borrows, or modification of `x` until the borrow ends
src/lib.rs:3     let y = &mut x;
                              ^
src/lib.rs:6:2: 6:2 note: previous borrow ends here
src/lib.rs:1 fn main() {
src/lib.rs:2     let mut x = 7;
src/lib.rs:3     let y = &mut x;
src/lib.rs:4
src/lib.rs:5     println!("{} {}", x, y);
src/lib.rs:6 }
             ^
error: aborting due to previous error
Could not compile `error-messages`.

To learn more, run the command again with --verbose.
```



![Une capture d'écran du terminal du message d'erreur 1.2.0.](/images/2020-05-15-five-years-of-rust/borrow-error-1.2.0.png)

<details>
 <summary>Message d'erreur 1.43.0</summary></details>

```
   Compiling error-messages v0.1.0 (/Users/ep/src/rust/error-messages)
error[E0502]: cannot borrow `x` as immutable because it is also borrowed as mutable
 --> src/lib.rs:5:23
  |
3 |     let y = &mut x;
  |             ------ mutable borrow occurs here
4 |
5 |     println!("{} {}", x, y);
  |                       ^  - mutable borrow later used here
  |                       |
  |                       immutable borrow occurs here

error: aborting due to previous error

For more information about this error, try `rustc --explain E0502`.
error: could not compile `error-messages`.

To learn more, run the command again with --verbose.
```



![Une capture d'écran du terminal du message d'erreur 1.43.0.]

## Citations des équipes

Bien sûr, nous ne pouvons pas couvrir tous les changements qui se sont produits. Nous avons donc contacté et demandé à certaines de nos équipes de quels changements elles sont le plus fières:

> Pour rustdoc, les choses importantes étaient:
> - La documentation générée automatiquement pour les implémentations générales
> - La recherche elle-même et ses optimisations (la dernière étant de la convertir en JSON)
> - La possibilité de tester plus précisément les blocs de code doc "compile_fail, should_panic, allow_fail"
> - Les tests Doc sont désormais générés sous la forme de leurs propres binaires séparés.
> - Guillaume Gomez ( [rustdoc] )

> Rust a maintenant un support IDE de base! Entre IntelliJ Rust, RLS et rust-analyzer, je pense que la plupart des utilisateurs devraient pouvoir trouver une expérience "pas horrible" pour l'éditeur de leur choix. Il y a cinq ans, "écrire Rust" signifiait utiliser la configuration Vim / Emacs à l'ancienne.
> - Aleksey Kladov ( [IDE et éditeurs] )

> Pour moi, ce serait: ajouter un support de première classe pour les architectures intégrées populaires et créer un écosystème dynamique pour faire du développement de micro-contrôleurs avec Rust une expérience facile et sûre, mais amusante.
> - Daniel Egger ( [GT intégré] )

> L'équipe de publication n'existe que depuis (à peu près) début 2018, mais même pendant cette période, nous avons réussi ~ 40000 commits uniquement dans rust-lang / rust sans aucune régression significative dans stable.
> Compte tenu de la rapidité avec laquelle nous améliorons le compilateur et les bibliothèques standard, je pense que c'est vraiment impressionnant (bien que l'équipe de publication ne soit pas le seul contributeur ici). Dans l'ensemble, j'ai trouvé que l'équipe de publication a fait un excellent travail pour s'adapter au trafic croissant sur les trackers de problèmes, les PR en cours de dépôt, etc.
> - Mark Rousskov ( [Libération] )

> Au cours des 3 dernières années, nous avons réussi à transformer [Miri] d'un interprète expérimental en un outil pratique pour explorer la conception du langage et trouver des bogues dans du code réel - une excellente combinaison de théorie et de pratique PL. Sur le plan théorique, nous avons [Stacked Borrows] , la proposition la plus concrète pour un modèle d'alias de Rust à ce jour. Sur le plan pratique, alors qu'au départ, seules quelques bibliothèques clés ont été vérifiées dans Miri par nous, nous avons récemment constaté une grande adoption de personnes utilisant Miri pour [trouver et corriger des bogues](https://github.com/rust-lang/miri/#bugs-found-by-miri) dans leurs propres caisses et dépendances, et une adoption similaire chez les contributeurs améliorant Miri, par exemple. en ajoutant la prise en charge de l'accès au système de fichiers, du déroulement et de la concurrence.
> - Ralf Jung ( [Miri] )

> Si je devais choisir une chose dont je suis le plus fier, c'est le travail sur les vies non lexicales (NLL). Ce n'est pas seulement parce que je pense que cela a fait une grande différence dans la convivialité de Rust, mais aussi à cause de la façon dont nous l'avons implémenté en formant le groupe de travail NLL. Ce groupe de travail a attiré de nombreux contributeurs, dont beaucoup travaillent encore sur le compilateur aujourd'hui. L'Open Source à son meilleur!
> - Niko Matsakis ( [Langue] )

## La communauté

Comme la langue a beaucoup changé et grandi ces cinq dernières années, sa communauté aussi. Il y a eu tellement de grands projets écrits en Rust, et la présence de Rust en production a augmenté de façon exponentielle. Nous voulions partager quelques statistiques sur la croissance de Rust.

- Rust a été élue ["Programmation la plus appréciée"] chaque année lors des quatre dernières sondages auprès des développeurs de Stack Overflow depuis sa version 1.0.
- Nous avons servi plus de 2,25 pétaoctets (1 Po = 1 000 To) de différentes versions du compilateur, des outils et de la documentation cette année seulement!
- Dans le même temps, nous avons servi plus de 170 To de caisses pour environ 1,8 milliard de demandes sur crates.io, doublant le trafic mensuel par rapport à l'année dernière.

Lorsque Rust a atteint 1.0, vous pouviez compter d'une part le nombre d'entreprises qui l'utilisaient en production. Aujourd'hui, il est utilisé par des centaines d'entreprises technologiques, certaines des plus grandes entreprises technologiques telles qu'Apple, Amazon, Dropbox, Facebook, Google et Microsoft choisissant d'utiliser Rust pour ses performances, sa fiabilité et sa productivité dans leurs projets.

## Conclusion

De toute évidence, nous ne pouvions pas couvrir tous les changements ou améliorations apportés à Rust depuis 2015. Quels ont été vos changements préférés ou vos nouveaux projets Rust préférés? N'hésitez pas à publier votre réponse et votre discussion sur [notre forum Discours](TODO:%20CREATE%20FORUM%20POST%20BEFORE%20MERGE) .

Enfin, nous voulions remercier tous ceux qui ont contribué au Rust, que vous ayez contribué à une nouvelle fonctionnalité ou corrigé une faute de frappe, votre travail a fait de Rust l'étonnant qu'il est aujourd'hui. Nous avons hâte de voir comment Rust et sa communauté continueront de croître et de changer, et de voir ce que vous construirez tous avec Rust dans la prochaine décennie!


[annoncé un aperçu public de sa prise en charge officielle de Rust pour l'API WinRT!]: https://blogs.windows.com/windowsdeveloper/2020/04/30/rust-winrt-public-preview/
[1.2]: https://blog.rust-lang.org/2015/08/06/Rust-1.2.html
[1.3]: https://blog.rust-lang.org/2015/09/17/Rust-1.3.html
[1.4]: https://blog.rust-lang.org/2015/10/29/Rust-1.4.html
[1.5]: https://blog.rust-lang.org/2015/12/10/Rust-1.5.html
[1.6]: https://blog.rust-lang.org/2016/01/21/Rust-1.6.html
[1.10]: https://blog.rust-lang.org/2016/07/07/Rust-1.10.html
[1.12]: https://blog.rust-lang.org/2016/09/29/Rust-1.12.html
[1.13]: https://blog.rust-lang.org/2016/11/10/Rust-1.13.html
[1.14]: https://blog.rust-lang.org/2016/12/22/Rust-1.14.html
[1.15]: https://blog.rust-lang.org/2017/02/02/Rust-1.15.html
[1.17]: https://blog.rust-lang.org/2017/04/27/Rust-1.17.html
[1.20]: https://blog.rust-lang.org/2017/08/31/Rust-1.20.html
[1.24]: https://blog.rust-lang.org/2018/02/15/Rust-1.24.html
[1.26]: https://blog.rust-lang.org/2018/05/10/Rust-1.26.html
[1.28]: https://blog.rust-lang.org/2018/08/02/Rust-1.28.html
[1.31]: https://blog.rust-lang.org/2018/12/06/Rust-1.31-and-rust-2018.html
[1.34]: https://blog.rust-lang.org/2019/04/11/Rust-1.34.0.html
[1.39]: https://blog.rust-lang.org/2019/11/07/Rust-1.39.0.html
[Une capture d'écran du terminal du message d'erreur 1.43.0.]:  /images/2020-05-15-five-years-of-rust/borrow-error-1.2.0.png
[Miri]: /images/2020-05-15-five-years-of-rust/borrow-error-1.43.0.png
[Stacked Borrows]:    /images/2020-05-15-five-years-of-rust/help-error-1.2.0.png
[rustdoc]:   /images/2020-05-15-five-years-of-rust/help-error-1.43.0.png
[IDE et éditeurs]:   /images/2020-05-15-five-years-of-rust/trait-error-1.2.0.png
[GT intégré]:  /images/2020-05-15-five-years-of-rust/trait-error-1.43.0.png
[Libération]: https://github.com/rust-lang/miri
[Miri]: https://github.com/rust-lang/unsafe-code-guidelines/blob/master/wip/stacked-borrows.md
[Langue]:  https://github.com/rust-lang/miri/#bugs-found-by-miri
["Programmation la plus appréciée"]: https://www.rust-lang.org/governance/teams/rustdoc