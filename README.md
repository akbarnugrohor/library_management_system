# library_management_system_pacmann_project

## Tujuan pengerjaan project :
Pembuatan Sistem Manajemen Perpustakaan Sederhana

## Deskripsi Task :
1. Importing library yang dibutuhkan :
```
import mysql.connector
import random
import pandas as pd
```

2. Melakukan Connect ke database menggunakan sintaks :
```
mydb = mysql.connector.connect(
    host = "localhost",
    user = "root",
    password = "rianto08", ##password tergantung pembuatan password MySQL Workbench anda
    database = "lms", ##nama database/schema di MySQL anda
)
```

3. Fungsi berikut untuk menambahkan User Baru :
```
def adduser():
    id_user = random.randint(1,1000)
    nama = input("Masukkan nama user:")
    tgl_lahir = input("Masukkan tgl lahir(YYYY-MM-DD):")
    pekerjaan = input("Masukkan pekerjaan:")
    alamat = input("Masukkan alamat:")
    data = (id_user,nama,tgl_lahir,pekerjaan,alamat)
    sql = "insert into user values(%s,%s,%s,%s,%s);"
    c = mydb.cursor()
    c.execute(sql,data)
    mydb.commit()
    print("...........")
    print("Data berhasil dimasukkan")
    main()
```

4. Fungsi berikut untuk menambahkan Buku Baru :
```
def addbook():
    id_buku = input("Masukkan id buku:")
    nama_buku = input("Masukkan nama buku:")
    kategori = input("Masukkan kategori buku:")
    stock = input("Masukkan stock buku:")
    data = (id_buku,nama_buku,kategori,stock)
    sql = "insert into buku values(%s,%s,%s,%s);"
    c = mydb.cursor()
    c.execute(sql,data)
    mydb.commit()
    print(".........")
    print("Data berhasil dimasukkan")
    main()
```

5. Fungsi berikut melakukan Pencatatan Buku yang dipinjam :
```
def issuebook():
    id_user = input("Masukkan id user:")
    id_buku = input("Masukkan id buku:")
    nama_peminjam = input("Masukkan nama peminjam:")
    nama_buku = input("Masukkan nama buku:")
    tgl_peminjaman = input("Masukkan tgl peminjaman(YYYY-MM-DD):")
    sql = "insert into pinjam values(%s,%s,%s,%s,%s);"
    data = (id_user,id_buku,nama_peminjam,nama_buku,tgl_peminjaman)
    c = mydb.cursor()
    c.execute(sql,data)
    mydb.commit()
    print("............")
    print("Buku dipinjamkan ke:",nama_peminjam)
    stokbuku(id_buku,-1)
    main()
```

6. Fungsi berikut melakukan Pencatatan Pengembalian Buku :
```
def submitbook():
    id_user = input("Masukkan id user:")
    id_buku = input("Masukkan id buku:")
    nama_peminjam = input("Masukkan nama peminjam:")
    nama_buku = input("Masukkan nama buku:")
    tgl_pengembalian = input("Masukkan tgl pengembalian(YYYY-MM-DD):")
    sql = "insert into kembali values(%s,%s,%s,%s,%s);"
    data = (id_user,id_buku,nama_peminjam,nama_buku,tgl_pengembalian)
    c = mydb.cursor()
    c.execute(sql,data)
    mydb.commit()
    print(".............")
    print("Buku dikembalikan oleh:",nama_peminjam)
    stokbuku(id_buku,1)
    main()
```

6. Fungsi untuk mengecek Stok Buku :
```
def stokbuku(id_buku,u):
    sql = "select stock from buku where id_buku = %s;"
    data = (id_buku,)
    c = mydb.cursor()
    c.execute(sql,data)
    result = c.fetchone()
    t = result[0] + u
    sql = "update buku set stock = %s where id_buku = %s;"
    d = (t,id_buku)
    c.execute(sql,d)
    mydb.commit()
    main()
```

7. Fungsi untuk melihat daftar User Perpustakaan :
```
def displayuser():
    sql = "select * from user;"
    c = mydb.cursor()
    c.execute(sql)
    result = c.fetchall()
    result = pd.read_sql(sql,con=mydb)
    print(result)
    main()
```

8. Fungsi untuk melihat daftar Buku :
```
def displaybook():
    sql = "select * from buku;"
    c = mydb.cursor()
    c.execute(sql)
    result = c.fetchall()
    result = pd.read_sql(sql,con=mydb)
    print(result)
    main()
```

9. Fungsi untuk melihat User yang melakukan Peminjaman :
```
def displaypeminjaman():
    sql = "select * from pinjam;"
    c = mydb.cursor()
    c.execute(sql)
    result = c.fetchall()
    result = pd.read_sql(sql,con=mydb)
    print(result)
    main()
```

10. Fungsi untuk melakukan Pencarian Buku :
```
def caribuku():
    nama_buku = input("Masukkan nama buku yang dicari:")
    sql = "select * from buku where nama_buku = %s;"
    data = (nama_buku,)
    c = mydb.cursor()
    c.execute(sql,data)
    result = c.fetchall()
    result = pd.read_sql(sql,con=mydb)
    print(result)
    main()
```

11. Fungsi untuk melihat Daftar Transaksi yang terjadi (Peminjaman, Pengembalian):
```
def transaksi():
    sql = """select id_user, id_buku, nama_user, nama_buku, tgl_peminjaman, tgl_pengembalian from pinjam
    join kembali using (id_user, id_buku, nama_user, nama_buku)
    group by tgl_pengembalian;
    """
    c = mydb.cursor()
    c.execute(sql)
    result = c.fetchall()
    result = pd.read_sql(sql,con=mydb)
    print(result)
    main()
```

12. Fungsi berikut adalah main Interface (Tampilan Awal) dari Project ini :
```
def main():
    print("""...............LIBRARY MANAGEMENT SYSTEM.................
    1. Pendaftaran User Baru
    2. Pendaftaran Buku Baru
    3. Peminjaman
    4. Tampilkan Daftar User
    5. Tampilkan Daftar Buku
    6. Tampilkan Daftar Peminjaman
    7. Cari Buku
    8. Pengembalian
    9. Histori Transaksi
    0. Exit
    """)
    choice = input("Masukkan Nomor Tugas:")
    print("..........................")
    if(choice == '1'):
        adduser()
    elif(choice == '2'):
        addbook()
    elif(choice == '3'):
        issuebook()
    elif(choice == '4'):
        displayuser()
    elif(choice == '5'):
        displaybook()
    elif(choice == '6'):
        displaypeminjaman()
    elif(choice == '7'):
        caribuku()
    elif(choice == '8'):
        submitbook()
    elif(choice == '9'):
        transaksi()
    elif(choice == '0'):
        exit()
    else:
        print("Input Salah")
        main()

main()
```

## Cara Running Program :
1. Import dataset "lms.sql" ke mySQL Workbench dengan cara klik 'server' lalu 'data import'
2. Setelah dataset di import, masuk ke terminal anaconda dan arahkan ke directory hasil download. command : cd "<directory_anda>"
3. Running library.py dengan cara command : python library.py
4. Lakukan task dengan memilih nomor input
