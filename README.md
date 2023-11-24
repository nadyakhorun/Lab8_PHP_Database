# Lab8_PHP_Database

Nama            : Nadya Khairunnisa

NIM             : 312210133

Kelas           : TI.22.A.1

Mata Kuliah     : Pemrograman Web 1

Dosen Pengampu  : Agung Nugroho, S.Kom., M.Kom

# Instruksi Praktikum

1. Persiapkan text editor misalnya VSCode.
   
2. Buat folder baru dengan nama lab8_php_database pada docroot webserver (htdocs)

3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.

# Langkah-Langkah Praktikum

# Persiapan

Untuk memulai membuat aplikasi CRUD sederhana, yang perlu disiapkan adalah database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.

# Menjalankan MySQL Server

Untuk menjalankan MySQL Server dari menu XAMPP Contol.

![xampp tugas 10](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/381de1ae-86b1-4b57-9e50-e87c70bd1222)

# Mengakses MySQL Client menggunakan PHP MyAdmin

Pastikan webserver Apache dan MySQL server sudah dijalankan. Kemudian buka melalui browser: http://localhost/phpmyadmin/

# Membuat Database: Studi Kasus Data Barang

# Membuat Database

        CREATE DATABASE latihan1;

# Membuat Tabel

          CREATE TABLE data_barang (
            id_barang int(10) auto_increment Primary Key,
            kategori varchar(30),
            nama varchar(30),
            gambar varchar(100),
            harga_beli decimal(10,0),
            harga_jual decimal(10,0),
            stok int(4)
          );

![phpmyadmin](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/41b0979e-e8bc-48de-9a40-e87f13346899)

# Menambahkan Data

          INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
          VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
          ('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
          ('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);

# Membuat Program CRUD

1. Buat folder lab8_php_database pada root directory web server (c:\xampp\htdocs)

![lab 8](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/d45ebda3-e1f2-4dc3-8b9f-a26b77ca01d7)

2. Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL: 
 http://localhost/lab8_php_database/

![php database](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/2608f525-9976-4a06-82c4-f77244fc9719)

# Membuat File Koneksi Database

1. Buat file baru dengan nama koneksi.php

            <?php
            $host = "localhost";
            $user = "root";
            $pass = "";
            $db = "latihan1";
            
            $conn = mysqli_connect($host, $user, $pass, $db);
            if ($conn == false)
            {
                echo "Koneksi ke server gagal.";
                die();
            } #else echo "Koneksi berhasil";
            ?>

2. Buka melalui browser untuk menguji koneksi database (untuk menyampilkan pesan koneksi berhasil, uncomment pada perintah echo “koneksi berhasil”;
   
![koneksi berhasil](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/7ba22645-2319-4805-9ba7-c5f0708ff313)

# Membuat File Index Untuk Menampilkan Data (Read)

* Buat file baru dengan nama index.php

                <?php
                include("koneksi.php");
                
                // query untuk menampilkan data
                $sql = 'SELECT * FROM data_barang';
                $result = mysqli_query($conn, $sql);
                
                ?>
                <!DOCTYPE html>
                <html lang="en">
                <head>
                      <meta charset="UTF-8">
                      <link href="style.css" rel="stylesheet" type="text/css" />
                      <title>Data Barang</title>
                </head>
                <body>
                    <div class="container">
                        <h1>Data Barang</h1>
                        <div class="main">
                              <table>
                              <tr>
                                    <th>Gambar</th>
                                    <th>Nama Barang</th>
                                    <th>Katagori</th>
                                    <th>Harga Jual</th>
                                    <th>Harga Beli</th>
                                    <th>Stok</th>
                                    <th>Aksi</th>
                            </tr>
                            <?php if($result): ?>
                            <?php while($row = mysqli_fetch_array($result)): ?>
                            <tr>
                                    <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
                $row['nama'];?>"></td>
                                    <td><?= $row['nama'];?></td>
                                    <td><?= $row['kategori'];?></td>
                                    <td><?= $row['harga_beli'];?></td>
                                    <td><?= $row['harga_jual'];?></td>
                                    <td><?= $row['stok'];?></td>
                                    <td><?= $row['id_barang'];?></td>
                              </tr>
                              <?php endwhile; else: ?>
                              <tr>
                                  <td colspan="7">Belum ada data</td>
                              </tr>
                              <?php endif; ?>
                              </table>
                          </div>
                    </div>
                </body>
                </html>

![index1](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/f07c31c4-b161-43b4-8aaa-e1c0038475c6)

# Menambahkan Data (Create)

* Buat file baru dengan nama tambah.php

          <?php
          error_reporting(E_ALL);
          include_once 'koneksi.php';
          if (isset($_POST['submit']))
          {
              $nama = $_POST['nama'];
              $kategori = $_POST['kategori'];
              $harga_jual = $_POST['harga_jual'];
              $harga_beli = $_POST['harga_beli'];
              $stok = $_POST['stok'];
              $file_gambar = $_FILES['file_gambar'];
              $gambar = null;
              if ($file_gambar['error'] == 0)
              {
                  $filename = str_replace(' ', '_',$file_gambar['name']);
                  $destination = dirname(__FILE__) .'/gambar/' . $filename;
                  if(move_uploaded_file($file_gambar['tmp_name'], $destination))
                  {
                        $gambar = 'gambar/' . $filename;;
                  }
              }
              $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli,
          stok, gambar) ';
              $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}',
          '{$harga_beli}', '{$stok}', '{$gambar}')";
              $result = mysqli_query($conn, $sql);
              header('location: index.php');
          }
          ?>
          <!DOCTYPE html>
          <html lang="en">
          <head>
                <meta charset="UTF-8">
                <link href="style.css" rel="stylesheet" type="text/css" />
                <title>Tambah Barang</title>
          </head>
          <body>
          <div class="container">
                <h1>Tambah Barang</h1>
                <div class="main">
                    <form method="post" action="tambah.php"
          enctype="multipart/form-data">
                  <div class="input">
                        <label>Nama Barang</label>
                        <input type="text" name="nama" />
                  </div>
                  <div class="input">
                        <label>Kategori</label>
                        <select name="kategori">
                            <option value="Komputer">Komputer</option>
                            <option value="Elektronik">Elektronik</option>
                            <option value="Hand Phone">Hand Phone</option>
                        </select>
                      </div>
                      <div class="input">
                          <label>Harga Jual</label>
                          <input type="text" name="harga_jual" />
                      </div>
                      <div class="input">
                          <label>Harga Beli</label>
                          <input type="text" name="harga_beli" />
                      </div>
                      <div class="input">
                          <label>Stok</label>
                          <input type="text" name="stok" />
                      </div>
                      <div class="input">
                          <label>File Gambar</label>
                          <input type="file" name="file_gambar" />
                      </div>
                      <div class="submit">
                          <input type="submit" name="submit" value="Simpan" />
                      </div>
                  </form>
              </div>
          </div>
          </body>
          </html>

![tambah](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/99ddbd02-a89c-4c79-b92a-07d0b6e11e3b)

# Mengubah Data (Update)

* Buat file baru dengan nama ubah.php

                  <?php
                  error_reporting(E_ALL);
                  include_once 'koneksi.php';
                  if (isset($_POST['submit']))
                  {
                  $id = $_POST['id'];
                  $nama = $_POST['nama'];
                  $kategori = $_POST['kategori'];
                  $harga_jual = $_POST['harga_jual'];
                  $harga_beli = $_POST['harga_beli'];
                  $stok = $_POST['stok'];
                  $file_gambar = $_FILES['file_gambar'];
                  $gambar = null;
                  if ($file_gambar['error'] == 0)
                  {
                  $filename = str_replace(' ', '_', $file_gambar['name']);
                  $destination = dirname(_FILE_) . '/gambar/' . $filename;
                  if (move_uploaded_file($file_gambar['tmp_name'], $destination))
                  {
                  $gambar = 'gambar/' . $filename;;
                  }
                  }
                  $sql = 'UPDATE data_barang SET ';
                  $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
                  $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok
                  = '{$stok}' ";
                  if (!empty($gambar))
                  $sql .= ", gambar = '{$gambar}' ";
                  $sql .= "WHERE id_barang = '{$id}'";
                  $result = mysqli_query($conn, $sql);
                  if (!$result) {
                      die('Error: ' . mysqli_error($conn));
                  } else {
                      header('location: index.php');
                  }
                  }
                  $id = $_GET['id'];
                  $sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
                  $result = mysqli_query($conn, $sql);
                  if (!$result) die('Error: Data tidak tersedia');
                  $data = mysqli_fetch_array($result);
                  function is_select($var, $val) {
                  if ($var == $val) return 'selected="selected"';
                  return false;
                  }
                  ?>
                  <!DOCTYPE html>
                  <html lang="en">
                  <head>
                        <meta charset="UTF-8">
                        <link href="style.css" rel="stylesheet" type="text/css" />
                        <title>Ubah Barang</title>
                  <style>
                  body {
                              font-family: Arial, sans-serif;
                              background-color: #f4f4f4;
                              margin: 0;
                              padding: 0;
                          }
                  
                  input[type=text], select, textarea {
                    width: 100%;
                    padding: 8px;
                    border: 1px solid #ccc;
                    box-sizing: border-box;
                  }
                  
                  label {
                    padding: 12px 12px 12px 0;
                    display: inline-block;
                  }
                  
                  input[type=submit] {
                    background-color: #0000FF;
                    color: white;
                    padding: 12px 20px;
                    border: none;
                    border-radius: 4px;
                    cursor: pointer;
                  }
                  
                  input[type=submit]:hover {
                    background-color: #4169E1;
                  }
                  
                  .container {
                    border-radius: 5px;
                    background-color: #f2f2f2;
                    padding: 20px;
                  }
                  </style>
                  </head>
                  <body>
                  <div class="container">
                      <h1>Ubah Barang</h1>
                      <div class="main">
                            <form method="post" action="ubah.php"
                  enctype="multipart/form-data">
                                <div class="input">
                                    <label>Nama Barang</label>
                                    <input type="text" name="nama" value="<?php echo
                  $data['nama'];?>" />
                                </div>
                                <div class="input">
                                    <label>Kategori</label>
                                    <select name="kategori">
                                    <option <?php echo is_select
                  ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                  <option <?php echo is_select
                  ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                  <option <?php echo is_select
                  ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                                      </select>
                                  </div>
                                  <div class="input">
                                      <label>Harga Jual</label>
                                      <input type="text" name="harga_jual" value="<?php echo
                  $data['harga_jual'];?>" />
                                  </div>
                                  <div class="input">
                                      <label>Harga Beli</label>
                                      <input type="text" name="harga_beli" value="<?php echo
                  $data['harga_beli'];?>" />
                                  </div>
                                  <div class="input">
                                      <label>Stok</label>
                                      <input type="text" name="stok" value="<?php echo
                  $data['stok'];?>" />
                                  </div>
                                  <div class="input">
                                      <label>File Gambar</label>
                                      <input type="file" name="file_gambar" />
                                  </div>
                                  <div class="submit">
                                  <input type="hidden" name="id" value="<?php echo
                  $data['id_barang'];?>" />
                                  <input type="submit" name="submit" value="Simpan" />
                              </div>
                          </form>
                      </div>
                  </div>
                  </body>
                  </html>

![ubah](https://github.com/nadyakhorun/Lab8_PHP_Database/assets/115801823/96b8c485-1278-45c8-9ed0-b4a2493c117c)

# Menghapus Data (Delete)

* Buat file baru dengan nama hapus.php

          <?php
          include_once 'koneksi.php';
          $id = $_GET['id'];
          $sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
          $result = mysqli_query($conn, $sql);
          header('location: index.php');
          ?>

# Sekian dan Terima Kasih
