# Studi Kasus Aplikasi Perpustakaan

Mulai dengan membuat projek baru berisi `Main.java` sebagai main class.

```java
package perpus;

public class Main {

    public static void main(String[] args) {
        // TODO code application logic here
    }

}
```

Pada aplikasi ini kita akan membuat interface untuk class pengguna. Silahkan buat file `UserInterface.java` pada package `perpus`. File ini akan berisi standart dari class `Petugas` dan `Siswa` nantinya.

```java
package perpus;

public interface UserInterface {
    public void setNama(String nama);
    public void setAlamat(String alamat);
    public void setTelepon(String telepon);

    public String getNama(int id);
    public String getAlamat(int id);
    public String getTelepon(int id);
}
```

Selanjutnya kita dapat membuat class `Petugas.java` pada package `perpus`. Kita perlu menambahkan attribut yang diperlukan dan melakukan override pada setiap method dan attribute yang ada di `UserInterface`. Attribut yang kita akan kita gunakan akan berita `ArrayList` yang langsung diinisiasi di constructor class. Kita juga bisa langsung menambahkan data petugas pada method yang terpisah dan akan langsung dipanggil di constructor. Sebagai tambahan kita juga dapat menambahkan method untuk print daftar petugas yang terdaftar.

```java
package perpus;

import java.util.ArrayList;

public class Petugas implements UserInterface {

    // penggunaan final untuk inisiasi pada constructor
    // sehingga tidak bisa di overide pada child class nya
    private final ArrayList<String> nama;
    private final ArrayList<String> alamat;
    private final ArrayList<String> telepon;

    public Petugas() {
        // inisiasi attribute
        this.nama = new ArrayList();
        this.alamat = new ArrayList();
        this.telepon = new ArrayList();

        // langsung mengisi data petugas saat membuat
        // instance classnya
        this.initListPetugas();
    }

    private void initListPetugas() {
        nama.add("Admin");
        alamat.add("Motherboard");
        telepon.add("127001");
    }

    public void printListPetugas() {
        System.out.println("Daftar petugas terdaftar:");
        for (int i = 0; i < this.nama.size(); i++) {
            String msg = i + ". ";
            msg += this.nama.get(i) + " | ";
            msg += this.alamat.get(i) + " | ";
            msg += this.telepon.get(i) + " | ";
            System.out.println(msg);
        }
        System.out.println("");
    }

    @Override
    public void setNama(String nama) {
        this.nama.add(nama);
    }

    @Override
    public void setAlamat(String alamat) {
        this.alamat.add(alamat);
    }

    @Override
    public void setTelepon(String telepon) {
        this.telepon.add(telepon);
    }

    @Override
    public String getNama(int id) {
        return this.nama.get(id);
    }

    @Override
    public String getAlamat(int id) {
        return this.alamat.get(id);
    }

    @Override
    public String getTelepon(int id) {
        return this.telepon.get(id);
    }

}
```

Setelah ini kita bisa lanjut untuk membuat class `Siswa` yang juga meng-implement `UserInterface` seperti class `Petugas` namun memiliki tambahan attribute yang digunakan untuk proses peminjaman buku dan method untuk update status peminjaman siswa.

```java
package perpus;

import java.util.ArrayList;

public class Siswa implements UserInterface {

    // penggunaan final untuk inisiasi pada constructor
    // sehingga tidak bisa di overide pada child class nya
    private final ArrayList<String> nama;
    private final ArrayList<String> alamat;
    private final ArrayList<String> telepon;
    private final ArrayList<Boolean> status;

    public Siswa() {
        // inisiasi attribute
        status = new ArrayList<>();
        telepon = new ArrayList<>();
        alamat = new ArrayList<>();
        nama = new ArrayList<>();

        // langsung mengisi data siswa saat membuat
        // instance classnya
        this.initListSiswa();
    }

    private void initListSiswa() {
        nama.add("Musfirati");
        alamat.add("Malang");
        telepon.add("081234567890");
        status.add(true);
    }

    public void printListSiswa() {
        System.out.println("Daftar siswa terdaftar:");
        for (int i = 0; i < this.nama.size(); i++) {
            String msg = i + ". ";
            msg += this.nama.get(i) + " | ";
            msg += this.alamat.get(i) + " | ";
            msg += this.telepon.get(i) + " | ";
            msg += this.status.get(i) ? "Meminjam" : "Kosong";
            System.out.println(msg);
        }
        System.out.println("");
    }

    public void updateStatus(int idSiswa, boolean status) {
        this.status.set(idSiswa, status);
    }

    @Override
    public void setNama(String nama) {
        this.nama.add(nama);
    }

    @Override
    public void setAlamat(String alamat) {
        this.alamat.add(alamat);
    }

    @Override
    public void setTelepon(String telepon) {
        this.telepon.add(telepon);
    }

    public void setStatus(Boolean status) {
        this.status.add(status);
    }

    @Override
    public String getNama(int id) {
        return nama.get(id);
    }

    @Override
    public String getAlamat(int id) {
        return alamat.get(id);
    }

    @Override
    public String getTelepon(int id) {
        return telepon.get(id);
    }

    public Boolean getStatus(int id) {
        return status.get(id);
    }

}
```

Selanjutnya kita bisa coba membuat instance dari 2 class yang sudah kita buat di class `Main` dan mencoba print daftar petugas dan siswa.

```java
package perpus;

public class Main {
    static Petugas petugas;
    static Siswa siswa;

    public static void main(String[] args) {
        System.out.println("SELEMAT DATANG DI PERPUSTAKAAN");
        System.out.println("");

        petugas = new Petugas();
        siswa = new Siswa();

        petugas.printListPetugas();
        siswa.printListSiswa();
    }

}
```

Ketika kita jalankan maka output yang disajikan adalah sebagai berikut.

```bash
run:
SELEMAT DATANG DI PERPUSTAKAAN

Daftar petugas terdaftar:
0. Admin | Motherboard | 127001 |

Daftar siswa terdaftar:
0. Musfirati | Malang | 081234567890 | Meminjam

BUILD SUCCESSFUL (total time: 1 second)
```

Selanjutnya adalah class `Buku` dan `Peminjaman` untuk transaksi peminjaman buku oleh siswa.

Kita buat class `Buku` pada package `perpus` yang berisi:

```java
package perpus;

import java.util.ArrayList;

public class Buku {
    private final ArrayList<String> nama;
    private final ArrayList<Integer> stok;
    private final ArrayList<Integer> harga;

    public Buku() {
        this.nama = new ArrayList();
        this.stok = new ArrayList();
        this.harga = new ArrayList();

        this.initListBuku();
    }

    private void initListBuku() {
        this.nama.add("The One");
        this.stok.add(10);
        this.harga.add(10);

        this.nama.add("The Second");
        this.stok.add(7);
        this.harga.add(12);

        this.nama.add("The Third");
        this.stok.add(5);
        this.harga.add(15);
    }

    public void printListBuku() {
        System.out.println("Berikut ketersediaan buku saat ini.");
        for (int i = 0; i < this.nama.size(); i++) {
            String msg = i + ". ";
            msg += this.stok.get(i) + " buku | ";
            msg += this.nama.get(i) + " | ";
            msg += "Rp. " + this.harga.get(i) + "K";
            System.out.println(msg);
        }
        System.out.println("");
    }

    public void pinjam(int id, int banyak) {
        int sisaStok = this.stok.get(id);
        sisaStok -= banyak;

        this.stok.set(id, sisaStok);
    }

    public void kembali(int id, int banyak) {
        int sisaStok = this.stok.get(id);
        sisaStok += banyak;

        this.stok.set(id, sisaStok);
    }

    public Integer getStok(int id) {
        return this.stok.get(id);
    }
}
```

Lalu class `Peminjaman` di package `perpus` yang berisi:

```java
package perpus;

import java.util.ArrayList;

public class Peminjaman {
    private final ArrayList<Integer> idSiswa;
    private final ArrayList<Integer> idBuku;
    private final ArrayList<Integer> banyak;

    public Peminjaman() {
        idSiswa = new ArrayList();
        idBuku = new ArrayList();
        banyak = new ArrayList();
    }

    public void printPeminjamanSiswa(int idSiswa) {
        System.out.println("Berikut adalah daftar pinjaman siswa id " + idSiswa);

        // proses filter data peminjaman berdasarkan idSiswa
        // sehingga hanya menampilkan data peminjaman dari 1 siswa
        this.idSiswa.stream()
            .filter(id -> (id == idSiswa))
            .map(id -> this.idSiswa.indexOf(id))
            .forEach(index -> {
                String msg = index + ". ";
                msg += "Buku id-" + this.idBuku.get(index) + " | ";
                msg += this.banyak.get(index) + " buku";
                System.out.println(msg);
            });
        System.out.println("");
    }

    public void tambahPeminjaman(int idSiswa, int idBuku, int banyak) {
        this.idSiswa.add(idSiswa);
        this.idBuku.add(idBuku);
        this.banyak.add(banyak);
    }

    public Integer getIdBuku(int idSiswa) {
        int index = this.idSiswa.indexOf(idSiswa);
        return this.idBuku.get(index);
    }

    public Integer getBanyak(int idSiswa) {
        int index = this.idSiswa.indexOf(idSiswa);
        return this.banyak.get(index);
    }

    public void hapusPeminjaman(int idSiswa) {
        int index = this.idSiswa.indexOf(idSiswa);
        this.banyak.remove(index);
        this.idBuku.remove(index);
        this.idSiswa.remove(index);
    }
}
```

Dari sini, kita sudah menyediakan method-method yang akan digunakan untuk proses peminjamann buku oleh siswa. Selanjutnya kita bisa menyusun interaksi user terhadap sistem langsung di class `Main`.

Dimulai dari menambahkan method untuk memilih siswa yang ingin meminjam buku.

Kemudian membuat sajian menu untuk meminjam, mengembalikan, cek data, dan keluar. Diikuti dengan method untuk proses peminjaman dan pengembalian.

```java
package perpus;

import java.util.Scanner;

public class Main {
    static Scanner input;

    static Petugas petugas;
    static Siswa siswa;
    static Buku buku;
    static Peminjaman peminjaman;

    static Integer idSiswa;

    public static void main(String[] args) {
        input = new Scanner(System.in);

        System.out.println("SELEMAT DATANG DI PERPUSTAKAAN");
        System.out.println("");

        petugas = new Petugas();
        siswa = new Siswa();
        buku = new Buku();
        peminjaman = new Peminjaman();

        petugas.printListPetugas();
        siswa.printListSiswa();

        idSiswa = pilihSiswa();

        Boolean ulang = true;
        while (ulang) {
            Integer menu = pilihMenu();
            switch (menu) {
                case 1 -> buku.printListBuku();
                case 2 -> prosesPeminjaman();
                case 3 -> prosesPengembalian();
                case 4 -> peminjaman.printPeminjamanSiswa(idSiswa);
                case 99 -> ulang = false;
                default -> {
                    System.out.println("Menu yang dipilih tidak tersedia");
                    System.out.println("");
                }
            }
        }

        input.close();
    }

    public static Integer pilihSiswa() {
        System.out.print("Silahkan masukkan id siswa : ");
        return input.nextInt();
    }

    public static Integer pilihMenu() {
        System.out.println("Selamat datang di Perpustakaan");
        System.out.println("1. List Buku");
        System.out.println("2. Peminjaman");
        System.out.println("3. Pengembalian");
        System.out.println("4. Data Siswa");
        System.out.println("99. Keluar");
        System.out.print("=> ");
        return input.nextInt();
    }

    public static void prosesPeminjaman() {
        if (!siswa.getStatus(idSiswa)) {
            System.out.println("Siswa sedang meminjam buku, tidak dapat meminjam lagi");
            System.out.println("");
            return;
        }

        System.out.print("Masukkan id buku: "); int idBuku = input.nextInt();
        System.out.print("Banyak buku: "); int banyak = input.nextInt();

        if (buku.getStok(idBuku) < banyak) {
            System.out.println("Sisa stok buku tidak mencukupi");
            System.out.println("");
            return;
        }

        peminjaman.tambahPeminjaman(idSiswa, idBuku, banyak);
        buku.pinjam(idBuku, banyak);
        siswa.updateStatus(idSiswa, Boolean.FALSE);
        System.out.println("Berhasil melakukan peminjaman buku");
        System.out.println("");
    }

    public static void prosesPengembalian() {
        int idBuku = peminjaman.getIdBuku(idSiswa);
        int banyak = peminjaman.getBanyak(idSiswa);

        peminjaman.hapusPeminjaman(idSiswa);
        buku.kembali(idBuku, banyak);
        siswa.updateStatus(idSiswa, Boolean.TRUE);

        System.out.println("Berhasil melakukan pengembalian buku");
        System.out.println("");
    }
}
```
