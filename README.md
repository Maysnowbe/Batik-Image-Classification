# Batik-Image-Classification
Contributors: Gladys Lionardi, Phoebe Patricia Wibowo, Anastasia Jocelyn Hilman
Programming Language: Python

# Description
Batik merupakan salah satu warisan budaya Indonesia yang telah diakui oleh UNESCO secara internasional sejak 2 Oktober 2009. Batik merupakan sebuah gambar dengan kain sebagai mediumnya (Girsang & Muhathir, 2021). Dimana masing-masing motif memiliki nama tersendiri dan mengandung arti tertentu dari para leluhur (Muwafiq & Pamungkas, 2020).

Selain bentuk dan pola, tekstur batik juga turut andil dalam menentukan motifnya, seperti pola ber-outline tebal dengan kontras tinggi, atau garis outline tipis dengan kontras rendah. Tebal-tipisnya outline pola beserta ukuran ornamen utama batik tersebut mempengaruhi motifnya (Girsang & Muhathir, 2021). Oleh karena itu, diperlukan sebuah metode untuk membantu dalam membedakan motif-motif batik yang ada.

Dalam proyek ini, digunakan algoritma Deep Learning, khususnya CNN (Convolutional Neural Network) yang dapat dikatakan merupakan algoritma terbaik untuk mempelajari data dari image, dan memiliki performa yang sangat bagus untuk klasifikasi, segmentasi, dan deteksi image (Saleem dkk, 2022)

Data yang kami miliki terdiri dari 3 tipe batik, yaitu kawung, keraton, dan lasem, yang masing-masing terdiri atas 40-50 gambar. Dimana setiap gambar memiliki warna, ukuran, serta resolusi yang variatif. Selain itu, tidak semua gambar fokus kepada pola batik, melainkan ada yang berupa model mengenakan batik tersebut.

# Data Preprocessing
Data diimport dan di-resize menjadi ukuran 260x260.
Data yang dimiliki berjumlah total 145 gambar, yang dipecah menjadi 80% training, dan 20% test. Data tidak dipecah menjadi validation karena kurangnya jumlah data yang dimiliki.

Label encoder dan one-hot encoder untuk encoding labels dari batik, yaitu nama-nama batik (kawung, keraton, dan lasem) baik untuk data train maupun test.

Image pada train & test diubah menjadi grayscale, karena untuk membedakan batik, hanya perlu fokus pada polanya dan bukan warnanya (Girsang, 2021).
Kemudian, fitur di ekstrak menggunakan LBP. Dengan mengurangi color channels, beban model menjadi lebih ringan.
Semua image kemudian di normalisasi agar proses modeling lebih stabil & lancar

Karena dikitnya jumlah data, dilakukan augmentation untuk membiasakan model membaca berbagai macam jenis batik. Augmentasi dilakukan dengan mencoba merubah rotasi, height, dan width shift, shear, zoom, horizontal flip, dan fill untuk antisipasi pizel baru yang terbentuk setelah augmentasi. 

# Modeling
*Transfer Learning*
Digunakan model EfficientNetB2 yang sudah di pretrained dengan data 'ImageNet'. Setelah model di load, ditambahkan Global Average Pooling, batch normalization, dan dropout 0.5.
Pada output layer, digunakan activation function softmax.
Loss function yang dioptimalkan adalah categorical cross entropy, karena model untuk multiclass classification.

Compile menggunakan Adam Optimizer dengan learning rate 0,01
Model kemudian di fit dengan epoch 30, batch size 5, dan early stopping serta checkpoint untuk menyimpan model dengan loss terkecil

*Architecture from scratch*
Ada 3 model yang dibuat dari dasar. Model pertama menggunakan separable convolution dengan regularizer L2 untuk mencegah overfit.
Activation function 'ReLu' untuk convolution layernya, dan 'softmax' untuk output later, dengan jumlah parameter 3 juta. Kemudian, model di compile dan fit dengan hyperparameter yang sama dengan transfer learning.

Ketiga model yang dibuat dari dasar menggunakan hyperparameter yang sama, dan hanya parameternya saja yang di tuning.

Setelah evaluasi model pertama, model kedua dibangun dengan mengganti seperable convolution dengan convolution biasa, jumlah parameter 27 juta.

Setelah train & evaluasi, model ketiga dibangun dengan arsitektur yang sama seperti model kedua, namun tanpa regularizer.

# Evaluation
Berdasarkan accuracy adalah sebagai berikut: 
1. EfficientNetB2: 0.2759
2. Model 1: 0.3793
3. Model 2: 0.3448
4. Model 3: 0.3448

Berdasarkan loss adalah sebagai berikut: 
1. EfficientNetB2: 1.1435
2. Model 1: 3.5381
3. Model 2: 124670856
4. Model 3: 21866950

# Kesimpulan
Dari keempat model, dapat dilihat bahwa accuracy masih buruk, yang menandakan semua model yang dibangun belum stabil dan cocok dengan data yang ada.
