# Ryzenth UBT  
**Userbot Yang Gak Bisa Diutak-Atik Sembarangan**

## ⚠️ BACA DULU SEBELUM COBA²

Kalo lo clone repo ini berharap bakal **langsung jalan** **gak bakal.**

Ryzenth UBT **bukan** userbot tinggal pakai.  
Ini sistem **yang dikontrol server**.  
Tanpa izin, gak bakal jalan.

## ❌ Peringatan Penting

Project ini **BUKAN** dibuat buat di fork, dijual lagi, atau dipakai ulang.

- Nyontoh source code **gak** otomatis dapet akses
- Hapus modul **gak** ngebypass restrictions
- Edit logic auth malah bikin deploy rusak
- Jalanin tanpa persetujuan server = auto gagal

Kalo lo gak ngerti kenapa,  
project ini **bukan buat lo**.

## Gimana Ryzenth UBT Beneran Kerja

**Client gak pernah mutusin. Server yang selalu mutusin.**

Setiap deploy butuh:

- API key dari server
- User ID yang udah diallowlist
- Status koneksi aktif
- Validasi policy tiap request

**Gak ada key = gak konek**  
**Gak diallow = gak deploy**  
**Kena ban = shutdown langsung**

## Desain Anti Copas (Sengaja Dibikin Gini)

Ryzenth UBT dibikin buat **nghukum source code yang dicopas**:

- API key gak pernah ada di repo
- Key dikasih sekali, disimpen pake hash di server
- Server ngecek:
  - `user_id`
  - Tanda tangan API key
  - Device / versi / build hash
- Kalo ada yang beda = `DISCONNECTED`

Lo bisa hapus `.github/`, `.env`, atau bahkan `init.py`.  
**Tetep gak bakal ngebantu.**

## Bisa Gak Aku Hapus Cek nya?

**Jawaban singkat:** Gak bisa.

**Jawaban panjang:**
- Hapus logic auth = header hilang
- Header hilang = ditolak server
- Ditolak server = bot berhenti
- Lo gak ngontrol servernya.  
  Lo gak pernah ngontrol.

## Ini Bukan Mainan Plugin

- Plugin cuma add-on, bukan pintu masuk
- Logic inti dikunci ke status server
- Keputusan AI moderator dari server
- Client cuma ngejalanin perintah

Kalo lo terbiasa:
- Nyomot plugin seenaknya
- Ngebypass cek
- Hardcode secret

**Berhenti di sini.**

## Ini Cocok Buat Siapa

- Developer yang ngerti infrastruktur
- Operator yang mau kontrol penuh
- Orang yang lebih peduli keamanan daripada gengsi
- Pembangun, bukan kolektor

## Ini Gak Cocok Buat:

- Anak script copas
- Penjual source curian
- Yang suka bilang "DM for source"
- Orang yang mikir hapus cek = hacker

## Notis Terakhir

Kalo lo gak dapet izin, ya emang lo gak seharusnya jalanin ini.

Ryzenth UBT itu **pribadi dari sononya**.  
Aksesnya harus disengaja.  
Gagalnya juga disengaja.


**Rilis:** 2027  
**Status:** Terkontrol / Pribadi  
**Akses:** Cuma yang diizinin
