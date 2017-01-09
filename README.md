# pgtl-4-php

## Datenbank Verbindng öffnen

```
database.php
```
```php
<?php

$connection = new MySqli('localhost', 'root', '', 'cinema');
$connection->set_charset('utf8');
```

----
## Alle Zeilen einer SQL Anweisungausgeben (SELECT)
```
index.php
```
```php

<?php
    require "database.php"; // verwendes des 
    $sql = "SELECT * FROM movie";
    $result = $connection->query($sql);
    $rows = $result->fetch_all(MYSQL_ASSOC);
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
