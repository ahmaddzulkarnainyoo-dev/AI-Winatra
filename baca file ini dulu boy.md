[baca ini dulu boy.md](https://github.com/user-attachments/files/31736928/baca.ini.dulu.boy.md)
# Readme First / Baca Ini Dulu Boy 

Selamat datang di proyek **Winatra AI (Notification-Based AI Assistant)**.

File ini bukan sekadar *blueprint* kasar, melainkan **panduan langkah demi langkah** bagi kamu yang ingin membangun, memodifikasi, dan merilis versi **Winatra AI** milik kamu sendiri. Kerangka proyek ini dirancang modular agar siapapun—baik developer pemula, profesional, maupun kamu yang menggunakan AI Code Generator—bisa langsung mengimplementasikannya tanpa hambatan.

---

## 📌 1. Konsep Utama Aplikasi

Aplikasi ini beroperasi sebagai utilitas *background service* di Android dengan fitur utama:
1. **Persistent Notification:** Berjalan terus di latar belakang via *Foreground Service* dengan notifikasi yang tidak bisa di-swipe hilangkan.
2. **Clipboard Manager Integration:** Mengakses memori salinan teks (*clipboard*) penggunanya secara otomatis saat dipicu.
3. **Custom API Key Support:** Pengguna memasukkan kunci API mereka sendiri (seperti Gemini API, OpenAI, atau Groq API) untuk fleksibilitas tanpa batas biaya server internal.
4. **Action Buttons di Notifikasi:**
   * **`[ JAWAB ]`**: Membaca teks di *clipboard*, mengirimkannya ke API AI, dan membalas jawaban langsung di dalam antarmuka notifikasi (*Quick Reply style*).
   * **`[ MATIKAN ]`**: Mematikan *Foreground Service* secara instan dan bersih.

---

## ⚙️ 2. Arsitektur Komponen

```
[ User Input API Key ] ──> [ EncryptedSharedPreferences ]
                                    │
                                    ▼
                     [ Foreground Service (ONGOING) ]
                                    │
             ┌──────────────────────┴──────────────────────┐
             ▼                                             ▼
     [ Action: JAWAB ]                             [ Action: MATIKAN ]
             │                                             │
   (Read ClipboardManager)                                 │
             │                                             │
   (Send HTTP Request AI API)                              │
             │                                             │
 (Update Notification View)                                │
             │                                             ▼
             └──────────────────────────────────────> [ Stop Service ]
```

---

## 🛠️ 3. Modul & Struktur Kodingan (Kotlin)

Jika kamu membuat aplikasi ini sendiri di Android Studio, pastikan kamu membagi kodenya ke dalam modul berikut:

1. **`MainActivity.kt`**: Halaman UI sederhana (menggunakan Jetpack Compose atau XML) untuk menginputkan API Key dan menekan tombol *Start/Stop Service*.
2. **`AIForegroundService.kt`**: Service utama yang memanggil `startForeground()` dan membangun UI Notifikasi menggunakan `NotificationCompat.Builder`.
3. **`NotificationActionReceiver.kt`**: `BroadcastReceiver` yang menangkap klik dari tombol notifikasi:
   - Mengambil teks menggunakan `ClipboardManager`.
   - Melakukan eksekusi asynchronous (menggunakan Kotlin Coroutines / Ktor / Retrofit).
4. **`AIServiceRepository.kt`**: Modul penghubung REST API untuk mengirim JSON ke endpoint AI (seperti Google Gemini REST API).

---

## 🔒 4. Deklarasi Izin Android (AndroidManifest.xml)

Tambahkan izin berikut pada file `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_SPECIAL_USE" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
<uses-permission android:name="android.permission.INTERNET" />
```

---

## 🤖 5. Copy-Paste Prompt Ini ke AI Kamu!

Jika kamu tidak mau koding manual dan ingin menggunakan AI (seperti ChatGPT, Claude, atau Gemini) untuk membuatkan seluruh source code aplikasi ini, **copy prompt di bawah ini dan berikan ke AI kamu:**

```text
Buatkan kode proyek Android lengkap (menggunakan Kotlin) untuk aplikasi bernama Winatra AI dengan spesifikasi berikut:

1. MainActivity:
   - Input Field untuk memasukkan Custom API Key (simpan di EncryptedSharedPreferences).
   - Tombol "Aktifkan Service" dan "Nonaktifkan Service".

2. Foreground Service (AIForegroundService):
   - Menampilkan Persistent Notification (Priority HIGH, ONGOING).
   - Memiliki 2 Action Button pada notifikasi: "JAWAB" dan "MATIKAN".

3. Action Handling (BroadcastReceiver / PendingIntent):
   - Jika tombol "JAWAB" ditekan: Ambil teks terkini dari ClipboardManager. Kirimkan teks tersebut bersama API Key ke API Google Gemini / OpenAI via HTTP Request secara asynchronous. Tampilkan respon AI tersebut langsung dengan memperbarui teks pada Notification Body.
   - Jika tombol "MATIKAN" ditekan: Panggil stopSelf() dan hapus notifikasi.

4. Sertakan seluruh konfigurasi AndroidManifest.xml dan dependencies build.gradle.kts yang dibutuhkan.
```

---

## 💡 6. Ide Pengembangan (Modifikasi Sesuka Hati)

Proyek ini adalah **kerangka dasar**. Kamu bebas merombak atau menambah fitur gokil lainnya, contohnya:
- **System Prompt Customization:** Menambahkan pilihan peran AI (misal: Mode Penerjemah, Mode Rangkuman Singkat, atau Mode Pemrogram).
- **Multi-Provider AI:** Fitur switcher antara OpenAI, Anthropic Claude, Groq (Llama 3), dan Google Gemini.
- **Floating Bubble / Quick Tile:** Menambahkan tombol shortcut di Quick Settings Bar Android.
- **Local History Database:** Menyimpan semua riwayat teks yang pernah dijawab ke dalam SQLite / Room Database lokal.

---

## 📄 Lisensi

File ini dirilis secara **Open Source**. Kamu bebas melakukan fork, clone, modifikasi, dan mendistribusikan ulang kode Winatra AI versi kamu sendiri. Selamat berkarya, boy! 🛠️🔥
