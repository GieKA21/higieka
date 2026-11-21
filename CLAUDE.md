# Konteks proyek higieka.web.id

Berkas ini berisi konteks yang perlu diketahui sebelum mengerjakan repositori ini.

## Apa ini

Situs portofolio pribadi As Shadiq Nur, mahasiswa Teknik Informatika UIN Alauddin
Makassar. Satu halaman statis, tanpa tahap build, tanpa dependensi selain font
yang dimuat dari Google Fonts.

Domain: `higieka.web.id` (registrar NyanHosting, domain saja tanpa hosting).
Hosting: GitHub Pages (pindah dari Netlify pada 14 November 2026), tayang otomatis
dari branch `main`, akar repositori. Berkas `CNAME` dan `.nojekyll` wajib tetap ada.

## Struktur

| Berkas         | Keterangan                                  |
| -------------- | ------------------------------------------- |
| `index.html`   | Seluruh halaman: markup, gaya, skrip, ikon  |
| `CNAME`        | Domain khusus untuk GitHub Pages            |
| `.nojekyll`    | Menyajikan berkas apa adanya, tanpa Jekyll  |
| `robots.txt`   | Izin perayapan mesin pencari                |
| `sitemap.xml`  | Peta situs                                  |
| `kkn/index.html` | Rancangan dashboard admin LKH KKN          |
| `pukat/index.html` | Pukat, pengumpul korpus media sosial berbasis Apify |

Seluruh CSS dan JavaScript berada inline di dalam `index.html`. Jangan memecahnya
menjadi berkas terpisah tanpa diminta; sifat satu-berkas adalah keputusan desain
yang disengaja.

Halaman `/kkn/` adalah rancangan antarmuka dashboard admin LKH KKN yang berdiri
sendiri: satu berkas, memakai tampilannya sendiri, bukan gaya portofolio. Halaman
`/kkn/` dan `/kkn/daftar/` berbagi satu set warna di `:root`: hijau malam `--malam`
(pita atas dan dok), hijau daun `--daun` (tombol utama), kuning padi `--padi` (hanya
untuk hari ini pada garis waktu dan pendaftar yang baru masuk), merah bata `--bata`
(galat dan penolakan), serta latar `--kertas`. Hurufnya Plus Jakarta Sans saja.
Elemen yang menonjol hanya satu: garis 45 hari di pita atas yang digambar seperti
batang padi tumbuh (hari yang lewat tinggi dan hijau, hari ini kuning, hari depan
pendek). Teks dibuat pendek: tanpa paragraf penjelas di bawah judul layar;
keterangan teknis masuk ke lipatan `details.lipat`. Galat isian formulir memakai
`:user-invalid`, jadi baru tampil setelah isian disentuh atau formulir dicoba
dikirim. Halaman itu tidak memuat data contoh: angkanya hanya berasal dari berkas
yang dimuat lewat menu Impor data dan disimpan di `localStorage` peramban (kunci
`kkn-dashboard-v1`), tidak dikirim ke mana pun. Halaman itu diberi `noindex`, serta
tidak dicantumkan pada `sitemap.xml`. Jangan memasukkan data mahasiswa sungguhan.

Halaman `/kkn/daftar/` adalah formulir uji coba pendaftaran KKN. GitHub Pages tidak
dapat menyimpan isian, jadi halaman itu mengirim isiannya ke aplikasi web Google Apps
Script milik pemilik situs, yang menambahkan satu baris ke Google Sheet. Alamatnya
disimpan pada variabel `ALAMAT_PENERIMA` di dalam halaman; selama kosong atau bukan
alamat `https://script.google.com/macros/s/.../exec`, tombol kirim tetap nonaktif.
Kode penerimanya (`terima-pendaftaran.gs`) tidak disimpan di repositori ini. Isian
dikirim sebagai formulir biasa tanpa kepala khusus, sebab Apps Script tidak melayani
permintaan pendahuluan (preflight). Pola isian yang diketik (nama, tempat lahir, hobi)
sama dengan pola pada penerima; ubah keduanya bersamaan. Judul kolom Sheet sama dengan
judul kolom yang dikenali pengimpor di `/kkn/`. Formulir ini terbuka tanpa masuk akun,
jadi hanya untuk data karangan sampai ada keputusan tentang pemilik formulir resmi.
Berkas pendaftar yang dimuat ke dashboard dibaca di peramban dan tidak dikirim ke peladen.

Menu Pendaftar pada `/kkn/` membaca Sheet yang sama lewat aplikasi web yang sama
(`aksi=daftar`) setiap 15 detik dan dapat mengubah kolom Status serta Catatan
(`aksi=status`). Kedua permintaan itu memerlukan kunci admin yang ditempel admin pada
dashboard dan disimpan di `localStorage` peramban itu saja; kunci tidak pernah ditulis
ke repositori. Isi Sheet tidak disimpan di peramban; hanya pendaftar yang dipakai untuk
penempatan yang masuk ke data peserta.

Halaman `/pukat/` adalah Pukat, sistem pengumpulan korpus media sosial berbasis Apify
yang menjadi bahan skripsi pemilik situs. Satu berkas dengan tipografinya sendiri
(Inter, Space Grotesk, dan JetBrains Mono), memakai tampilan gelap saja: latar
`#0B0E14`, bidang datar `#12161E` dan `#181D27`, garis tipis, serta aksen biru
`#3B82F6` dengan tombol utama `#2563EB`. Formulir berjarak lega, navigasi ramping
melekat di atas, dan tombol berbentuk pil selaras dengan beranda. Seluruh proses berjalan di
peramban: token Apify pengguna dipakai untuk memulai run aktor (TikTok, Instagram, X,
Facebook) lewat `api.apify.com`, memantau statusnya, mengunduh dataset, lalu mengolah
korpus (filter periode, relevansi kata kunci, bahasa, Melayu, deduplikasi, pseudonim
HMAC, penyamaran kontak, subset seimbang, sampel verifikasi) dan memeriksa ulang lewat
oEmbed X dan TikTok. Keluaran berupa CSV, `DATA_SIAP_PAKAI.xlsx` (SheetJS), log JSONL,
dan `MANIFEST.json` berisi hash SHA-256, dikemas dengan JSZip; kedua pustaka dimuat dari
cdnjs. Token tidak pernah disimpan. Riwayat bekerja tanpa akun: daftar proyek (config
dan antrean run), log, draf, dan kunci pseudonim disimpan di `localStorage`, sedangkan
hasil olahan dan salinan data mentah per run disimpan di IndexedDB (basis data `pukat`,
store `hasil` dan `mentah`) pada peramban pengguna. Bila sinkron aktif dan token sudah
diuji, riwayat juga disalin ke key-value store bernama `pukat-riwayat` di akun Apify
pengguna (rekaman `INDEKS`, `LOG-<id>`, `HASIL-<id>` dan `MENTAH-<runId>` berformat gzip),
sehingga dapat dibuka dari perangkat lain tanpa akun Pukat. Penggabungan memakai stempel
`diubah` per proyek dan daftar proyek terhapus; hanya proyek milik akun yang sedang dipakai
yang disinkronkan. Token dan kunci pseudonim tidak pernah ikut disinkronkan. Run juga dapat
dipulihkan dari riwayat run di akun Apify. Pemetaan kolom aktor mengikuti `korpus_agama.py`.
Jangan menambahkan token, kunci, atau data mentah ke repositori.

## Arah desain

Atas permintaan pemilik situs, tampilan dirancang agar sederhana, elegan, dan
berwibawa, serta tidak berkesan dibuat oleh AI. Warna aksen adalah biru; jangan
dikembalikan ke ungu.

- Hero mengembalikan langit malam bergradasi, 30 titik bintang statis,
  14 bintang berkelip, dan bintang jatuh. Bulan SVG inline berukuran besar
  memakai maria bertepi tidak beraturan, kawah beragam dengan tepian terang
  dan bayangan, sinar kawah, tekstur regolit, serta penggelapan tepi yang halus.
  Pendar berdenyut setiap 7 detik dan bulan melayang setiap 11 detik.
  Animasi hanya berjalan jika pengurangan gerak tidak diminta. Fase mengikuti
  kursor dan guliran; bayangannya tetap di luar SVG agar filter tidak digambar
  ulang saat cahaya bergerak. Navigasi ramping dan gaya tombol tetap dipakai.
- Navigasi berupa bilah ramping yang melekat di atas, berlatar tembus pandang
  dengan pemburaman ringan dan garis tipis. Menu aktif ditandai garis bawah.
- Latar biru hampir hitam `--bg` (`#0B0E14`), bidang datar `--panel`
  (`#12161E`), dan bidang ilustrasi `--panel-2` (`#181D27`) dipisahkan garis
  tipis `--line`, tanpa bayangan berat atau gradasi permukaan. Kartu proyek
  bersudut 20 piksel dan lebar konten maksimum 1100 piksel.
- Sistem tombol memakai pil utama biru gelap, pil sekunder bergaris tipis,
  dan tautan teks. Tombol KKN dan Pukat bergaya sama, dengan sasaran sentuh
  minimal 44 piksel serta keadaan sorot, tekan, dan fokus yang jelas.
- Aksen utama biru `#3B82F6`, varian terang `#93C5FD`, varian gelap `#2563EB`
  untuk latar tombol bertulisan putih (aksen utama tidak memenuhi kontras AA)
- Amber `#F2A93B` khusus sebagai penanda status aktif
- Seluruh teks memakai Plus Jakarta Sans; JetBrains Mono hanya untuk data seperti
  angka, koordinat, kode, dan daftar teknologi

Warna ditetapkan sebagai variabel pada `:root`: `--bg`, `--panel`,
`--panel-2`, `--line`, `--ink`, `--text`, `--muted`, `--accent`,
`--accent-rgb`, `--accent-lite`, `--accent-deep`, dan `--amber`.
Gunakan variabel tersebut, bukan nilai mentah. Teks isi berukuran 17–18 piksel
dengan tinggi baris sekitar 1,6; judul besar memakai jarak huruf rapat.

Hindari pola yang lazim pada halaman buatan AI: label kecil berkotak dengan huruf
kapital renggang, bingkai jendela tiruan bertitik tiga, judul aforistik, tanda pisah
panjang, dan daftar bertiga yang berulang. Judul bagian cukup berupa kata biasa
seperti "Tentang", "Proyek", dan "Kontak".

Ketentuan kualitas yang harus dipertahankan pada setiap perubahan:

- Responsif hingga lebar layar ponsel, tanpa gulir horizontal
- `prefers-reduced-motion` dihormati
- Cincin fokus papan ketik terlihat jelas
- Kontras warna memenuhi WCAG AA

## Interaksi dan visual proyek

Skrip inline di akhir `index.html` menjalankan beberapa interaksi: demo Lokasi pada
panel proyek Lokasi (joystick dapat diseret atau digerakkan dengan tombol panah,
peta dapat diklik), tahapan verifikasi wajah LKH KKN, tab laporan pemeriksa PKTI,
pilihan platform pada rekaman asal-usul korpus, tombol salin surel, jam WITA,
sorotan menu yang meluncur ke menu yang ditunjuk atau yang sedang dibaca, angka 192
yang menghitung naik, dan animasi muncul saat digulir.

Bulan pada hero bergerak dengan beberapa cara:

- Sisi terangnya mengikuti kursor, seolah kursor adalah matahari, sehingga fasenya
  berubah. Tanpa tetikus, fasenya berubah mengikuti guliran halaman.
- Bayangan bulan berada pada elemen `.moon__shade` di luar SVG dan dikendalikan
  oleh variabel `--lx`, `--ly`, dan `--k`, agar perubahan cahaya tidak menggambar
  ulang filter SVG. Gelung animasinya berbasis waktu dan berhenti saat cahaya tenang.
- Bulan melayang perlahan, pendarnya berdenyut, bintang berkelip, sesekali ada
  bintang jatuh, dan bulan bergerak paralaks saat digulir.

- Situs harus tetap utuh tanpa JavaScript. Kelas `js` pada `html` dipasang oleh
  skrip kecil di `head`; gaya yang menyembunyikan konten hanya berlaku di bawahnya.
- Animasi hanya berjalan di bawah `prefers-reduced-motion: no-preference`.
- Gelung animasi demo Lokasi berhenti saat joystick diam, meniru aplikasi aslinya.

Setiap kartu proyek memuat visual yang merupakan ilustrasi, bukan tangkapan layar
asli, dan keterangannya menyatakan hal itu. Jangan menyajikan ilustrasi seolah-olah
tangkapan layar asli. Jika pemilik situs memberikan tangkapan layar asli, visual
tersebut boleh diganti; periksa dahulu agar tidak memuat data pribadi.

## Gaya komunikasi

Saat berbicara langsung dengan pemilik repositori di dalam sesi, gunakan bahasa
Indonesia percakapan yang santai dan akrab sebagaimana berbicara dengan teman
dekat, serta sapa dia dengan panggilan "mate". Nada yang kaku dan formal tidak
dikehendaki dalam percakapan.

Ketentuan tersebut berlaku khusus untuk percakapan. Seluruh teks yang masuk ke
dalam berkas — isi situs, pesan commit, maupun dokumentasi — tetap ditulis dalam
bahasa Indonesia ragam formal sebagaimana diatur pada bagian Bahasa di bawah.

## Bahasa

Seluruh teks situs dan pesan commit ditulis dalam bahasa Indonesia ragam formal.
Atribut `lang` pada elemen `html` adalah `id`.

## Isi

Jangan mencantumkan karya milik orang lain pada situs ini. Beberapa judul skripsi
yang pernah dibahas pemilik situs adalah milik rekannya, bukan miliknya sendiri.

## Skill untuk pekerjaan web

Folder `.claude/skills/` berisi skill proyek yang dipasang dengan `npx skills`
(sumber dan komitnya tercatat di `skills-lock.json`):

- `modern-web-guidance` (Google Chrome): cari panduan dengan
  `npx -y modern-web-guidance@latest search "<kueri>"` sebelum menulis HTML, CSS,
  atau JavaScript sisi klien, lalu `retrieve` panduan yang cocok
- `frontend-design` (Anthropic): arah visual dan cara menghindari kesan templat
- `ui-ux-pro-max` (nextlevelbuilder): data pedoman UX, palet, dan pasangan huruf
- `web-design-guidelines` (Vercel): audit akhir terhadap Web Interface Guidelines

Berkas lisensi tiap skill ada di foldernya masing-masing.

## Verifikasi sebelum push

Periksa struktur HTML sebelum mendorong perubahan, lalu buka halaman di peramban
untuk memastikan konsol bebas galat dan tidak ada gulir horizontal pada lebar 320px:

```sh
python3 -c "
from html.parser import HTMLParser
VOID={'area','base','br','col','embed','hr','img','input','link','meta','source','track','wbr'}
class P(HTMLParser):
    def __init__(self): super().__init__(); self.s=[]; self.e=[]
    def handle_starttag(self,t,a):
        if t not in VOID: self.s.append((t,self.getpos()[0]))
    def handle_endtag(self,t):
        if t in VOID: return
        o,l=self.s.pop() if self.s else ('?',0)
        if o!=t: self.e.append(f'<{o}> baris {l} ditutup </{t}>')
p=P(); p.feed(open('index.html',encoding='utf-8').read())
print(p.e or 'struktur bersih')
"
```
