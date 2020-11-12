---
layout: default
title: State Management
parent: Flutter Cheatsheet
---

Aplikasi state management dapat dilakukan dengan menggunakan *design pattern* seperti Provider, Bloc, dan ScopedModel. Sementara pada aplikasi yang sederhana kita bisa membuat state management tanpa *design pattern*. Implementasi demikian dapat dilakukan bila aplikasi memiliki tampilan sederhana dan pengembangan aplikasi dapat kita prediksi. Contoh dari tampilan yang sederhana adalah dimana kita bisa mendefinisikan widget ke dua kelompok, *parent* dan *child*, seperti tampilan ini. Pada tampilan ini, widget parent adalah container yang memuat seluruh halaman yang nampak. Widget child berupa komponen dalam tampilan, yaitu image selector dan image display.
![Alt Text](https://merry-drylands.cloudvent.net/assets/images/state_management.jpg)

Perubahan state dapat kita lakukan dengan membuat fungsi di masing-masing widget. Bagaimana jika perubahan state sebuah widget mempengaruhi widget lainnya seperti ilustrasi ini?
![Alt Text](https://merry-drylands.cloudvent.net/assets/images/state_management.gif)

Pada tampilan ini kita melakukan perubahan state pada beberapa widget. Widget child 1 menerima input saat user melakukan tap pada sebuah gambar. Kemudian widget child mengirim perubahan state ke parent widget. Kemudian parent widget mengirim perubahan state ke widget child 2. Maka kita bisa melakukan perubahan dengan cara berikut.

1. Mengubah child dari parent

Untuk child widget, kita membuat sebuah fungsi callback yang mana sebuah fungsi di parent widget yang akan dipanggil dengan trigger dari child.

~~~ dart
class ChildWidget extends StatefulWidget {

  Function(Color, String, bool) callback;
  ToolsContainer(this.callback);

  @override
  State<StatefulWidget> createState() {
    return _ToolsContainer();
  }
}

...

onTap: () {
    widget.callback(null, _sampleImages[position], false);

},
~~~

Kemudian pada parent widget, kita mendeklarasikan child widget seperti ini
~~~ dart
    child: new ToolsContainer(callback_set_background),
~~~

2. Mengubah parent dari child
Fungsi callback_set_background dalam widget parent akan ditrigger untuk melakukan perubahan pada child 2. Berikut ini bagaimana kita mentrigger perubahan child 2 dari parent.

Widget parent kita bungkus dengan sebuah widget perantara yang kita namakan ParentProvider.
~~~ dart
class ParentProvider extends InheritedWidget {

  final String bgUrl;

  ParentProvider(this.bgUrl);

  @override
  bool updateShouldNotify(ParentProvider oldWidget) {
    return true;
  }

  static ParentProvider of(BuildContext context) =>
      context.inheritFromWidgetOfExactType(ParentProvider);
}
~~~

Di parent provider kita menyimpan setiap value milik parent widget yang akan diekspos ke widget child. Sehingga, ketika salah satu value tersebut mengalami perubahan widget child akan otomatis terupdate. Kita menempatkan widget parent dalam parent provider, sehingga fungsi build akan nampak seperti ini.
~~~ dart
  @override
  Widget build(BuildContext context) {
    return new ParentProvider(
        recentStates[activeState]._chosen_image_url,
        new Scaffold());
  }
~~~

Kemudian kita bisa menggunakan value milik parent dan menerima update di widget child dengan cara demikian
~~~ dart
    @override
  Widget build(BuildContext context) {
    final selectedBgUrl = ParentProvider.of(context).bgUrl;
  }
~~~