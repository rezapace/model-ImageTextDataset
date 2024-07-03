# ImageTextDataset `dataset.py`

## Deskripsi
`ImageTextDataset` adalah sebuah kelas yang mengimplementasikan `torch.utils.data.Dataset` untuk memuat dataset gambar dan teks. Dataset ini dirancang untuk mempermudah proses pelatihan model pembelajaran mesin yang memerlukan input berupa gambar dan anotasi teks.

## Kegunaan
Kelas ini berguna untuk:
- Memuat gambar dari folder yang ditentukan.
- Memuat anotasi teks dari file yang ditentukan.
- Mengaplikasikan transformasi pada gambar jika diperlukan.
- Mengembalikan pasangan gambar dan anotasi teks dalam format yang siap digunakan untuk pelatihan model.

## Fungsi
Kelas `ImageTextDataset` memiliki beberapa fungsi utama:
- `__init__(self, image_folder, annotation_file, tokenize, transform=None)`: Inisialisasi dataset dengan folder gambar, file anotasi, fungsi tokenisasi, dan transformasi opsional.
- `__len__(self)`: Mengembalikan jumlah total pasangan gambar dan anotasi dalam dataset.
- `__getitem__(self, idx)`: Mengembalikan pasangan gambar dan anotasi teks berdasarkan indeks yang diberikan.
- `load_annotations(self, annotation_file)`: Memuat anotasi dari file yang diberikan dan mengembalikannya dalam format yang sesuai.

## Bagaimana Menjalankan
1. Pastikan Anda telah menginstal semua dependensi yang diperlukan:
    ```bash
    pip install torch torchvision pillow
    ```
2. Siapkan folder gambar dan file anotasi teks. File anotasi harus berformat teks dengan setiap baris berisi nama file gambar dan anotasi teks yang dipisahkan oleh koma dan spasi.
3. Buat instance dari `ImageTextDataset`:
    ```python
    from dataset import ImageTextDataset
    from torchvision import transforms

    # Definisikan transformasi opsional
    transform = transforms.Compose([
        transforms.Resize((128, 128)),
        transforms.ToTensor()
    ])

    # Fungsi tokenisasi sederhana
    def tokenize(text):
        return text.split()

    # Buat instance dataset
    dataset = ImageTextDataset(image_folder="path/to/images", 
                               annotation_file="path/to/annotations.txt", 
                               tokenize=tokenize, 
                               transform=transform)

    # Buat DataLoader
    dataloader = DataLoader(dataset, batch_size=32, shuffle=True)
    ```

## Kesimpulan
`ImageTextDataset` adalah solusi yang efisien dan mudah digunakan untuk memuat dataset yang terdiri dari pasangan gambar dan teks. Dengan menggunakan kelas ini, Anda dapat dengan mudah mempersiapkan data untuk pelatihan model pembelajaran mesin, termasuk mengaplikasikan transformasi pada gambar dan tokenisasi pada teks.

---

# Finetune CLIP Model dengan `finetune.py`

## Deskripsi
`finetune.py` adalah sebuah skrip Python yang digunakan untuk melakukan fine-tuning pada model CLIP (Contrastive Language-Image Pre-Training) dari OpenAI. Skrip ini memanfaatkan `PyTorch Lightning` untuk mempermudah proses pelatihan dan pengelolaan model. Dataset yang digunakan adalah pasangan gambar dan teks yang di-load menggunakan kelas `ImageTextDataset`.

## Kegunaan
Skrip ini berguna untuk:
- Melakukan fine-tuning pada model CLIP dengan dataset kustom.
- Mengoptimalkan model untuk tugas-tugas spesifik yang memerlukan pemahaman gambar dan teks.
- Memanfaatkan `PyTorch Lightning` untuk pelatihan yang efisien dan terukur.

## Fungsi
Skrip `finetune.py` memiliki beberapa fungsi utama:
- `ClipFinetuner`: Kelas yang mengimplementasikan `LightningModule` untuk fine-tuning model CLIP.
  - `__init__(self, clip_model)`: Inisialisasi dengan model CLIP.
  - `forward(self, image, text)`: Menghasilkan fitur gambar dan teks.
  - `training_step(self, batch, batch_idx)`: Melakukan satu langkah pelatihan dan menghitung loss.
  - `configure_optimizers(self)`: Mengonfigurasi optimizer untuk pelatihan.
- `main`: Fungsi utama yang memuat model CLIP, dataset, dan memulai proses pelatihan.

## Bagaimana Menjalankan
1. Pastikan Anda telah menginstal semua dependensi yang diperlukan:
    ```bash
    pip install torch torchvision pytorch-lightning clip-by-openai
    ```
2. Siapkan folder dataset dengan struktur berikut:
    ```
    example_dataset/
    ├── images/
    │   ├── image1.jpg
    │   ├── image2.jpg
    │   └── ...
    └── annotations.txt
    ```
    File `annotations.txt` harus berformat teks dengan setiap baris berisi nama file gambar dan anotasi teks yang dipisahkan oleh koma dan spasi.
3. Jalankan skrip `finetune.py`:
    ```bash
    python finetune.py
    ```

## Kesimpulan
`finetune.py` adalah solusi yang efisien dan mudah digunakan untuk melakukan fine-tuning pada model CLIP dengan dataset kustom. Dengan menggunakan skrip ini, Anda dapat mengoptimalkan model CLIP untuk berbagai tugas yang memerlukan pemahaman gambar dan teks, serta memanfaatkan kemudahan dan efisiensi yang ditawarkan oleh `PyTorch Lightning`.