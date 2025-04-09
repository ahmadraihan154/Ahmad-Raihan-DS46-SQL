![meta-image](https://github.com/user-attachments/assets/ec9d90b2-2c9c-4d13-a389-bf35bfe1eb72)

# Tugas Submission Dicoding "Klasifikasi Gambar dari Belajar Pengembangan Machine Learning" by Ahmad Raihan

## Project Klasifikasi Gambar
Repository ini berisi proyek Ai Engineer yang saya kerjakan selama mengikuti program Laskar AI.

### A. Pengenalan Dataset
- Dalam pembuatan project ini, dataset yang digunakan merupakan gambar makanan yang diambil dari [Kaggle](https://www.kaggle.com/datasets/harishkumardatalab/food-image-classification-dataset).
- Dataset ini secara keseluruhan terdiri dari 34 kelas, dengan jumlah gambar yang bervariasi di setiap kelas, mulai dari 144 hingga 1500 gambar per kelas. Ukuran gambar juga bervariasi, dengan dimensi lebar atau tinggi berada dalam rentang 100 hingga 5000 piksel.
- Untuk keperluan pelatihan model, hanya 7 kelas yang dipilih secara selektif dengan total 1500 gambar. Sebelum diproses oleh model, seluruh gambar di-resize menjadi ukuran tetap 224x224 piksel.

### B. Kriteria Pengerjaan Project 
- Adapun 2 kriteria digunakan dalam pengerjaan project yaitu kriteria wajib dan kriteria tambahan.
#### 1. Kriteria Wajib
- Kriteria Wajib yang perlu saya lakukan agar project ini diterima yaitu sebagai berikut:
1. Bebas Memilih Dataset yang Ingin Dipakai, tetapi Harus Memiliki Minimal 1000 Gambar
2. Tidak Diperbolehkan Menggunakan Dataset yang Sudah Pernah Digunakan seperti dataset Rock, Paper, Scissors dan X-Ray
3. Dataset Dibagi Menjadi Train Set, Test Set dan Validation Set.
4. Model Harus Menggunakan Model Sequential, Conv2D, Pooling Layer
5. Akurasi pada Training dan Testing Set Minimal Sebesar 85%
6. Membuat Plot Terhadap Akurasi dan Loss Model
7. Menyimpan Model ke Dalam Format SavedModel, TF-Lite dan TFJS

#### 2. Kriteria Tambahan
- Kriteria tambahan yang saya kerjakan sehingga mendapat nilai terbaik:
1. Mengimplementasikan Callback
2. Gambar-gambar pada dataset memiliki resolusi yang tidak seragam.
3. Dataset yang digunakan berisi lebih dari 10000 gambar.
4. Akurasi pada training set dan validation set minimal 95%.
5. Memiliki 3 buah kelas atau lebih.
6. Melakukan inference menggunakan salah satu model (TF-Lite, TFJS atau savedmodel dengan tf serving).


### C. Preview Gambar
- Adapun 7 kelas yang dipilih dalam pengerjaan project dapat dilihat gambarnya dibawah:
![image](https://github.com/user-attachments/assets/3977833e-8bfb-4299-bbd3-b2a84c17be46)


### D. Model Evaluasi

#### a. Arsitektur Model
1. **MobileNetV2 Pre-trained**:
    - Menggunakan MobileNetV2 yang telah dilatih pada ImageNet dengan menghapus top layer (`include_top=False`).
    - Ukuran input model adalah `(224, 224, 3)`.

2. **Layer Beku**:
    - Semua layer MobileNetV2 dibekukan (`layer.trainable = False`) untuk mempertahankan bobot dan fitur yang telah dilatih.

3. **Layer Tambahan**:
    - Layer `Conv2D` dengan 128 filter, kernel size 3x3, dan aktivasi ReLU, diikuti oleh `MaxPooling2D` dengan pool size 2x2.

4. **Layer Flatten dan Fully Connected**:
    - Fitur yang didapatkan dari layer sebelumnya di-flatten menggunakan `Flatten`.
    - Layer `Dropout` dengan rate 0.3 untuk mencegah overfitting.
    - Layer `Dense` dengan 128 unit dan aktivasi ReLU.
    - Layer output `Dense` dengan 7 unit dan aktivasi softmax untuk klasifikasi multi-kelas.

#### b. Grafik Akurasi dan Loss 

![image](https://github.com/user-attachments/assets/ac997c09-d44f-43f8-ba2f-cf395085e5af)


| Epoch | Loss   | Accuracy | Val Loss | Val Accuracy |
|-------|--------|----------|----------|--------------|
| 1/10  | 0.5378 | 0.7982   | 0.1223   | 0.9620       |
| 2/10  | 0.1330 | 0.9593   | 0.0851   | 0.9769       |
| 3/10  | 0.0983 | 0.9697   | 0.1223   | 0.9620       |
| 4/10  | 0.0818 | 0.9755   | 0.0634   | 0.9785       |
| 5/10  | 0.0462 | 0.9851   | 0.1197   | 0.9620       |
| 6/10  | 0.0566 | 0.9831   | 0.0957   | 0.9711       |
| 7/10  | 0.0539 | 0.9842   | 0.1261   | 0.9661       |

![image](https://github.com/user-attachments/assets/769be769-8147-42cb-875b-0fcd7fef6e83)


#### c. Prediction (Inference)
![image](https://github.com/user-attachments/assets/6c3afd22-2387-42f1-bc9e-2589658cf43b)

- Insight:
    - Berdasarkan hasil inference, terlihat bahwa dari 7 kelas yang diprediksi, model berhasil mengklasifikasikan 6 gambar dengan benar. Namun, terdapat satu kesalahan prediksi pada gambar pertama yang seharusnya merupakan baked_potato, tetapi diprediksi sebagai sandwich.

    - Hal ini menunjukkan bahwa model masih memiliki keterbatasan dalam membedakan beberapa kelas makanan yang memiliki tampilan visual serupa. Meskipun demikian, secara keseluruhan performa model sangat baik, dengan akurasi mencapai 95% pada data training, validation, maupun testing.


### E. Cara Melakukan Inference
- Inference Menggunakan TensorFlow Serving.
  - Siapkan docker dekstop
  - Jalan command berikut pada terminal
  ```
  docker pull tensorflow/serving
  ```
- Install TensorFlow Serving Python API
    ```
    pip install tensorflow-serving-api
    ```
- Jalan command berikut pada terminal, ubah `YOUR_PATH`
    ```
    docker run -it -v YOUR_PATH\saved_model:/models -p 8501:8501 --entrypoint /bin/bash tensorflow/serving
    ```
- Jalan command berikut pada terminal
    ```
    tensorflow_model_server --rest_api_port=8501 --model_name=klasifikasi_model --model_base_path=/models/saved_model/
    ```
- Buka URL berikut pada browser untuk memastikan model berjalan
    ```
    http://localhost:8501/v1/models/klasifikasi_model
    ```
