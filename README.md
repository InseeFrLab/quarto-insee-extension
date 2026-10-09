# Changelog

::include{file=CHANGELOG}

# Présentation Quarto

Template de support de présentation dynamique en quarto markdown (.qmd) et reveal.js intégrant désormais {+la charte graphique de l'Insee 2025.+}

> Les dépendances dont a besoin ce template pour fonctionner correctement en local sont listées dans le fichier `renv.lock`.

{+Important :+} Si vous utilisez ce support **dans l'environnement LS3**, une image configurée et contenant l'ensemble des dépendances nécessaires au support est disponible [ici](https://onyxia.datascience.kube.insee.fr/launcher/ide/rstudio?name=presentation-quarto&version=0.2.0&rstudio.service.image.custom.enabled=true&rstudio.service.image.custom.version=%C2%ABgitlab-registry.insee.fr%2Fdatascience%2Fregistry-prod-custom%2Fonyxia-rstudio-presentations-quarto%3A0.0.1%C2%BB&rstudio.kubernetes.role=%C2%ABadmin%C2%BB&rstudio.init.personalInit=%C2%ABhttps%3A%2F%2Fgitlab.insee.fr%2Fe9lc05%2Fmonrstudio%2F-%2Fraw%2Fmaster%2Finit_rstudio.sh%3Fref_type%3Dheads%C2%BB&autoLaunch=true). Cette image vous évite de devoir faire tourner le script dev.R au préalable.

## :one: Changement de thème

Il n'y a plus de thème selon le public auquel on s'adresse. En effet, il n'y a désormais qu'un thème clair et un thème sombre.

Par défaut, la présentation est "branchée" sur la charte graphique du thème clair (Insee-clair). Un thème sombre verra peut être le jour ultérieurement.

:arrow_right: Les images des licences, des logos de chaque région et de chaque SSM, et les pictogrammes Insee sont disponibles dans le dossier `_extensions/inseefrlab/insee-clair/ressources/img/` de l'extension.

Pour changer de format, vous devez sélectionner le render associé au thème de la charte graphique de l'Insee que vous souhaitez utiliser, comme le montre l'image suivante :

![](img/renders_quarto.png)

> Si ces options ne sont pas disponibles et que le render ne produit qu'un fichier texte, sans style, alors il vous faudra monter de version votre RStudio. L'intégration de quarto dans RStudio se fait à partir de la version `v2022.07` de RStudio. De plus, il est {-fortement recommandé-} d'utiliser a minima la version `v2023.06` de Rstudio pour faire du quarto.

**Pour changer de thème dans la page gitlab**, il faut modifier la variable `COULEURFORMAT` présente dans le fichier `_variables.yml`.

Ainsi, le thème choisi en local dans Rstudio peut être différent de celui qu'on publie sur gitlab.

## :two: Génération des outputs

Le fait de faire l'action "Render" dans Rstudio entraîne, s'il n'existe pas, la création d'un dossier `_output/`, puis à chaque fois qu'on va générer le support, ce dossier sera mis à jour. Toutes les ressources dont ont besoin les fichiers html en sortie sont ainsi copiées dans ce dossier.

:warning: Si vous souhaitez ajouter des images par exemple dans votre support, il faut les ajouter dans le dossier `img` à la racine du projet et non dans le dossier `_output/img/`.

Si la génération de votre support de présentation échoue (page not found = page blanche dans le navigateur), alors il vous faudra copier dans le navigateur l'URL mentionnée dans l'onglet `Background Jobs` de Rstudio.

## :three: Les feuilles de style

Les feuilles de style en cascade (CSS) permettent la mise en forme du support. Cette mise en forme est répartie comme suit :

- `Insee_Commun.scss` : Style commun aux différents thèmes. Ce fichier, au format SCSS, permet de respecter la charte graphique de base de l'Insee.
- `Insee_clair.css` : Ces feuilles CSS sont dédiées aux styles propres à chacun des thèmes permettant de respecter la charte graphique de l'Insee.
- `default.css` : Ce fichier CSS regroupe l'ensemble des classes mises à votre disposition pouvant être utilisées pour modifier le style du support (couleurs, box, etc..) C'est probablement dans cette feuille que je rajouterai de nouveaux styles si besoin.

:warning: Ces feuilles de style n'intègrent pas toutes les classes qui sont à votre disposition. Quarto intègre nativement beaucoup de fonctionnalités dont l'utilisation présente des similitudes (ex: la classe **.fragment**). Rendez-vous sur la documentation officielle pour découvrir le champ des possibles. :wink:

## :four: L'export en PDF

Pour exporter le support en PDF, il faut suivre les instructions écrites dans les slides dédiées (fichier `05_exportPdf.qmd`).

Il y a du CSS dédié au support quand on passe en PDF export Mode (quand on tape e sur le clavier). Celui-ci est réparti dans plusieurs feuilles de style et débute à chaque fois par les classes `.pdf-page` ou `.print-pdf`. Il y a également du css spécifique quand on lance l'impression du support en PDF. Celui-ci se situe dans le scope suivant :

```css
@media print{
  ...
}
```

## :five: Déploiement du support

Le support de présentation est consultable sur internet via l'url suivante :

- sur gitlab : http://pole-bpe.gitlab-pages.insee.fr/presentations-formations/presentation-quarto>
- sur github : 

Il s'agit de l'uri présente dans le dépôt à la page suivante : "settings → pages". Une redirection automatique est faite vers la page principale du support de présentation, à savoir `index.qmd` ou `index.html`.

La branche déployée sur gitlab/github est définie dans le fichier `.gitlab-ci.yml` ou `.github/workflows/deploy.yml` dans le snippet ci-après :

```yml
only:
  - main
```

## :six: Précaution d'usage (interne)

- RStudio plante si vous essayez de changer de projet R ou de quitter RStudio alors que le render est encore en cours d'exécution (onglet {+ Background Jobs +} de RStudio). Pensez à stopper le processus avant de changer de projet R ou de quitter RStudio (bouton Stop).

![](img/background_jobs_rstudio.png)

- Ce projet fonctionne désormais sur AUS. Cependant, il subsiste un problème de "Render" lorsqu'on utilise les fonctionnalités suivantes :
  - mathJax pour les équations mathématiques
  - les popups

Ces problèmes viennent du fait que la librairie mathJax et les pages javascript des popups sont récupérées sur des serveurs (cdn) via internet, ce qui est bloqué par défaut sur AUS.

## :seven: Licence

Si vous souhaitez mettre le support sous licence libre, il est possible de faire apparaître le logo de la licence que vous aurez choisi dans la page de garde du diaporama. Pour cela, vous pouvez faire une issue dans ce dépôt gitlab. Une licence _open source_ permet de rendre le code source de votre présentation accessible, modifiable et redistribuable tout en respectant certaines conditions définies par la licence que vous aurez choisie, comme par exemple l'affichage de l'origine du code. Vous pouvez trouver des explications détaillées sur le principe des licences et le choix de l'une d'entre elles [ici](https://zestedesavoir.com/tutoriels/261/le-droit-dauteur-creative-commons-et-les-licences-sur-zeste-de-savoir/). Par exemple, pour indiquer que votre support est sous licence Common Creative BY-SA, vous pouvez modifier le fichier `_quarto.yml` en décommentant et modifiant la ligne 28 de la manière suivante :

```
logo: img/licences/cc/png/by-sa.png
```

## :eight: Contribution

Si vous avez remarqué des bugs, des dysfonctionnalités ou des points d'amélioration, n'hésitez pas à m'en faire part et à transmettre ces informations. Tout enrichissement est bon à prendre.

## :nine: Récupération du projet

### Avec quarto
Ce projet étant une extension quarto, vous pouvez démarrer une présentation au format Insee simplement en important le template. Pour cela, il suffit d'indiquer dans le terminal : 
`quarto use template inseefrlab/quarto-insee-extension`. Vous devrez ensuite indiquer si vous faites confiances à ce modèle (indiquer Yes), si voulez créer un dossier pour ce modèle (en général Oui) et, si oui, le nom du dossier. Le dossier sera le nom du repo. Avec l'extension sont installées plusieurs autres plugins qui ajoutent des fonctionnalités à la présentation : indiquer le nombre de slides (verticator), lancer des confettis (confetti) etc. 

### Manuellement

Pour récupérer le projet, vous pouvez :

- télécharger le .zip en cliquant ici : <https://gitlab.insee.fr/pole-bpe/presentations-formations/presentation-quarto/-/archive/main/presentation-quarto-main.zip> ou en vous rendant sur la page suivante : <https://gitlab.insee.fr/pole-bpe/presentations-formations/presentation-quarto/-/releases>

et à partir de ce zip, initier (ou pas) un nouveau dépôt sur gitlab dans votre groupe. Ce projet sera indépendant de celui-ci.

OU

- forker le projet si vous souhaitez ultérieurement y contribuer via des merges request. Pour cela, voici les étapes à suivre :
  1. Forker le projet template et ajouter le fork dans son groupe. Changer le nom du projet pour éviter toute confusion ultérieure entre les 2 projets (le projet forké et le projet initial qui s'appelle presentation-quarto).
  2. Ouvrer le projet forké via RStudio.(File -> New Project.. -> Version Control -> Git).
  3. Dans le terminal de RStudio, coller la ligne suivante : `git remote add upstream https://gitlab.insee.fr/pole-bpe/presentations-formations/presentation-quarto.git`. Vous pouvez vérifier l'ajout de cette remote en tapant `git remote -v`.
  4. Mettre à jour votre projet forké avec le projet initial en faisant : `git pull upstream main`. C'est via cette commande que vous pourrez rapatrier dans votre projet forké les corrections et évolutions faites sur ce projet.
  5. Vérifier que vous avez bien récupéré la branche main du dépôt initial en tapant `git branch -a` : la ligne suivante apparaît dans la console -> remotes/upstream/main.
  6. Mettre à jour sa branche main du dépôt forké : `git push`
  7. Réaliser votre ou vos propres supports de formation sur une ou plusieurs branches dédiées. Ne jamais travailler sur la branche main si vous souhaitez récupérer les possibles futures évolutions / corrections du dépôt initial.
