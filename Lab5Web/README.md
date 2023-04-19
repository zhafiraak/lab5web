# Tugas Pemograman Web 2
## Profil
| #               | Biodata           |
| --------------- | ----------------- |
| **Nama**        | Basri Sangaji     |
| **NIM**         | 312110152         |
| **Kelas**       | TI.21.A.1         |
| **Mata Kuliah** | Pemrograman Web 2 |

# Persiapan 
1. Persiapkan Text Editor misalnya VSCode.
2. Buat folder baru dengan nama Lab5Web pada root Web Server (htdocs).
3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.

## Class Dasar
- Buat file baru dengan nama `mobil.php`. Kemudian masukan kode berikut.

```php
<?php

class Mobil
{
    private $warna;
    private $merk;
    private $harga;

    public function __construct()
    {
        $this->warna = "Hitam";
        $this->merk = "SSC_Tuatara";
        $this->harga = "29500000000";
    }

    public function set_warna($value)
    {
        $this->warna = $value;
    }

    public function get_warna()
    {
        return "Warna mobilnya adalah " . $this->warna;
    }
}

$mobil_pertama = new Mobil();
$mobil_kedua = new Mobil();
?>

<h2>Mobil Pertama</h2>

<?= $mobil_pertama->get_warna(); ?>

<h2>Mobil Pertama ganti warna</h2>

<?php
$mobil_pertama->set_warna("Merah");
echo $mobil_pertama->get_warna(); 
?>

<h2>Mobil Kedua</h2>

<?php
$mobil_kedua->set_warna("Biru");
echo $mobil_kedua->get_warna();
```

- Maka hasilnya seperti berikut.

![Mobil](img/mobil.png)

## Membuat Form (Class Library)
<p>Class library merupakan pustaka kode program yang dapat digunakan bersama pada beberapa file
yang berbeda (konsep modularisasi). Class library menyimpan fungsi-fungsi atau class object
komponen untuk memudahkan dalam proses development aplikasi.</p>

- Buat file baru dengan nama `form-input.php`. Kemudian masukan kode berikut.

```php
<?php
require_once 'includes/database.php';
require_once 'includes/form.php';

$db = new Database();
$conn = $db->getConnection();
$form = new Form($conn);
?>

<!DOCTYPE html>
<html>
<head>
	<title>Modular Lab5Web</title>
  <link rel="stylesheet" type="text/css" href="includes/form.css">
</head>
<body>
	<h1>Modular Lab5Web</h1>
	<?php
	if ($_SERVER['REQUEST_METHOD'] == 'POST') {
		$form->processForm($_POST);
	}
	$form->displayForm();
	?>
</body>
</html>
```

- Buat folder baru di dalam folder `Lab5Web` dengan nama `includes`.
- Kemudian, buat file baru di dalam folder `includes` dengan nama `config.php`.
- Isi file `config.php` dengan kode berikut:

```php
<?php
define('DB_HOST', 'localhost');
define('DB_USER', 'root');
define('DB_PASS', '');
define('DB_NAME', 'latihan1');
?>
```

- Kemudian, buat koneksi ke database pada file `database.php` dengan kode berikut:

```php
<?php
require_once 'config.php';

class Database {
	private $conn;

	public function __construct() {
		$this->conn = new mysqli(DB_HOST, DB_USER, DB_PASS, DB_NAME);
		if ($this->conn->connect_error) {
			die('Connection failed: ' . $this->conn->connect_error);
		}
	}

	public function getConnection() {
		return $this->conn;
	}
}
?>
```

- Lalu buat file baru di dalam folder `includes` dengan nama `form.php`. Dan masukan kode berikut.

```php
<?php
class Form {
	private $conn;

	public function __construct($conn) {
		$this->conn = $conn;
	}

	public function displayForm() {
		echo '<form method="POST" action="' . htmlspecialchars($_SERVER["PHP_SELF"]) . '">';
		echo '<label for="nim">NIM:</label>';
		echo '<input type="number" name="nim" required><br>';
		echo '<label for="nama">Nama:</label>';
		echo '<input type="text" name="nama" required><br>';
		echo '<label for="alamat">Alamat:</label>';
		echo '<input type="text" name="alamat" required><br>';
		echo '<input type="submit" value="Submit">';
		echo '</form>';
	}

	public function processForm($data) {
		$nim = $data['nim'];
		$nama = $data['nama'];
		$alamat = $data['alamat'];

		$sql = 'INSERT INTO data_mahasiswa (nim, nama, alamat) VALUES (?, ?, ?)';
		$stmt = $this->conn->prepare($sql);
		$stmt->bind_param('sss', $nim, $nama, $alamat);
		$stmt->execute();
	}
}
?>
```

- Kemudian tambahkan sedikit css agar terlihat rapih dan menarik.
- Buat file baru dengan nama `form.css` di dalam folder `includes`.
- Isi file `form.css` dengan kode berikut:

```css
form {
  margin: 20px;
  padding: 20px;
  border: 2px solid #ccc;
  border-radius: 10px;
  background-color: #f5f5f5;
}

label {
  display: inline-block;
  width: 80px;
  text-align: right;
  margin-right: 10px;
}

input[type="text"],
input[type="number"] {
  width: 200px;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
  margin-bottom: 10px;
}

input[type="submit"] {
  padding: 10px;
  margin-left: 235px;
  border-radius: 5px;
  border: none;
  background-color: #4CAF50;
  color: #fff;
  font-size: 15px;
  cursor: pointer;
}

input[type="submit"]:hover {
  background-color: #3e8e41;
}
```

- Maka hasilnya akan seperti ini.

![Form](img/form.png)

- Dengan menggunakan konsep modularisasi dan class library untuk form dan database connection, kode program menjadi lebih terstruktur, mudah dipelihara, dan mudah dikembangkan di masa depan. Jika ada perubahan pada form atau database connection, kita hanya perlu mengubah kode pada class library-nya tanpa harus mengubah kode pada file utama.

# Terima Kasih!!!
