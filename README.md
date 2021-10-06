# Projet Portfolio - Back-Office

## Contexte

Dans le cadre du développement de votre Portfolio, le besoin porte sur la conception et le développement d'un back-office permettant d'administrer la publication de projets, puis d'une interface publique permettant de consulter les projets publiés.

## Conception de la base de données

La première étape d'un développement back-end est la conception de la base de données. Sans celle-ci, vous n'avez pas la possibilité de pouvoir avancer dans le développement convenablement.

Dans phpMyAdmin, créez une nouvelle base de données nommée `backoffice` et choisissez pour l'encodage des caractères `utf8_general_ci`.  
Créez une nouvelle table `projects` dans cette base de données. Structurez celle-ci :

* projects :
  * id (pk)
  * title (varchar 255)
  * description (text)
  * picture (varchar 255)
  * created_at (datetime)

Pour chaque nouvelle entrée (à chaque nouvelle requête d'insertion), le champs `id` s'auto-incrémente, les autres champs de l'entrée sont alimentées par les informations transmises par le formulaire.

Vous aurez également besoin d'un table `users` afin de sécurisé l'administration du portfolio :

* users :
  * id (pk)
  * username (varchar 255)
  * password (varchar 255)

## Connexion à la base de données

Créez une page dédiée à la seule connexion à la base de données, la connexion s'opère grâce à l'objet PDO. La page contient 4 variables, dont les valeurs seront à réadapter au moment du passage en production : (commencez par déclarer que ce qui suivra est du PHP en ouvrant la balise `<?php`)

```php
$db_host = "localhost";
$db_name = "backoffice";
$db_user = "root";
$db_pwd = "";
```

La page établit ensuite la connexion :

```php
try {
    $db = new PDO("mysql:host=$db_host;dbname=$db_name", $db_user, $db_pwd);
    // set the PDO error mode to exception
    $db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "Connected successfully";
} catch(PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
}
```

Si le message « Connected successfully » apparaît, c'est que tout s'est bien passé. On peut ensuite passer en commentaire l'instruction echo `"Connected successfully";` qui n'a qu'un caractère informatif pour le développeur.

## Création d'un utilisateur administrateur

Le back-office doit être fermé au public et accessible seulement aux personnes possédant un `username`et `mot de passe`. Pour cela, il va falloir enregistrer le premier utilisateur qui n'est d'autre que vous.

Dans un dossier situé à la racine de votre portfolio, créez un dossier nommé `back`. Dans ce même dossier, créez une page `register.php` contenant un formulaire. Ce formulaire enverra les données vers une page nommée `addUser.php`. Dans cette page, faites en sorte d'insérer les données reçues dans la table `users` et n'oubliez surtout pas d'effectuer un hash sur le mot de passe en utilisant la fonction PHP `password_hash()`.
