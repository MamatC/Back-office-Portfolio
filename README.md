# Projet Portfolio - Back-Office

## Contexte

Dans le cadre du développement de votre Portfolio, le besoin porte sur la conception et le développement d'un back-office permettant d'administrer la publication de projets, puis d'une interface publique permettant de consulter les projets publiés.

## Conception de la base de données

La première étape d'un développement back-end est la conecption de la base de données. Sans celle-ci, vous n'avez pas la possibilité de pouvoir avancer dans le développement convenablement.

Dans phpMyAdmin, créez une nouvelle base de données nommée « backoffice » et choisissez pour l'encodage des caractères « utf8_general_ci ».  
Créez une nouvelle table « projects » dans cette base de données. Structurez celle-ci :

* projects :
  * id (pk)
  * title (varchar 255)
  * description (text)
  * picture (varchar 255)

Pour chaque nouvelle entrée (à chaque nouvelle requête d'insertion), le champs « id » s'auto-incrémente, les autres champs de l'entrée sont alimentées par les informations transmises par le formulaire.

## Connexion à la base de données

Créer une page dédiée à la seule connexion à la base de données, la connexion s'opère grâce à l'objet PDO. La page contient 4 variables, dont les valeurs seront à réadapter au moment du passage en production : (commencez par déclarer que ce qui suivra est du PHP en ouvrant la balise <?php, par contre il est inutile de fermer cette balise)

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
