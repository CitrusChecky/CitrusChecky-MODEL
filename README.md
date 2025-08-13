# 🍊 Model YOLOv8n — Deteksi Tingkat Kebusukan Buah Jeruk Secara Real-Time

> **Catatan:** **CitrusChecky** adalah **nama aplikasi Android** yang **menggunakan** model ini.  
> Repo ini berisi **model YOLOv8n** beserta aset dan panduan reproduksi/training/evaluasi.

<p align="center">
  <img src="https://img.shields.io/badge/Model-YOLOv8n-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Kelas-3%20kelas-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/GPU-NVIDIA%20A100-9cf?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Lisensi-MIT-lightgrey?style=for-the-badge" />
</p>

Model ini mendeteksi dan mengklasifikasikan **tingkat kebusukan buah jeruk** secara **real-time** ke dalam 3 kelas:
**Matang**, **Sedikit Busuk**, **Sangat Busuk**.  
Model terbaik (v1) meraih performa rata-rata: **Akurasi 0.93 • Presisi 0.84 • Recall 0.94 • F1 0.89**.

---

## 🔗 Tautan Penting
- 📓 **Notebook Training & Inference**: [YOLOv8n_deteksi_tingkat_kebusukan_buah_jeruk.ipynb](https://github.com/CitrusChecky/CitrusChecky-MODEL/blob/main/YOLOv8n_deteksi_tingkat_kebusukan_buah_jeruk.ipynb)
- 🗂 **Model v1 (terbaik)**: [v1-segmentasi-tingkat-jeruk.yolov8](https://github.com/CitrusChecky/CitrusChecky-MODEL/tree/main/v1-segmentasi-tingkat-jeruk.yolov8)
- 🗂 **Model v2**: [v2-segmentasi-tingkat-jeruk.yolov8](https://github.com/CitrusChecky/CitrusChecky-MODEL/tree/main/v2-segmentasi-tingkat-jeruk.yolov8)
- 🗂 **Model v3**: [v3-segmentasi-tingkat-jeruk.yolov8](https://github.com/CitrusChecky/CitrusChecky-MODEL/tree/main/v3-segmentasi-tingkat-jeruk.yolov8)

---

## 🧾 Ringkasan Dataset
- **Sumber utama (Kaggle):**
  1. https://www.kaggle.com/datasets/sriramr/fruits-fresh-and-rotten-for-classification  
  2. https://www.kaggle.com/datasets/raghavrpotdar/fresh-and-stale-images-of-fruits-and-vegetables
- **Pelabelan & segmentasi:** Roboflow  
- **Kelas & urutan label:** `matang`, `sedikit_busuk`, `sangat_busuk`

### 📊 Rincian Gambar & Anotasi (per Model)
| Model | Gambar Matang | Gambar Sangat Busuk | Gambar Sedikit Busuk | Anotasi Matang | Anotasi Sangat Busuk | Anotasi Sedikit Busuk |
|:----:|:--------------:|:-------------------:|:--------------------:|:--------------:|:--------------------:|:---------------------:|
|  1   | 215            | 234                 | 253                  | 266            | 266                  | 266                   |
|  2   | 215            | 234                 | 252                  | 266            | 266                  | 266                   |
|  3   | 303            | 305                 | 252                  | 383            | 419                  | 322                   |

> **Split data**: dilakukan untuk train/val/test; pelabelan mencakup **segmentasi** dan **bounding box**.

---

## 🛠 Teknologi & Lingkungan
- **YOLOv8n** (Ultralytics)
- **Google Colab Pro** — **GPU NVIDIA A100** (direkomendasikan)
- **Roboflow** — anotasi & augmentasi
- **TensorFlow Lite** — untuk deployment mobile
- **tflite_support.metadata_writers** — penambahan metadata TFLite

---

## 🔄 Alur Pengembangan Model
1. **Pengumpulan Dataset** (Kaggle)  
2. **Pemisahan 3 Kelas**: `matang`, `sedikit_busuk`, `sangat_busuk`  
3. **Segmentasi & Pelabelan** di **Roboflow** (segmentasi & bounding box)  
4. **Training** di **Google Colab (A100)** dengan **YOLOv8n**  
5. **Evaluasi** (confusion matrix: akurasi, presisi, recall, F1) → pemilihan model terbaik  
6. **Konversi ke TensorFlow Lite** (`yolo export`)  
7. **Penambahan Metadata** (input/output, label, normalisasi) ke file `.tflite`

---

## ⚙️ Parameter Training Utama
- **Model dasar**: `yolov8n.yaml`  
- **Epoch**: `1000` *(dengan early stopping)*  
- **Ukuran gambar**: `960`  
- **Batch size**: `32`  
- **Optimizer**: `Adam`  
- **Learning rate awal/akhir**: `0.0003 / 0.001`  
- **Augmentasi**: rotasi, hue, translasi/pergeseran, skala, shear, flip, **mosaic**, **mixup**, **copy-paste**, **erasing**

---

## ✅ Hasil Evaluasi (Rata-Rata per Model)
| Model | Akurasi | Presisi | Recall | F1-Score |
|:----:|:-------:|:-------:|:------:|:--------:|
|  1   | **0.93** | **0.84** | **0.94** | **0.89** |
|  2   | 0.90    | 0.79    | 0.89   | 0.83     |
|  3   | 0.84    | 0.49    | 0.88   | 0.61     |

> **Model terpilih** untuk deployment: **Model 1 (v1-segmentasi-tingkat-jeruk.yolov8)**.

---

## ▶️ Cara Menjalankan / Reproduksi
**Colab:**
1. Buka notebook:  
   `YOLOv8n_deteksi_tingkat_kebusukan_buah_jeruk.ipynb`
2. Jalankan sel secara berurutan (training → evaluasi → ekspor TFLite).
