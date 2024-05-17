# Tutorial Modul 11
## Reflection on Hello Minikube
### 1. Compare Logs
Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?

Sebelum 
![alt text](image.png)

Sesudah
![alt text](image-1.png)

Berdasarkan dokumentasi di atas, dapat dilihat bahwa log mengalami penambahan setiap kali kita membuka atau me-refresh aplikasi. Pada gambar diatas terlihat bahwa pada gambar sesudah terdapat tambahan log berupa `GET /` Hal ini terjadi karena pengguna melakukan request GET sehingga logs bertambah.

### 2. Penggunaan `-n`
What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

Opsi `-n` pada perintah `kubectl get` digunakan untuk menentukan namespace dari mana kita ingin melihat resource. Ketika Anda menjalankan `kubectl get` tanpa opsi `-n`, perintah ini hanya akan menampilkan resource di namespace default di mana Anda bekerja saat ini. Sebaliknya, menggunakan `kubectl get -n` diikuti dengan nama namespace, seperti `kube-system`, akan memfilter perintah untuk hanya menampilkan resource di dalam namespace yang ditentukan.

Tujuan utama dari opsi `-n` adalah untuk memungkinkan Anda melihat pods dan services di namespace tertentu. Jika Anda tidak menggunakan opsi `-n kube-system`, outputnya hanya akan mencakup resource dari namespace default. Akibatnya, pods dan services yang telah Anda buat secara eksplisit di namespace `kube-system` tidak akan ditampilkan karena perintah tanpa opsi `-n` tidak mencakup resource di luar namespace default.

Sebagai kesimpulan, opsi `-n` sangat penting untuk mengakses resource di namespace tertentu, memastikan bahwa Anda dapat melihat dan mengelola pods dan services yang spesifik untuk namespace tersebut, seperti yang ada di `kube-system`, yang sebaliknya akan diabaikan dari tampilan default.

## Reflection on Rolling Update & Kubernetes Manifest File
### 1. What is the difference between Rolling Update and Recreate deployment strategy? 
Rolling Update: Strategi ini memperbarui aplikasi secara bertahap untuk memastikan tidak ada downtime. Proses ini melibatkan penggantian pod lama dengan pod baru satu per satu. Pod baru diluncurkan dan ketika sudah berjalan, pod lama dihentikan. Ini memastikan aplikasi tetap tersedia selama proses update.

Recreate Deployment: Strategi ini menghentikan semua pod yang ada sebelum memulai pod yang baru. Selama proses update, tidak ada pod yang berjalan, sehingga terjadi downtime sementara. Pendekatan ini sepenuhnya menghentikan aplikasi sebelum memulai versi terbaru, yang dapat mengganggu ketersediaan layanan tetapi memastikan lingkungan baru yang bersih.

### 2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.
Saya melakukan deploy Spring Petclinic REST dengan recreate deployment berdasarkan panduan dari https://dev.to/cloudskills/kubernetes-deployment-strategy-recreate-3kgn Berikut adalah tahapan yang dilakukan:
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-5.png)
![alt text](image-6.png)

### 3. Prepare different manifest files for executing Recreate deployment strategy. Saya menjalankan command berikut untuk membuat deployment2.yaml
![alt text](image-7.png)

### 4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking kubectl apply -f command) to the cluster.
Menurut saya, penggunaan manifest files di Kubernetes sangat bermanfaat, terutama dalam hal efisiensi. Manifest files membantu memastikan konsistensi deployment karena semua konfigurasi disimpan dalam file YAML yang dapat dikontrol versinya, sehingga mengurangi kesalahan manusia. Selain itu, proses deployment menjadi lebih cepat dan mudah, cukup dengan menjalankan perintah kubectl apply -f <file.yaml>, dibandingkan dengan cara manual yang memerlukan banyak langkah dan rentan terhadap kesalahan.