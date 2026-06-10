# 🖥️ SSH Playground

Belajar command Linux gratis menggunakan GitHub Actions sebagai server sementara, diakses via SSH melalui tunnel Cloudflare.

---

## 🚀 Cara Penggunaan

Ada dua workflow tersedia, pilih salah satu sesuai kebutuhan:

---

### 1. `blank.yml` — SSH Playground via Secret
Semua kredensial disimpan di GitHub Secrets. Lebih aman karena tidak ada data sensitif yang terlihat di log.

**Persiapan (sekali saja):**
Set secrets berikut di repo: **Settings → Secrets and variables → Actions → New repository secret**

| Secret | Keterangan |
|---|---|
| `SSH_PASSWORD` | Password bebas untuk login SSH |
| `TELEGRAM_BOT_TOKEN` | Token bot dari [@BotFather](https://t.me/BotFather) |
| `TELEGRAM_CHAT_ID` | ID chat kamu dari [@userinfobot](https://t.me/userinfobot) |

**Cara pakai:**
1. Buka tab **Actions** → pilih **SSH Playground via secret**
2. Klik **Run workflow**
3. Isi durasi sesi (opsional, default 6 jam)
4. Klik **Run workflow** (hijau)
5. Tunggu notifikasi masuk ke Telegram berisi info koneksi

---

### 2. `manual.yml` — SSH Playground Manual
Kredensial diisi langsung saat trigger workflow. Cocok untuk penggunaan sekali pakai tanpa perlu setup secrets.

> ⚠️ **Perhatian:** Repo ini publik. Token Telegram dan Chat ID akan terlihat di log Actions. Gunakan workflow ini hanya jika kamu tidak keberatan, atau jadikan repo **private** terlebih dahulu.

**Cara pakai:**
1. Buka tab **Actions** → pilih **SSH Playground Manual**
2. Klik **Run workflow**
3. Isi semua field yang muncul:
   - **Password SSH** — password bebas untuk login
   - **Telegram Bot Token** — token dari @BotFather
   - **Telegram Chat ID** — ID chat kamu
   - **Durasi sesi** — dalam detik, default 21600 (6 jam)
4. Klik **Run workflow** (hijau)
5. Tunggu notifikasi masuk ke Telegram berisi info koneksi

---

## 🔌 Cara Konek SSH

Pastikan sudah install **cloudflared** sesuai OS kamu, lalu jalankan perintah konek di bawah.

> Ganti `HOSTNAME` dengan hostname dari notif Telegram,  
> contoh: `wrote-prime-sizes-assist.trycloudflare.com`

### Windows

1. Download cloudflared: [cloudflared-windows-amd64.exe](https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-windows-amd64.exe)
2. Simpan di folder mana saja, misalnya `C:\Users\user\Downloads\`
3. Buka **Command Prompt** atau **PowerShell**, lalu jalankan:

```cmd
ssh -o "ProxyCommand=C:\Users\user\Downloads\cloudflared-windows-amd64.exe access ssh --hostname %h" runner@HOSTNAME
```

### Linux

1. Install cloudflared:
```bash
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -o cloudflared
chmod +x cloudflared
sudo mv cloudflared /usr/local/bin/
```

2. Konek SSH:
```bash
ssh -o "ProxyCommand=cloudflared access ssh --hostname %h" runner@HOSTNAME
```

### macOS

1. Install cloudflared via Homebrew:
```bash
brew install cloudflare/cloudflare/cloudflared
```

Atau download manual:
```bash
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-darwin-amd64.tgz | tar xz
sudo mv cloudflared /usr/local/bin/
```

2. Konek SSH:
```bash
ssh -o "ProxyCommand=cloudflared access ssh --hostname %h" runner@HOSTNAME
```

---

## ⚠️ Catatan

- Sesi otomatis mati setelah durasi habis (maksimal **6 jam** limit GitHub Actions)
- Semua data di server akan **hilang** setelah sesi selesai — jangan simpan file penting di sini
- Gunakan untuk **belajar saja**, bukan production

---

## 📦 Spesifikasi Server

| | |
|---|---|
| OS | Ubuntu Latest |
| User | `runner` |
| Tunnel | Cloudflare (trycloudflare.com) |
