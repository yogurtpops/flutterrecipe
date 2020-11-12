---
layout: default
title: State Management - Eventbus
parent: Flutter Cheatsheet
---

Kita bisa menggunakan *event bus* untuk melakukan update state dari satu class ke class lain. Sebaiknya dalam sebuah aplikasi hanya digunakan sebuah eventbus untuk menghindari kesulitan pada proses debugging. Berikut ini adalah contoh penggunaan eventbus untuk mentrigger update state.

Deklarasikan sebuah eventbus sebagai static object dan eventbus event dengan sebuah muatan string.
~~~ dart
class EventBusEvent{
  final String message;
  EventBusEvent(this.message);
}

EventBus eventBus = EventBus();
~~~

Kirim sebuah pesan menggunakan eventbus menggunakan perintah berikut. Pesan ini akan diterima oleh setiap komponen dalam aplikasi yang melakukan subskripsi ke event yang sesuai.
~~~ dart
    eventBus.fire(EventBusEvent("Halo"));
~~~

Inisiasi eventbus subscription di class yang akan merespon update data.
~~~ dart
class StateClassA extends State<ClassA> {
    StreamSubscription subscription;
    String messageState = "";

      @override
  void initState() {
    super.initState();
    subscription = eventBus.on<EventBusEvent>().listen((event) { 
      setState(() {
           messageState = (event as EventBusEvent).message;
      });    
    }
    });
}
~~~

