### Refleksi Adpro Modul 11 

#### Perbandingan Log Aplikasi Sebelum dan Sesudah Di-Expose sebagai Service
Sebelum aplikasi diekspos sebagai service, log aplikasi yang ditampilkan menggunakan perintah:
kubectl logs deployment/hello-node
hanya menunjukkan aktivitas awal dari container, dan belum terlihat adanya interaksi pengguna. Namun, setelah deployment diekspos sebagai service dengan perintah:
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
minikube service hello-node
dan aplikasi dibuka beberapa kali melalui browser, log mulai menampilkan aktivitas tambahan setiap kali aplikasi diakses. Hal ini menunjukkan bahwa setiap permintaan dari browser berhasil diteruskan ke pod, dan interaksi tersebut terekam dalam log.
![alt text](image.png)
Kesimpulan: Jumlah log bertambah setiap kali aplikasi dibuka setelah diekspos, yang menandakan bahwa service berhasil meneruskan trafik ke aplikasi.

---

#### Perbedaan kubectl get Tanpa dan Dengan Opsi -n
Terdapat dua variasi perintah kubectl get yang digunakan dalam tutorial ini:

kubectl get pods
Menampilkan resource yang berada di namespace default, yaitu tempat aplikasi hello-node dijalankan.

kubectl get pods -n kube-system
Menampilkan resource di namespace kube-system, yang berisi komponen internal Kubernetes seperti coredns, kube-scheduler, dan metrics-server.

Namespace dalam Kubernetes berfungsi untuk mengelompokkan resource ke dalam ruang lingkup yang terpisah. Ketika perintah menggunakan -n kube-system, maka resource di namespace lain (seperti default) tidak akan terlihat.

Kesimpulan: Opsi -n digunakan untuk menentukan namespace tertentu. Tanpa opsi ini, kubectl hanya akan menampilkan resource dari namespace default.

---

#### Perbedaan antara strategi deployment Rolling Update dan Recreate
Strategi Rolling Update dan Recreate digunakan dalam Kubernetes untuk menentukan bagaimana proses pembaruan (update) terhadap aplikasi yang sedang berjalan dilakukan.
Berikut perbedaannya:

Rolling Update melakukan pembaruan secara bertahap, yaitu dengan men-deploy pod baru satu per satu sambil tetap menjaga agar pod lama tetap berjalan hingga pod baru siap. Ini cocok digunakan jika kamu ingin menghindari downtime, karena aplikasi tetap tersedia selama proses update.

Recreate akan menghentikan semua pod lama terlebih dahulu sebelum membuat pod baru. Strategi ini bisa menyebabkan downtime, tetapi lebih sederhana dan cocok untuk aplikasi yang tidak dapat menjalankan versi lama dan baru secara bersamaan (misalnya karena konflik database atau dependency).

Dari sisi keamanan dan kenyamanan pengguna, Rolling Update biasanya lebih disukai untuk aplikasi yang harus terus online, sedangkan Recreate bisa digunakan untuk update besar yang butuh "start fresh".

---

####  Manfaat menggunakan file manifest Kubernetes
Menggunakan file manifest Kubernetes sangat membantu karena mempermudah proses deployment menjadi lebih cepat, konsisten, dan otomatis. Dengan satu perintah kubectl apply -f, semua konfigurasi yang tertulis di dalam file YAML dapat langsung diterapkan ke cluster tanpa perlu mengetik perintah manual satu per satu. Ini mengurangi potensi kesalahan dan mempercepat pengelolaan aplikasi, terutama saat bekerja dalam tim atau mengelola banyak resource sekaligus.
