# FaculProjects
# INDEX.PHP

 <?php
require_once __DIR__ ."/db.php";

$pdo->exec( "CREATE TABLE IF NOT EXISTS cadastro(id INT AUTO_INCREMENT PRIMARY KEY, Nome VARCHAR(100) NOT NULL, Endereco VACHAR(150) NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, ENGINE=INNODB DEFAULT CHARSET=utf8mb4");
$erro = '';
if($_SERVER ['REQUEST_METHOD'] == 'POST') {
    $Nome = $_POST ['nome'] ?? '';
    $Endereco = $_POST ['endereco'] ??'';
    if($Nome == '' || $Endereco == '') 
        $erro = 'Preencha os campos.';
    }else{
        $stms = $pdo->prepare('INSERT INTO cadastro(nome,endereco) VALUES (:nome, :endereco)');
        $stms->execute([
            ':nome'=> $Nome,
            ':endereco'=>$Endereco
        ]);
        header('Location: ' .$_SERVER
        ['PHP_SELF']);
        exit();
    }
?>
#  DB.PHP

 
<?php

$DB_HOST = "localhost"; 
$DB_NAME = "cad";
$DB_USER = "root";
$DB_PASS = "";
$dns = "mysql:host=$DB_HOST;dbname=$DB_NAME;charset=utf8mb4; ";

$options = [
    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    PDO::ATTR_EMULATE_PREPARES => false,
];
try{
    $pdo = new PDO($dns, $DB_USER, $DB_PASS, $options);
}catch(PDOException $e){
    exit("Erro ao conectar ao banco". $e->getMessage());
}
