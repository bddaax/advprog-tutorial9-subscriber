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