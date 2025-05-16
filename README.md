# Modul 9 Pemrograman Lanjut : Software Architectures
oleh **Brenda Po Lok Fahida**

<br>
<br>

> What is AMQP?

**AMQP (Advanced Message Queuing Protocol)** adalah protokol komunikasi terbuka yang digunakan untuk **message broker** seperti RabbitMQ. AMQP memungkinkan aplikasi saling berkomunikasi melalui sistem antrian pesan (message queue), tanpa harus langsung terhubung satu sama lain.

Dengan AMQP, satu aplikasi bisa mengirim pesan ke antrian, dan aplikasi lain bisa mendengarkan antrian tersebut untuk menerima pesan. Ini memungkinkan sistem yang terdesentralisasi dan asinkron.

> What does it mean? guest:guest@localhost:5672 , what is the first guest, and what
is the second guest, and what is localhost:5672 is for?

Terdapat bagian `amqp://guest:guest@localhost:5672` pada `main.rs` yang digunakan sebagai alamat koneksi ke Rabbitmq. String tersebut memiliki format `amqp://username:password@host:port`. Kata guest pertama merupakan username yang digunakan untuk mengakses Rabbitmq, sedangkan guest kedua adalah password untuk autentikasi. Localhost menunjukkan bahwa Rabbitmq dijalankan secara lokal di komputer sendiri, dan 5672 adalah port standar yang digunakan oleh Rabbitmq untuk menerima koneksi AMQP. 

Jadi, secara keseluruhan, dugaan awal saya, bagian kode pada aplikasi subscriber ini mencoba terhubung ke Rabbitmq yang berjalan di komputer lokal melalui port 5672, menggunakan akun default dengan username dan password guest. Koneksi ini diperlukan agar aplikasi dapat mendengarkan pesan dari queue bernama user_created dan memprosesnya menggunakan handler yang telah didefinisikan.

## Message Broker dengan RabbitMQ
**Simulation slow subscriber**

<img src="image/image_1.png">

Pada gambar di atas, saya telah mengaktifkan simulasi slow subscriber dengan meng-uncomment `thread::sleep(ten_millis);` di dalam main.rs. Kemudian, saya menjalankan Publisher beberapa kali dengan cepat menggunakan perintah cargo run. Akibatnya, grafik pertama (Queued messages) pada RabbitMQ menunjukkan adanya antrian pesan yang masuk, namun belum sempat dikonsumsi oleh Subscriber secara langsung.

Hal ini terjadi karena Publisher mengirimkan pesan lebih cepat daripada kemampuan Subscriber untuk memprosesnya. Oleh karena itu, total jumlah pesan dalam antrian meningkat menjadi 36 (dapat dilihat pada bagian "Unacked"). Angka ini menunjukkan ada 36 pesan yang sedang menunggu untuk dikonsumsi oleh Subscriber secara bertahap.

<br>

**One publisher and three subscribers**

<img src="image/image_2.png">

<img src="image/image_3.png">

Pada skenario *One Publisher and Three Subscribers*, saya menjalankan satu Publisher dan tiga Subscriber secara paralel. Tanpa melakukan perubahan pada kode program, hasil yang diperoleh sangat berbeda dibandingkan saat hanya ada satu Subscriber. Saat Publisher mengirimkan pesan, RabbitMQ secara otomatis mendistribusikan pesan-pesan tersebut ke beberapa Subscriber yang aktif. Berbeda dari Simulation slow subscriber, pada skenario ini tidak terjadi penumpukan pesan dalam antrian (*queued messages*) pada RabbitMQ. Hal ini dapat diamati dari grafik pertama pada dashboard RabbitMQ yang menunjukkan bahwa nilai Ready, Unacked, dan Total kembali ke nol dengan cepat.

Grafik pada bagian *Queued messages* menunjukkan bahwa semua pesan segera didistribusikan dan dikonsumsi oleh Subscriber secara efisien. Ini menunjukkan bahwa penambahan jumlah Subscriber dapat meningkatkan kecepatan pemrosesan pesan dan mengurangi kemungkinan penumpukan antrean. Mekanisme ini mencerminkan prinsip sistem *event-driven*, di mana Publisher hanya bertugas mengirimkan pesan ke message broker (RabbitMQ), dan broker akan mengelola distribusi pesan ke Subscriber yang tersedia. Dengan demikian, perilaku sistem dapat berubah hanya dengan menambah jumlah Subscriber atau menyesuaikan konfigurasi broker, tanpa perlu mengubah kode program itu sendiri.


## Bonus
**Simulation slow subscriber**

<img src="image/image_4.png">

**One publisher and three subscribers**

<img src="image/image_5.png">

<img src="image/image_6.png">