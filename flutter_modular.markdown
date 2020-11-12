---
layout: default
title: Steps to Build Flutter modular app
parent: Messing with Flutter
published: false

---

## Projek Modular
Struktur projek modular adalah organisasi code dengan mengelompokkan code berdasarkan fitur atau bagian yang memiliki tujuan berbeda ke dalam beberapa modul, yang biasa disebut *separation of concern*.

Struktur modular biasa digunakan oleh projek super app yang memiliki beberapa fitur dengan skala besar. Setiap fitur dalam sebuah aplikasi melalui proses development yang berbeda-beda, sehingga ideal nya setiap fitur dapat berdiri sendiri dan tidak mempengaruhi development fitur lain yang tidak berhubunchrgan. Dengan mengelompokkan code aplikasi ke dalam modul, proses development akan menjadi lebih mudah dan maintenance dapat dilakukan dengan mudah dan cepat.

Selain memberi kemudahan pada tahapan development, modularisasi juga mempermudah kerjasama tim. Ketika beberapa developer mengerjakan sebuah aplikasi dalam sebuah repository, konflik sangat mudah terjadi. Membagi repository ke dalam beberapa bagian atau modul dapat mengurangi resiko konflik.

## Kontrol akses dan kemudahan maintenance
Pembagian code base aplikasi ke dalam modul memudahkan kontrol akses ke repository, dimana akses ke sebuah repository module didapatkan oleh developer yang berpartisipasi pada development modul tersebut. Hal ini akan mempermudah proses maintenance pada aplikasi, jika akan dilakukan proses debugging misalnya, maka dapat dilakukan penelusuran jejak ke developer yang memiliki akses ke modul terkait.

## Implementasi aplikasi modular menggunakan flutter_modular
Kita akan mengimplementasikan struktur aplikasi modular pada sebuah aplikasi flutter. Yang perlu diperhatikan pada aplikasi modular adalah sistem routing yang dapat memfasilitasi navigasi antar modul, maupun navigasi di dalam satu modul yang sama. Untuk manajemen routing kita akan menggunakan library [flutter_modular](https://pub.dev/packages/flutter_modular) yang akan mempermudah kita membuat routing yang sesuai.

### Setting projek
Berikut ini adalah struktur dari projek yang akan kita buat, yang kita beri nama Projek.
<br>Projek
- mainmodule
- editormodule
- util

Pertama siapkan projek dengan membuat folder Projek. Dalam folder ini kita akan membuat 3 buah modul yang akan kita simpan dalam 3 repository berbeda. Kita akan membuat setiap modul sebagai sebuah aplikasi flutter supaya modul dapat diakses oleh package manager flutter.

### Util dan resource
Sebuah modul akan didedikasikan untuk menyimpan seluruh resource asset dan file lain yang memuat fungsi-fungsi umum yang bisa digunakan oleh modul lain. Dengan menyimpan semua resource dalam satu repository kita menghindari adanya duplikat item yang sama dan menjaga konsistensi konfigurasi yang dipakai bersama oleh beberapa modul. Pada projek ini resource disimpan dalam modul util yang memuat file helper untuk penggunaan warna dan beberapa asset berupa gambar dan font. Menggunakan opsi GUI New Flutter Project, buat modul util dan atur lokasi penyimpanan di root folder Projek.

Setelah selesai melakukan perubahan pada modul util, masuk ke directory Projek/util yang telah dibuat, kemudian lakukan inisiasi git file disini dengan git init ke repository yang telah kita buat di github dengan nama utilmodule. Nantinya modul lain dapat mengakses modul util dengan menambahkan dependency ke url [https://github.com/yogurtpops/utilmodule](https://github.com/yogurtpops/utilmodule)

### MainModule
Sebuah projek modular memiliki sebuah modul utama atau mainmodule yang akan dijalankan pertama kali ketika aplikasi dibuka. Menggunakan Android Studio, buat sebuah projek flutter menggunakan opsi GUI New Flutter Project, lalu isi nama projek dengan mainmodule, dan atur lokasi penyimpanan di root folder Projek. Pada file pubspec.yaml tambahkan import library [flutter_modular](https://pub.dev/packages/flutter_modular), dan tambahkan import modul util dengan deklarasi berikut

~~~ yaml
util:
  git:
    url: git://github.com/yogurtpops/utilmodule.git
~~~

Pada projek ini, dalam mainmodule kita membuat sebuah halaman dashboard dengan sebuah bottom navigation bar yang menampilkan dua buah halaman. Code untuk tampilan dashboard ini dapat di lihat [di sini](https://github.com/yogurtpops/mainmodule).

Untuk menjadikan modul ini sebagai modul utama, buat sebuah class MainModule dari library [flutter_modular](https://pub.dev/packages/flutter_modular) yang memuat aturan routing sebagai berikut:

~~~ dart
class AppModule extends MainModule {
  @override
  List<Bind> get binds => [];

  @override
  List<Router> get routers => [
    Router('/', child: (_, __) => LandingPage()),
    Router('/imageeditor', module: ImageEditorModule()),
  ];

  @override
  Widget get bootstrap => AppWidget();
}
~~~

get routers memuat dynamic routing yang dapat diakses oleh mainmodule , yang dapat digunakan untuk melakukan navigasi ke halaman dalam mainmodule seperti halaman LandingPage, atau untuk mengakses modul lain seperti modul ImageEditorModule yang akan kita buat dalam projek [ini](https://github.com/yogurtpops/mainmodule). AppWidget adalah root widget yang akan dipanggil oleh main function dari projek untuk menjalankan aplikasi. Berikut ini adalah main function dari projek ini

~~~ dart
void main() => runApp(ModularApp(module: AppModule()));

class AppWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: "/",
      navigatorKey: Modular.navigatorKey,
      onGenerateRoute: Modular.generateRoute,
    );
  }
}
~~~

### ChildModule
Dalam sebuah projek, kita bisa memiliki sebuah modul utama dan beberapa modul lain yang disebut childmodule. Pada projek ini kita menerapkan sebuah childmodule yang memuat sebuah fitur pengeditan gambar, yang dapat dilihat [disini](https://github.com/yogurtpops/imageeditor).
Untuk membuat sebuah childmodule menggunakan Android Studio GUI buat sebuah Flutter Module dengan nama editormodule di root folder Projek. Dengan struktur demikian, mainmodule dan editormodule dapat berjalan tanpa tergantung satu sama lain.
Buka pubspec.yaml dan tambahkan import dependencies package util dan [flutter_modular](https://pub.dev/packages/flutter_modular). Kemudian buat deklarasi modul editormodule sebagai berikut:

~~~ dart
class ImageEditorModule extends ChildModule {
  @override
  List<Bind> get binds => [

  ];

  @override
  List<Router> get routers => [
    Router('/', child: (_, args) => ImageEditor()),
  ];

  static Inject get to => Inject<ImageEditorModule>.of();
}
~~~

Untuk menjalankan editormodule tanpa mainmodule, buat sebuah MainModule yang nanti nya hanya akan digunakan jika editormodule dijalankan tanpa mainmodule, sehingga fungsi main dari editormodule terlihat seperti ini

~~~ dart
class AppModule extends MainModule {
  @override
  List<Bind> get binds => [];

  @override
  List<Router> get routers => [
    Router('/', child: (_, args) => ImageEditor()),
  ];

  @override
  Widget get bootstrap => AppWidget();
}

void main() => runApp(ModularApp(module: AppModule()));

class AppWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: "/",
      navigatorKey: Modular.navigatorKey,
      onGenerateRoute: Modular.generateRoute,
    );
  }
}
~~~

Setup editormodule selesai dan modul bisa dijalankan tanpa terikat dengan mainmodule.

### Integrasi module
Setelah development mainmodule dan editormodule selesai, aplikasi yang terdiri dari kedua modul ini dapat dicoba dengan mengintegrasikan kedua modul.
Buka kembali projek mainmodule, lalu buka file pubspec.yaml, dan tambahkan dependensi editormodule sebagai berikut

~~~ yaml
editormodule:
  git:
    url: git://github.com/yogurtpops/imageeditor.git
~~~

Refresh projek, kemudian jalankan kembali aplikasi.
Pada screen LandingPage, button di halaman 'Edit it' mentrigger fungsi berikut ini.
<em>Modular.to.pushNamed('/imageeditor');</em>
Routing aplikasi dilakukan dengan dynamic routing yang dihandle oleh [flutter_modular](https://pub.dev/packages/flutter_modular) dengan handle Modular, dan mengarahkan navigasi ke halaman yang telah dideklarasikan dalam class MainModule dan ChildModule. Pada fungsi di atas, target routing adalah modul editormodule yang mengarahkan user ke halaman utama editormodule.