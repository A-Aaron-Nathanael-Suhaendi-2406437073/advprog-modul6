Commit 1 Reflection notes

Pada tahap awal ini, fungsi handle_connection ditambahkan untuk menangani koneksi TCP yang masuk dan mengekstrak informasi HTTP Request dari browser. Berikut adalah penjelasan mengenai cara kerja kode di dalam fungsi tersebut:

TcpStream dan BufReader: Parameter stream merepresentasikan koneksi yang terbuka antara client (browser) dan server. Untuk membaca data dari koneksi ini dengan lebih efisien, koneksi tersebut dibungkus menggunakan BufReader. BufReader mengelola pembacaan data menggunakan metode buffering, dibandingkan harus membaca data byte demi byte secara langsung dari OS.

.lines(): Method ini disediakan oleh trait BufRead untuk membaca stream data dan mengembalikannya sebagai iterator yang memecah data baris demi baris setiap kali menemukan indikator newline.

.map(|result| result.unwrap()): Setiap pemanggilan dari .lines() mengembalikan tipe Result karena pembacaan data dari jaringan memiliki potensi error. Pemanggilan .unwrap() di dalam fungsi closure ini bertugas untuk mengekstrak isi String jika pembacaan berhasil, dan akan menghentikan program jika terjadi error.

.take_while(|line| !line.is_empty()): Berdasarkan standar protokol HTTP, request header dari browser akan diakhiri oleh dua baris baru (CRLF berurutan) yang terbaca sebagai satu baris kosong. Method ini akan memastikan server hanya membaca stream secara terus-menerus selama baris tersebut tidak kosong (hingga akhir dari HTTP header).

.collect(): Berfungsi sebagai consumer yang mengambil semua hasil dari iterator sebelumnya dan mengumpulkannya menjadi sebuah koleksi data baru, yaitu Vec<_> (vektor string), sehingga data request dapat dicetak dan diamati.

Kesimpulannya, kode ini bertugas menangkap rentetan teks yang dikirimkan oleh browser, membacanya secara efisien hingga batas header HTTP selesai, dan menyimpannya ke dalam bentuk vektor (Array/List) agar mudah dikelola oleh program kita selanjutnya.


