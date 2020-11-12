# Développeur web back-end

## Mise en place de l'environnement

Afin de travailler dans de bonnes conditions j'ai installé sur ma machine Visual Studio Code, avec les extensions "PHP IntelliSense", et "PHP Debug".       
Sur le serveur, j'ai installé Apache (dans le TP Administrateur Système).       
J'ai ensuite installé XAMPP, pour se faire, j'ai télécharger l'installateur sur le site officiel de [XAMPP](https://www.apachefriends.org/fr/index.html).                
J'ai ensuite mis, à l'aide d'un SFTP, l'installateur dans /root/, et je l'ai lancé à l'aide des commandes suivantes :   
`chmod +x xampp-linux-*-installer.run` - On donne la permissions au fichier de s'executer.      
`./xampp-linux-*-installer.run` - On execute le fichier.        

Ensuite, pour démarrer XAMPP, j'ai effectuer la commande suivante `/opt/lampp/lampp start`      
### Notes :
Pour arrêter XAMPP, j'effectue la commande suivante `/opt/lampp/lampp stop`     
Pour vérifier le status de XAMPP, j'effectue la commande suivante `/opt/lampp/lampp status`         

J'ai également installer XAMPP en local, sur ma machine personnelle Windows. 

Commande la correction : `apt install php libapache2-mod-php php-mysql`

## Syntaxe

Afin d'afficher `Hello world` sur ma première page PHP, je vais dans `/xampp/htdocs`.     
J'ai ensuite créé un dossier `test`, où j'ai créé un dossier `index.php`.      
Pour afficher le `Hello world`, j'ai écris dans le dossier
```php
<?php
echo "Hello world";
```     
Dans la barre de recherche, je tape `localhost/tests/index.php`, et je vois donc afficher "Hello world".

## Debogage

Afin de voir les erreurs dans PHP, je dois modifier le configuration, afin de la trouver, j'ajoute `phpInfo()` à mon code, ce qui donne

```php
<?php
echo "Hello world";
phpInfo();
?>
```    
Je vois donc afficher une tonne d'information, mais une seule m'interesse, **Loaded Configuration File**, je l'ai trouvé, et je m'aperçois que le fichier de configuration est `	C:\xampp\php\php.ini`.

J'ouvre donc le fichier, et le modifie.     
Afin de trouver la partie qui m'intéresse, je cherche `error_reporting`, je trouve donc, à la ligne 465 :
```ini
error_reporting=E_ALL & ~E_DEPRECATED & ~E_STRICT
```
Je modifie la valeur en :
```ini
error_reporting=E_ALL
```

Et je m'assure que, un peu plus bas, le `display_errors` est bien sur On.
Après avoir modifié et sauvegarder le fichier, je redémarre XAMPP, afin qu'il prenne en compte les modifications. 

Afin de tester, je fais exprès de faire une erreur de syntaxe, j'écris :
```php
<?php
echo "Hello world";
phpInfo(;
?>
```  
Lorsque je veux afficher `index.php`, je vois
```
Parse error: syntax error, unexpected ';' in C:\xampp\htdocs\tests\index.php on line 4
```

Cela m'indique qu'il y a une erreur de syntaxe, que `;` n'étais pas attendu dans `C:\xampp\htdocs\tests\index.php` à la ligne **4**

## Bases de la programmation

### Les variables

Afin de déclarer une variable, j'utilise `$`        
Voici quelques variables que j'ai déclaré

```php
<?php
echo "Hello world";

$variable1 = "Bonjour, ceci est ma première variable"; // Je définit ma première variable, qui est une chaine de caractères
$age = 18;              // Ma deuxième variable, qui est un nombres
$surname = "Ettouil";   // Encore une chaîne de caractère
$name = "Adel";
$city = "Charleville-Mézières";
$isMajeur = true;       // Ceci est une valeure booléenne
?>
```
### Conditions
J'ai ensuite fais de la concaténation, et j'ai utilisé des conditions (if, elseif, else, switch)

Mon code ressemble désormais à ça :
```php
<?php
echo "Hello world <br>";

$variable1 = "Bonjour, ceci est ma première variable"; // Je définit ma première variable, qui est une chaine de caractères
$age = 18;              // Ma deuxième variable, qui est un nombres
$surname = "Ettouil";   // Encore une chaîne de caractère
$name = "Adel";
$city = "Charleville-Mézières";
$isMajeur = false;       // Ceci est une valeure booléenne

echo "L'âge de " .$name. " " .$surname. " est de " .$age. " et il habite à " .$city. ".<br>";

echo "<h2>Structure if</h2>";
// Detecter si la personne est majeure avec IF
if($isMajeur == true) // Si la variable $isMajeur est vraie
    echo "Oui il est majeur <br>";
else // Sinon
    echo "Non, il n'est pas majeur <br>";

echo "<h2>Structure switch</h2>";
// Detecter si la personne est majeure avec SWITCH
switch($isMajeur) {
    case true:
        echo "Oui, il est majeur <br>";
        break;
    case false:
        echo "Non, il n'est pas majeur <br>";
        break;
}
?>
``` 

### Boucles

J'ai ensuite créé des boucles, d'abord avec la table while, puis avec la table for, voici le code PHP que j'ai ajouté.

```php
$nombre_de_lignes = 1; // On définit la valeur
while ($nombre_de_lignes <= 10) { // Tant que le nombre de lignes est inférieur ou égal à 10
    echo 'Je ne dois pas regarder les mouches voler quand j\'apprends le PHP.<br />'; // On affiche un msg
    $nombre_de_lignes++; // On ajoute un aux nombre de lignes (à chaque phrase)
}
for ($i = 1; $i <= 10; $i++) { // On définit $i, puis tant que $i est inférieur ou égal à 10, à chaque fois on ajoute un à $i
    echo $i. " "; // On affiche $i
}
```

### Tableaux

J'ai donc créé un tableau, et je l'ai affiché (clé & valeur), voici le code PHP que j'ai ajouté

```php
$ages = array(
    "Adel" => 18,       // "Adel" est une clé, et 18 est sa valeur
    "Matheo" => 19,
    "Père" => 44,
    "Mère" => 41,
); // On définit un tableau avec des âges

foreach($ages as $key => $value) { // pour chaque clé & valeur
    echo $key. " a " .$value. " ans<br>"; // on affiche clé a valeur ans
}
```

### Fonctions

J'ai appelé, et créé des fonctions, voici le code PHP créé
```php
// Pour appeler une fonction, on marque son nom avec (). dans les parenthèse on peux y ajouter des paramètres.
// Je vais désormais appeler une fonction existante "date" afin d'afficher la date d'ajd

$jour = date('d'); // date est le nom de la fonction, 'd' est le paramètre
$mois = date('m');
$annee = date('Y');

$heure = date('H');
$minute = date('i');

// Maintenant on peut afficher ce qu'on a recueilli
echo "Nous sommes le " .$jour. "/" .$mois. "/" .$annee. " et il est " .$heure. "h" .$minute. "<br>";

// Pour créer une fonction je tape function .. le nom de ma fonction (). Je peux également insérer des paramètres dans les parenthèses
// Exemple : une fonction qui dis bonjour $prénom nous sommes le date

function hello($name) {
    $jour = date('d');
    $mois = date('m');
    $annee = date('Y');

    $heure = date('H');
    $minute = date('i');
    echo "Bonjour " .$name. " nous sommes le " .$jour. "/" .$mois. "/" .$annee. " et il est " .$heure. "h" .$minute. "<br>";
}

// J'apelle ensuite la fonction

hello("Adel");
hello("Mathéo");

// Désormais, je vais chercher à récupérer une valeur   

function ajoute($value1, $value2) { // Ma fonction consiste à ajouter valeur1 à valeur2 et retourner une valeur finale
    $finalvalue = $value1 + $value2;
    return $finalvalue;
}

$moncalcul = ajoute(5, 6); // La valeur $moncalcul prendra la valeur retournée dans la fonction ajoute()
echo "La valeure finale est donc " .$moncalcul. " et je peux l'utiliser dans mon code comme je le souhaite<br>";
?>
```

## Transmettre, stocker et récupérer des données

En PHP, on peux transmettre des données dans l'URL.     
On peux également les transmettre via un formulaire (c'est la méthode que nous utiliserons).       

Pour envoyer les données, je peux utiliser deux méthodes `post` et `get`. Avec `get` les données seront transmises dans l'URL, mais ce n'est pas sécurisé, et l'utilisateur peut modifier les données. C'est pour cela que nous utiliseras la méthode `post` même si elle n'est pas plus sécurisée, il est préférable.

On peux récupérer des données avec `$_GET` ou `$_POST`.

Voici le code utilisé pour ma page de login

**Page de login :**
```html
<form action="cible.php" method="post">
    Bienvenue, veuillez-vous identifier <br><br>
    <input type="text" name="login" placeholder="Votre login"/>
    <input type="password" name="password" placeholder="Votre mot de passe"/>
    <input class="btn2 success" type="submit" value="Se connecter" />
</form>
```
**Page cible**

```php
<strong>
    Merci de vous être identifié<br><br>
</strong>
<?php
$login = $_POST['login'];
$mdp = $_POST['password'];
if($login == "secretaire" AND $mdp == "sio@secretaire2020") {
    include("includes/secretaire.php");
} elseif($login == "evaluateur" AND $mdp == "sio@eval2020") {
    include("includes/evaluateur.php");
} elseif($login == "admin" AND $mdp == "sio@admin2020") {
    include("includes/admin.php");
} else {
    echo "Erreur <br> Identifiant ou mot de passe invalide <br><br> <input class='btn2 danger' type='button' value='Retour' onclick='document.location.href=`index.php`'";
}
?>
```

