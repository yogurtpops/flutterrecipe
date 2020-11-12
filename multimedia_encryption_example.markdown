---
layout: default
title: Enkripsi Konten Multimedia
parent: Flutter Cheatsheet
---

Pada umumnya tidak disarankan untuk menyimpan data berharga dalam aplikasi mobile. Sementara beberapa jenis data *non-confidential* contohnya konten berupa *intellectual property*, biasanya memiliki ukuran yang besar sehingga perlu disimpan secara permanen dalam memori device, sehingga memerlukan perlindungan enkripsi.

Enkripsi biasanya dilakukan oleh pengirim data, misalnya penyimpanan data di cloud database menyimpan data dalam bentuk data enkripsi. Ketika akan digunakan data yang sudah dienkripsi perlu di dekripsi terlebih dahulu. Data hasil dekripsi perlu disimpan dalam memori sementara, yang akan dihapus ketika penggunaan data berakhir. Berikut adalah contoh *use-case* penggunaan data konten multimedia yang diunduh dari cloud server dalam bentuk enkripsi.

![Alt Text](https://merry-drylands.cloudvent.net/assets/images/flow_use_case_encrypted_content.png)

Dengan *use-case* tersebut, user hanya bisa mengakses konten dari aplikasi yang kita buat.


[View on Github](https://github.com/yogurtpops/encryptiontest){: .btn .btn-blue .mr-2}






