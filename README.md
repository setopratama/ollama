# Ollama Docker GPU Setup

Repository ini berisi konfigurasi dan panduan orkestrasi untuk menjalankan **Ollama** di dalam kontainer **Docker** dengan dukungan akselerasi perangkat keras **NVIDIA GPU** menggunakan CUDA.

---

## 🚀 Fitur Utama

- **Akselerasi GPU (CUDA):** Konfigurasi *hardware passthrough* penuh untuk memanfaatkan VRAM secara optimal (diuji pada NVIDIA RTX 3050 Laptop).
- **Volume Persistensi:** Model AI yang diunduh disimpan di luar kontainer (`./ollama-data`) agar tidak hilang ketika kontainer di-update atau dihapus.
- **Portabel & Cepat:** Setup sekali klik menggunakan Docker Compose.

---

## 🛠️ Prasyarat Sistem

Sebelum memulai, pastikan sistem host Anda memiliki:
1. **NVIDIA Driver** (Driver GPU aktif)
2. **NVIDIA Container Toolkit** terpasang pada Docker Engine
3. **Docker** & **Docker Compose**

---

## 🏁 Memulai Cepat (Quick Start)

### 1. Jalankan Kontainer
Nyalakan layanan Ollama di latar belakang (*background*):
```bash
docker compose up -d
```

### 2. Kelola & Jalankan Model AI

#### A. Mengunduh Model Baru
```bash
docker compose exec ollama ollama pull gemma3:1b
```

#### B. Sesi Chat Interaktif di Terminal
Gunakan flag `-it` agar terminal mengalokasikan sesi interaktif (TTY):
```bash
# Chat dengan Gemma 3 (1B - ringan & respons instan)
docker compose exec -it ollama ollama run gemma3:1b

# Chat dengan Qwen 2.5 (3B - optimal untuk tugas umum & multibahasa)
docker compose exec -it ollama ollama run qwen2.5:3b

# Chat dengan Mistral (7B - penalaran & instruksi kompleks)
docker compose exec -it ollama ollama run mistral:7b
```

> [!TIP]
> **Cara Keluar dari Chat:** Ketik `/bye` lalu tekan **Enter**, atau tekan pintasan **`Ctrl + D`**.

#### C. Prompt Sekali Jalan (Single Query)
Jika ingin mengirim prompt dan langsung menerima output tanpa masuk ke sesi interaktif:
```bash
docker compose exec ollama ollama run gemma3:1b "Jelaskan apa itu Docker secara singkat!"
```

#### D. Perintah Bermanfaat Lainnya
```bash
# Melihat daftar model yang terpasang
docker compose exec ollama ollama list

# Melihat model yang sedang aktif di VRAM GPU
docker compose exec ollama ollama ps
```

### 3. Tes Endpoint API
Pastikan API merespons dengan mengirimkan permintaan generate:
```bash
curl http://localhost:11434/api/generate -d '{
  "model": "gemma3:1b",
  "prompt": "Hi, how are you?",
  "stream": false
}'
```

---

## 📄 Dokumentasi Tambahan

- **[todo.md](todo.md):** Catatan tahapan dan rencana konfigurasi sistem.
- **[AGENTS.md](AGENTS.md):** Analisis performa, benchmark, alokasi memori VRAM, dan data pengujian inferensi.
