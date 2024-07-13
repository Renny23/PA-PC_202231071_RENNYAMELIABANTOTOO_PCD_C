# PA-PC_202231071_RENNYAMELIABANTOTOO_PCD_C

# TEORI DETEKSI DAUN
Deteksi daun adalah salah satu aplikasi dalam bidang pengolahan citra yang bertujuan untuk mengidentifikasi dan mengekstraksi daun dari gambar digital. Tujuan utama dari deteksi daun adalah untuk mendapatkan informasi yang berguna mengenai karakteristik daun, seperti bentuk, ukuran, tekstur, dan distribusi spasialnya. Berikut adalah beberapa teori dasar yang terkait dengan deteksi daun:

### 1. Segmentasi Warna dan Tekstur
Pada tahap awal deteksi daun, biasanya dilakukan segmentasi untuk memisahkan daun dari latar belakang dan objek lainnya. Metode segmentasi ini sering kali menggunakan informasi warna dan tekstur daun yang berbeda dengan latar belakang. Misalnya, penggunaan ruang warna seperti RGB, HSV, atau Lab untuk menentukan rentang warna daun. Selain itu, informasi tekstur seperti kehalusan atau kerapatan piksel juga dapat digunakan untuk membantu membedakan daun dari latar belakang.

### 2. Ekstraksi Fitur
Setelah segmentasi, langkah berikutnya adalah ekstraksi fitur dari daun yang telah diidentifikasi. Fitur-fitur ini dapat mencakup:
- **Geometri**: Seperti bentuk daun (persegi panjang, bulat, lonjong), ukuran, dan orientasi.
- **Tekstur**: Misalnya, menggunakan metode statistik atau transformasi seperti Gabor untuk mengekstrak pola tekstur dari daun.
- **Warna**: Histogram warna dari daun dapat memberikan informasi mengenai distribusi warna di dalam daun.

### 3. Pengklasifikasi
Setelah fitur-fitur diekstraksi, langkah terakhir adalah mengklasifikasikan daun ke dalam kategori atau jenis tertentu. Pengklasifikasi dapat dilakukan menggunakan berbagai pendekatan seperti:
- **Pengenalan Pola**: Menggunakan metode pembelajaran mesin atau jaringan saraf untuk mengenali pola yang ada dalam fitur-fitur yang diekstraksi.
- **Pengenalan Berbasis Aturan**: Berdasarkan aturan heuristik atau pengetahuan ahli mengenai ciri-ciri khusus dari jenis daun yang dikenali.

### 4. Aplikasi
Deteksi daun memiliki banyak aplikasi praktis, termasuk dalam bidang pertanian (misalnya untuk deteksi penyakit tanaman), biologi (pencatatan jenis tumbuhan), dan lingkungan (pemantauan vegetasi). Di samping itu, deteksi daun juga dapat digunakan dalam sistem pengenalan citra untuk keperluan ilmu komputer vision dan robotika.

### Metode dan Algoritma Populer
Beberapa metode dan algoritma yang sering digunakan dalam deteksi daun termasuk:
- **Thresholding**: Untuk segmentasi berdasarkan tingkat kecerahan atau warna.
- **Deteksi Tepi**: Untuk menemukan kontur atau tepi daun.
- **Transformasi Morfologi**: Untuk membersihkan dan memperbaiki kontur yang ditemukan.
- **Ekstraksi Fitur**: Seperti ekstraksi bentuk, tekstur, dan warna.
- **Klasifikasi**: Dengan menggunakan teknik pembelajaran mesin seperti SVM, k-NN, atau jaringan saraf.

Deteksi daun adalah bidang yang terus berkembang dengan berbagai teknik dan pendekatan baru yang terus dikembangkan untuk meningkatkan akurasi dan efisiensi proses pengolahan citra.

# Mengimport CV
import cv2
import numpy as np
import matplotlib.pyplot as plt

Import Library: Mengimpor pustaka yang diperlukan, yaitu OpenCV (cv2), NumPy (np), dan matplotlib (plt).

# Mengimport Gambar Nanass
image = cv2.imread('Nanass.jpg')

Membaca Gambar: Menggunakan cv2.imread('Nanass.jpg') untuk membaca gambar 'Nanass.jpg' dan menyimpannya dalam variabel image.

# Mengkonversi Gambar dari BGR ke RGB
hsv_image = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

Konversi ke HSV: Menggunakan cv2.cvtColor(image, cv2.COLOR_BGR2HSV) untuk mengubah gambar dari mode warna BGR (yang digunakan oleh OpenCV) ke mode warna HSV (Hue, Saturation, Value)

# Mendefinikan Batas Bawah dan Atas Untuk Warna Kuning (Nanas)
lower_yellow = np.array([30, 50, 100], dtype="uint8")
upper_yellow = np.array([80, 255, 255], dtype="uint8")

Range Warna untuk Masking: Menentukan nilai batas bawah (lower_yellow, lower_green) dan batas atas (upper_yellow, upper_green) untuk mengidentifikasi warna kuning (nanas) dan hijau (daun) dalam mode warna HSV.

# Mendefinikan Batas Bawah dan Atas Untuk Warna Hijau (Daun)
lower_green = np.array([31, 60, 5], dtype="uint8")
upper_green = np.array([150, 255, 255], dtype="uint8")

Masking: Menggunakan cv2.inRange() untuk membuat masker (mask_pineapple dan mask_leaves) berdasarkan range warna kuning dan hijau yang telah ditentukan.

# Membuat Masker untuk Pixel Kuning dan Hijau
mask_pineapple = cv2.inRange(hsv_image, lower_yellow, upper_yellow)
mask_leaves = cv2.inRange(hsv_image, lower_green, upper_green)

Segmentasi: Menggunakan cv2.bitwise_and() untuk menerapkan masker pada gambar asli (image) dan menghasilkan gambar yang hanya menampilkan bagian nanas (segmented_pineapple) dan bagian daun (segmented_leaves).

# Melakukan Oprasi Bitwise
segmented_pineapple = cv2.bitwise_and(image, image, mask=mask_pineapple)
segmented_leaves = cv2.bitwise_and(image, image, mask=mask_leaves)

Masker Nanas Tanpa Daun: Membuat masker mask_pineapple_no_leaves dengan mengambil kebalikan dari masker daun (mask_leaves) menggunakan cv2.bitwise_not(), kemudian menggabungkan masker ini dengan masker nanas menggunakan cv2.bitwise_and().

# Melakukan Masker untuk Nanas tanpa Daun
mask_pineapple_no_leaves = cv2.bitwise_not(mask_leaves)
mask_pineapple_no_leaves = cv2.bitwise_and(mask_pineapple, mask_pineapple_no_leaves)

Menampilkan Gambar Hasil: Menggunakan matplotlib.pyplot.subplots() untuk membuat layout subplot 2x2. Menampilkan gambar asli, masker nanas tanpa daun, hasil segmentasi nanas, dan hasil segmentasi daun menggunakan imshow()

# Menampilkan Output gambar asli, masker nanas tanpa daun, segmentasi nanas, dan segmentasi daun
fig, axs = plt.subplots(2, 2, figsize=(12, 10))
axs[0, 0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
axs[0, 0].set_title('Nanas Asli')
axs[0, 1].imshow(mask_pineapple_no_leaves, cmap='gray')
axs[0, 1].set_title('Mask Nanas (Tanpa Daun)')
axs[1, 0].imshow(cv2.cvtColor(segmented_pineapple, cv2.COLOR_BGR2RGB))
axs[1, 0].set_title('Segmentasi Nanas')
axs[1, 1].imshow(cv2.cvtColor(segmented_leaves, cv2.COLOR_BGR2RGB))
axs[1, 1].set_title('Segmentasi Daun')

plt.show()

Menampilkan Plot: Menampilkan plot menggunakan plt.show()



