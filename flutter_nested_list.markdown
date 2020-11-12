---
layout: default
title: Alternatif Nested List
parent: Flutter Cheatsheet
---

Bagaimana cara mengimplementasikan tampilan seperti ini di flutter?
![Alt Text](https://merry-drylands.cloudvent.net/assets/images/bad_listview.gif)


Pilihan pertama kita jatuh pada *nested listview*, seperti dibawah ini

~~~ dart
ListView(
    controller: scrollController,
    children: [
        Container(
            height: 150,
            child: Center(
            child: Text("Header"),
            ),
        ),
        ListView.builder(
            shrinkWrap: true,
            physics: NeverScrollableScrollPhysics(),
            itemBuilder: (ctx, index){
            
            })
    ]
)
~~~

Pada cara ini, kita menempatkan sebuah listview di dalam listview lainnya, sehingga kita mendapatkan efek scrolling seperti pada video. Perlu diperhatikan, listview di dalam widget dengan fungsi scroll memerlukan parameter *shrinkWrap* bernilai *true*, yang artinya fungsi *lazy load* dari listview tidak berfungsi. Apabila fungsi lazy load dimatikan, maka setiap item dalam listview akan mengalami rebuild setiap kali data dalam listview di update. Kita bisa memastikannya dengan membuka panel Flutter Performance. Setelah melakukan scrolling, Flutter Performance menunjukkan aktivitas berikut

![Alt Text](https://merry-drylands.cloudvent.net/assets/images/bad_list_item_70.png)

Pada panel Flutter Performance kita bisa melihat widget yang di build pada frame terakhir berjumlah 80, sementara yang terjadi pada saat kita melakukan scrolling adalah listview menampilkan 10 buah item baru. Kemungkinannya adalah, listview melakukan rebuild pada 70 item yang dimuat, baik item baru maupun item lama.

Melakukan rebuild pada seluruh listview bisa berdampak pada performa aplikasi jika jumlah muatan listview semakin banyak. Saat kita melakukan scrolling pada halaman 100 misalnya, maka aplikasi melakukan rebuild pada 1000 item dalam satu kali scroll. Hal ini bisa membahayakan performa aplikasi. Sebagai alternatif, kita bisa menggunakan *CustomScrollView* untuk menghasilkan efek scroll bertingkat tanpa menggunakan parameter shrinkWrap. Maka dengan mengganti ListView terluar dengan CustomScrollView, kita bisa mendapatkan tampilan yang sama dengan code berikut
 
~~~ dart
CustomScrollView(
    controller: scrollController,
    slivers:[
        SliverToBoxAdapter(
            child: Container(
            height: 150,
            child: Center(
                child: Text("Header"),
            ),
            ),
        ),
        SliverList(
            delegate: SliverChildBuilderDelegate(
                (context, index) {

                }
            ))
    ]
)
~~~

Karakteristik dari CustomScrollView adalah child yang ada harus berupa Sliver Widget, sehingga kita perlu menggantikan ListView dengan SliverList. Untuk melihat bagaimana performa dari code ini, kita melakukan pengecekan pada Flutter Performance dengan case yang sama, dan berikut hasil scrolling hingga ke item 70 pada aplikasi yang sudah kita perbarui

![Alt Text](https://merry-drylands.cloudvent.net/assets/images/good_list_item_70.png)

Dengan improvement yang telah kita lakukan, sekarang jumlah rebuild yang dilakukan aplikasi dalam sekali scroll berjumlah konstan pada angka 16.

