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
# 
