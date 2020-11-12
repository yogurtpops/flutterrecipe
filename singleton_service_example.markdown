---
layout: default
title: Singleton Object
parent: Flutter Cheatsheet
---

Seringkali kita menggunakan sebuah service dalam aplikasi yang kita buat, dimana service tersebut tidak boleh memiliki duplikat. Untuk memastikan bahwa service yang kita buat hanya dapat memiliki satu buah *copy* dalam satu waktu, kita bisa membuat service sebagai singleton. Berikut bagaimana kita membuat sebuah singleton.

~~~ dart
class SingleCopyService {

  static final CreatSingleCopyServiceeProduct _singleton = SingleCopyService._internal();

  factory SingleCopyService() {
    return _singleton;
  }

  SingleCopyService._internal(){
    inProgress = false;
  }
}
~~~

Dengan cara demikian, service akan di create dan diinisiasi jika belum dibuat. Kemudian jika Service dipanggil kembali, maka objek factory akan mereturn service yang telah dibuat.