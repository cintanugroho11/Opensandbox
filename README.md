# OpenClaw Sandbox Configuration & Disablement Guide

Welcome to the **Opensandbox** repository. This repository serves as a comprehensive guide on how to configure and disable the agent sandbox in [OpenClaw](https://github.com/openclaw/openclaw), an advanced agent runtime environment.

## ⚠️ Security Warning

**Disabling the sandbox grants the AI agent direct access to your host operating system.**
When the sandbox is disabled, the agent can:
- Read, modify, or delete any files accessible by the user running the OpenClaw gateway.
- Execute arbitrary system commands natively.
- Access the host's networking environment directly.

Only disable the sandbox if you fully trust the agent's prompts and you are running OpenClaw in a secure or disposable environment (such as a dedicated Virtual Machine or VPS).

---

## 🛠️ How to Disable the OpenClaw Sandbox

By default, OpenClaw isolates agent operations using Docker-based sandbox containers. To turn off this isolation and allow the agent to run commands directly on the host, use the OpenClaw CLI config setter.

### Step 1: Disable the Sandbox Mode
Run the following command in your terminal:
```bash
openclaw config set agents.defaults.sandbox.mode off
```

### Step 2: Restart the Gateway
For the changes to take effect, restart your OpenClaw gateway daemon:
```bash
openclaw gateway restart
```
*(Or simply stop and start your gateway process if running it manually via `openclaw gateway`)*

### Step 3: Verify the Configuration
You can verify the effective sandbox configuration by running:
```bash
openclaw sandbox explain
```
Look for `mode: off scope: agent perSession: false` in the output, which confirms the agent is running in `direct` (host) mode.

---

## 📚 Real-World Use Case: The Aksara Baru AI System

In our production environment, we disabled the sandbox to build a complex, host-dependent automation system.

**Our active repository:** [Aksarabaru.com)

### About Aksara Baru AI
Aksara Baru AI is an automated, AI-powered digital news platform. It utilizes a sophisticated 4-Layer News Generation architecture:

### Why We Disabled the Sandbox for This Project:
To make the Aksara Baru system work efficiently, the agent (Cinta) required native access to:
- **System Cron Jobs:** We set up host-level schedulers using `openclaw cron` to run the news scraper and WordPress publisher.
- **Python Virtual Environments:** Native execution allowed the agent to install and manage heavy Python packages (`requests`, `beautifulsoup4`, `google-api-python-client`) without rebuilding Docker images.
- **File System Persistence:** We maintain large `.jsonl` databases for article memory and logs directly in the workspace directory.
- **Local WordPress Integration:** Native API requests to the WordPress server without dealing with Docker networking bridges.

By turning off the sandbox (`agents.defaults.sandbox.mode off`), the agent effectively became a native Linux system administrator, capable of coding, debugging, and executing the Aksara Baru pipeline entirely on its own.

---

## 🔄 Re-enabling the Sandbox

If you ever need to turn the sandbox back on for security purposes, you can revert the setting to its default (Docker-based isolation):

```bash
openclaw config set agents.defaults.sandbox.mode docker
openclaw gateway restart
```

---

# 🇮🇩 Panduan Konfigurasi & Menonaktifkan Sandbox OpenClaw (Versi Indonesia)

Selamat datang di repositori **Opensandbox**. Repositori ini berfungsi sebagai panduan komprehensif tentang cara mengonfigurasi dan menonaktifkan agen sandbox di [OpenClaw](https://github.com/openclaw/openclaw), sebuah lingkungan *runtime* agen tingkat lanjut.

## ⚠️ Peringatan Keamanan

**Menonaktifkan sandbox memberikan agen AI akses langsung ke sistem operasi host Anda.**
Saat sandbox dinonaktifkan, agen dapat:
- Membaca, memodifikasi, atau menghapus file apa pun yang dapat diakses oleh *user* yang menjalankan gateway OpenClaw.
- Mengeksekusi perintah sistem (shell) secara langsung.
- Mengakses jaringan host secara langsung.

Hanya nonaktifkan sandbox jika Anda sepenuhnya memercayai perintah (prompt) agen dan Anda menjalankan OpenClaw di lingkungan yang aman atau sekali pakai (seperti Virtual Machine khusus atau VPS).

---

## 🛠️ Cara Menonaktifkan Sandbox OpenClaw

Secara bawaan (*default*), OpenClaw mengisolasi operasi agen menggunakan container sandbox berbasis Docker. Untuk mematikan isolasi ini dan mengizinkan agen menjalankan perintah langsung di sistem host, gunakan pengatur konfigurasi CLI OpenClaw.

### Langkah 1: Nonaktifkan Mode Sandbox
Jalankan perintah berikut di terminal Anda:
```bash
openclaw config set agents.defaults.sandbox.mode off
```

### Langkah 2: Restart Gateway
Agar perubahan diterapkan, *restart* daemon gateway OpenClaw Anda:
```bash
openclaw gateway restart
```
*(Atau cukup hentikan dan mulai ulang proses gateway Anda jika menjalankannya secara manual via `openclaw gateway`)*

### Langkah 3: Verifikasi Konfigurasi
Anda dapat memverifikasi konfigurasi sandbox yang sedang aktif dengan menjalankan:
```bash
openclaw sandbox explain
```
Cari keterangan `mode: off scope: agent perSession: false` pada *output*, yang mengonfirmasi bahwa agen berjalan dalam mode `direct` (host).

---

## 📚 Contoh Kasus Nyata: Sistem AI Aksara Baru

Di lingkungan produksi kami, kami menonaktifkan sandbox untuk membangun sistem otomatisasi yang kompleks dan bergantung pada sistem operasi host.

**Repositori aktif kami:** [Aksara Baru AI](https://github.com/cintanugroho11/aksara-baru-ai)

### Tentang Aksara Baru AI
Aksara Baru AI adalah platform berita digital otomatis bertenaga AI. Sistem ini memanfaatkan arsitektur 4-Layer News Generation yang canggih:
1. **Layer 1:** Web Scraper (BeautifulSoup/Selenium) mengumpulkan data *real-time* dari portal berita teratas Indonesia.
2. **Layer 2:** LLM Super Cepat (Groq / Llama 3) menghasilkan draf artikel awal.
3. **Layer 3:** Skrip Python menjalankan Quality Gate (Deduplikasi MinHash, Pengecekan Kepatuhan Konten).
4. **Layer 4:** LLM Tingkat Lanjut (OpenRouter) memoles artikel dengan standar jurnalistik Indonesia yang ketat (KBBI & EYD V).

### Mengapa Kami Menonaktifkan Sandbox untuk Proyek Ini:
Agar sistem Aksara Baru dapat bekerja secara efisien, agen (Cinta) membutuhkan akses *native* ke:
- **System Cron Jobs:** Kami mengatur penjadwalan otomatis tingkat host menggunakan `openclaw cron` untuk menjalankan *scraper* berita dan *publisher* WordPress.
- **Python Virtual Environments:** Eksekusi *native* memungkinkan agen menginstal dan mengelola paket Python secara langsung (`requests`, `beautifulsoup4`, `google-api-python-client`) tanpa perlu membangun ulang *image* Docker.
- **File System Persistence:** Kami menyimpan *database* `.jsonl` berukuran besar untuk riwayat artikel dan log langsung di dalam direktori *workspace*.
- **Local WordPress Integration:** Dapat melakukan *request* API secara *native* ke server WordPress tanpa harus berurusan dengan *bridge* jaringan Docker.

Dengan mematikan sandbox (`agents.defaults.sandbox.mode off`), agen secara efektif menjadi administrator sistem Linux *native*, yang mampu melakukan *coding*, *debugging*, dan mengeksekusi *pipeline* Aksara Baru sepenuhnya secara mandiri.

---

## 🔄 Mengaktifkan Kembali Sandbox

Jika suatu saat Anda perlu menghidupkan kembali sandbox untuk alasan keamanan, Anda dapat mengembalikan pengaturan ke *default*-nya (isolasi berbasis Docker):

```bash
openclaw config set agents.defaults.sandbox.mode docker
openclaw gateway restart
```

---
*Maintained by Cinta for Andry EN.*