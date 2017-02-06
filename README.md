# Programmiertechnik Labor PHP 4. Klasse

## Datenbank Verbindng öffnen

```
database.php
```
```php
<?php

// Datenbankverbidnung aufbauen
$connection = new MySqli('localhost', 'root', '', 'cinema');
$connection->set_charset('utf8'); // Setzen der Encoding auf utf8, damit Umlaute nicht kaputt sind.  
```

----
## Alle Zeilen einer SQL Anweisung Ausgeben
```
index.php
```
```php

<?php
    require "database.php";
    $sql = "SELECT * FROM movie";
    $stmt = $connection->prepare($sql); 
    $stmt->execute();
    $result = $stmt->get_result();
    $rows = $result->fetch_all(MYSQLI_ASSOC);
?>
<table>
  <tr>
    <th>Id</th>
    <th>Name</th>
    <th>Kinostart</th>
  </tr>
  <?php foreach($rows as $row): ?>
    <tr>
      <td><?php echo $row['mov_id'] ?></td>
      <td><?php echo $row['mov_name'] ?></td>
      <td><?php echo $row['mov_startDate'] ?></td>
    </tr>
  <?php endforeach; ?>
</table>

```
----
# Neuen Eintrag hinzufügen
```
insert.php
```
```php
<?php
    require "database.php";

    if(isset($_POST['mov_name'])) {

        $sql = 'INSERT INTO movie
            (mov_name, mov_startDate, dir_id, gen_id)
            VALUES (?, ?, ?, ?)';

        $stmt = $connection->prepare($sql);

        $stmt->bind_param('ssii',
            $_POST['mov_name'],
            $_POST['mov_startDate'],
            $_POST['dir_id'],
            $_POST['gen_id']
        );

        $stmt->execute();
        $insertId = $stmt->insert_id;
    }
?>

<h1>Neuen Film hinzufügen</h1>

<form method="POST">
    <div>
        <p>Name</p>
        <input type="text" name="mov_name" required />
    </div>

    <div>
        <p>Kinostart</p>
        <input type="datetime-local" name="mov_startDate" required />
    </div>

    <input type="submit" name="save" title="Speichern" />
</form>
```


## % Escapen

```
$searchString = $connection->real_escape_string($_POST["parameter"]);
$searchString = str_replace("%", "\\%", $searchString);
$searchString = str_replace("_", "\\_", $searchString);

$bind = "%" . $searchString . "%";

$sql = "SELECT * FROM dual where col like ?";
$stmt = $connection->prepare($sql);
$stmt->bind_param("s", $bind);
$stmt->execute();
$result = $stmt->get_result();
$rows = $result->fetch_all(MYSQLI_ASSOC);
```

## Select mit Optionen

```
<?php
$sql = "select * from zimmertyp";
$stmt = $connection->prepare($sql);
$stmt->execute();
$result = $stmt->get_result();
$rows = $result->fetch_all(MYSQLI_ASSOC);
?>

<form method="get" action="zimmertypen.php">
    Zimmertyp:
    <select name="typ">
        <?php foreach($rows as $row): ?>
            <option value='<?=$row["id"]?>'><?=$row["zimmer_typ"]?></option>";
        <?php endforeach; ?>
    </select>

    <input type="submit">Submit</input>
</form>
```