# Rencana Setup Ollama dengan Docker

Berikut adalah langkah-langkah yang akan saya lakukan untuk menyiapkan Ollama dengan Docker agar mudah di-manage:

## Todo List:

- [x] 1.0.0 **Menganalisis Kebutuhan Awal:**
    - Memahami model AI yang ingin dijalankan (ukuran, kebutuhan resource).
    - Memastikan sistem memiliki Docker terinstal dan berjalan.

- [x] 2.0.0 **Membuat Dockerfile (Opsional, jika kustomisasi diperlukan):**
    - Jika ada kebutuhan khusus (misalnya, menginstal dependensi tambahan di dalam container), saya akan membuat `Dockerfile`. Namun, untuk setup dasar, image Ollama resmi biasanya sudah cukup.

- [x] 3.0.0 **Menyiapkan `docker-compose.yml` untuk Orkestrasi:**
    - Membuat file `docker-compose.yml` untuk mendefinisikan layanan Ollama. Ini akan mencakup:
        - Image Ollama (misalnya, `ollama/ollama`).
        - Port mapping (misalnya, `11434:11434`).
        - Volume mapping untuk menyimpan model AI secara persisten di luar container (misalnya, `./ollama-data:/root/.ollama`). Ini penting agar model tidak hilang saat container dihapus atau di-update.
        - Pengaturan resource (CPU, RAM, GPU jika tersedia) jika diperlukan.
    - Contoh struktur `docker-compose.yml`:
        ```yaml
        version: '3.8'
        services:
          ollama:
            image: ollama/ollama
            container_name: ollama_server
            ports:
              - "11434:11434"
            volumes:
              - ./ollama-data:/root/.ollama
                        # Jika menggunakan GPU NVIDIA (seperti RTX), uncomment baris berikut dan sesuaikan
            deploy:
              resources:
                reservations:
                  devices:
                    - driver: nvidia
                      count: all
                      capabilities: [gpu]
        ```

- [x] 4.0.0 **Memulai Layanan Ollama:**
    - Menjalankan `docker-compose up -d` dari direktori yang sama dengan `docker-compose.yml` untuk membuat dan menjalankan container di latar belakang.

- [x] 5.0.0 **Mengunduh dan Menjalankan Model AI:**
    - Setelah container berjalan, saya akan menggunakan perintah `ollama pull <nama_model>` untuk mengunduh model AI yang diinginkan (misalnya, `ollama pull llama2`). Perintah ini bisa dijalankan melalui `docker exec -it ollama_server ollama pull <nama_model>`.
    - Setelah model diunduh, saya akan menjalankan model menggunakan `ollama run <nama_model>`.

- [x] 6.0.0 **Verifikasi dan Pengujian:**
    - Memastikan bahwa Ollama API dapat diakses (misalnya, melalui `curl http://localhost:11434`).
    - Menguji interaksi dengan model AI yang sudah diunduh.

- [x] 7.0.0 **Manajemen dan Pemeliharaan:**
    - Menjelaskan cara menghentikan (`docker-compose down`), memulai ulang (`docker-compose restart`), dan melihat log (`docker-compose logs -f`) container.
    - Menjelaskan cara memperbarui image Ollama dan model AI.
