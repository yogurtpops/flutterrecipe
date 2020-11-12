---
layout: page
title: Play With Frida
published: false
---

Frida adalah package python yang berfungsi untuk melakukan interception ke proses dalam aplikasi. Hal-hal yang dapat dicapai dengan library ini biasanya dilakukan untuk keperluan testing aplikasi. Tulisan ini akan mengulik teknik sederhana yang dapat dilakukan menggunakan Frida untuk melakukan injeksi ke dalam aplikasi android, dan kita akan melihat hal yang dapat dicapai juga sejauh mana dengan Frida kita bisa mengakses aplikasi dari pintu belakang.

1.  Decompile apk  
Install aplikasi dari playstore, kemudian menggunakan adb copy apk dari android ke pc. Decompile apk dengan apk tool. 
    Untuk mendapatkan nama package : 
        adb shell 
        pm list packages
    Mendapatkan lokasi path apk dengan nama_package :
        adb shell pm path <nama_package>
    Untuk mengcopy apk dengan path_apk :
        adb pull <path_apk>


2.  Mengupload frida-server ke device & update permission:
        adb push frida-server /data/local/tmp/frida-server
        su -c "chmod 777 /data/local/tmp/frida-server"

    Menjalankan frida-server di device:
        adb shell
        ./data/local/tmp/frida-server -D

    Mendapatkan nama aplikasi:
        frida-ps -U

    Running aplikasi dengan injeksi script frida:
        frida -U -l /path/file-javascript-nya.js -f Nama.package.aplikasinya --no-pause
        frida -U -l /home/tlab-n024/Toped_Flashsale/inject_frida.js -f com.tokopedia.tkpd --no-pause
    

3.  Melakukan dynamic analisis untuk menemukan fungsi x
Kita bisa membuat otomasi log pada suatu kelompok fungsi
Lakukan analisa activity dengan membaca file manifest untuk mengetahui root activity. Buka file code untuk activity ini,
    lalu buat script frida yaitu sebuah fungsi untuk mengintersep setiap fungsi yang terprediksi sebagai trigger untuk menuju halaman detail, buat log yang akan dipanggil ketika fungsi berjalan. Upload script frida, jalankan aplikasi, dan ikuti debugging proses yang ditampilkan di console dan apabila terdapat fungsi yang melakukan log maka dapat dianggap fungsi routing ke halaman detail.
        Search file:
            grep -r "keyword"
        
Selesai.
>>> Menggunakan frida untuk melakukan launch aplikasi dengan timer yang dibuat dengan script .......