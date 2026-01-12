# **Learn Code Ryzenth UBT**

Dokumen ini menjelaskan alur logika, alasan desain, dan cara kerja internal Ryzenth UBT.  
Bukan tutorial pemula. Ditujukan untuk developer yang paham konsep backend, API auth, dan async runtime.

## **1. Filosofi Dasar**

Ryzenth UBT **bukan userbot biasa**.  
Prinsip utamanya:

- **Server punya kontrol penuh**
- **Client (userbot) gak dipercaya**
- **Source bisa bocor = deploy tetap bisa ditolak**
- **Semua keputusan penting di server**

Kalau dibilang singkat:  
**Client boleh pintar, tapi server selalu lebih galak.**

## **2. Arsitektur Singkat**

```
Userbot
 ├─ init.py
 ├─ runtime modules
 ├─ API client (HMAC)
 │
 ▼
Ryzenth API (Next.js)
 ├─ Auth (HMAC + nonce + timestamp)
 ├─ MongoDB (UBTUser, UBTLog)
 ├─ Deploy gate (ban / allow / version guard)
 └─ AI Moderator (PM / Group)
```

---

## **3. Identity = user_id**

- **Gak pake username** (bisa ganti)
- **Gak pake session token** (bisa expired)
- Semua **berbasis user_id Telegram**

**Kenapa?**  
`user_id` immutable, gak bisa diganti.  
Lebih murah & stabil buat indexing.

```js
UBTUser {
  user_id: number;
}
```

## **4. Signed API (Anti Bypass)**

Setiap request penting **wajib signed**.

**Header wajib:**
```
X-UBT-TS      // timestamp (ms)
X-UBT-NONCE   // random hex
X-UBT-SIGN    // HMAC(ts.user_id.nonce)
```

**Tujuan:**
- Anti replay
- Anti curl spam
- Source bocor = server bisa dipakai

**Kalau salah satu gagal:**
- Request ditolak
- Deploy dihentikan


## **5. Deploy Gate (Hard Stop)**

Userbot **gak bisa jalan sendiri**.

**Urutan startup:**
1. `/api/create/check-update`
2. `/api/create/log-update`
3. `/api/create/health`
4. Baru lanjut runtime

**Jika server balas:**
- `BANNED`
- `DISCONNECTED`
- `FAILED`

process exit  
`raise RuntimeError("DEPLOY_BLOCKED_BY_SERVER")`

Gak ada fallback. Gak ada bypass.

## **6. API Key (Connected Mode)**

API key **bukan static**.

**Flow:**
1. User allowlisted di server
2. `/api/provision/issue-key`
3. Key dikirim sekali
4. Client simpan lokal (`ubt_api_key.txt`)
5. Server simpan **hash saja**

**Validasi tiap request:**
```
sha256(api_key) === user.api_key_hash
```

**Kalau salah:**  
User dianggap disconnected = PM AI & fitur server mati.

## **7. AI Moderator (Server-Side)**

**AI gak jalan di userbot.**

**Kenapa?**
- Prompt bocor = aman
- Aturan bisa diubah tanpa update client
- Decision authoritative di server

**AI output = JSON strict:**
```json
{
  "is_allow": false,
  "is_deleted": true,
  "is_ban": false,
  "reason": "..."
}
```
Userbot cuma eksekutor.

## **8. PM Permit Logic**

**Flow singkat:**
1. User DM = masuk PM guard
2. AI analisa teks
3. Server kirim verdict
4. Userbot:
   - delete
   - warn
   - block
   - allowlist

Gak ada debat. Gak ada chat panjang.

## **9. Log & Telemetry**

Server selalu tahu:
- versi
- device
- OS
- platform
- IP
- user-agent

**Contoh:**
```
version=2026
device=CPython 3.10.19
os=Linux 6.1
```
Ini **bukan spyware**.  
Ini **deploy integrity check**.

## **10. Anti Copas / Source Theft**

**Scenario:**  
> Orang download ZIP upload ke GitHub = hapus .github

**Hasil:**
- commit hash gak cocok
- version guard gagal
- API reject
- deploy stop

**Source ada = sistem jalan.**

## **11. Kenapa Ribet?**

**Karena:**
- Userbot = target abuse
- Source Telegram bot mudah dicuri
- Keamanan bukan soal rahasia, tapi **kontrol**

Ryzenth UBT fokus ke:
- kontrol
- konsistensi
- enforce dari server

## **12. Target Pengguna**

**Cocok buat:**
- Developer berpengalaman
- Paham async & API
- Gak takut server bilang **NO**

**Bukan buat:**
- Copas hunter
- Jalanin dulu
- Edit 1 baris berharap lolos

## **13. Penutup**

- **Kalau kamu baca sampai sini dan paham** = kamu target user.
- **Kalau bingung kenapa deploy mati** = sistem memang dirancang begitu.

Ryzenth UBT **bukan soal jalan atau tidak**,  
tapi soal **diizinkan atau tidak**.
