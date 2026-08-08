# Ryna API

<img src="https://cdn.zass.in/5jTNKGrL1C.gif" alt="Ryna Portfolio Banner" width="100%" />

REST API publik berisi scraper anime, donghua, & film/series, downloader media sosial, chat AI, tools audio (speech-to-text, text-to-speech), dan maker gambar.

- **Base URL:** `https://api.rynaqrtz.my.id`
- **Format response:** JSON (kecuali endpoint yang secara eksplisit disebut mengembalikan file/gambar/audio)
- **Autentikasi:** tidak ada, API publik
- **Rate limit:** 30 request/menit per IP

## Daftar Isi

- [Format Response](#format-response)
- [Rate Limit](#rate-limit)
- [Endpoint Sistem](#endpoint-sistem)
- [Anime](#anime)
- [Donghua](#donghua)
- [Nonton](#nonton)
- [Download](#download)
- [AI](#ai)
- [Tools](#tools)
- [Maker](#maker)

## Format Response

Sukses:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": { }
}
```

Gagal:
```json
{
  "status": false,
  "creator": "rynaqrtz",
  "message": "Pesan error"
}
```

## Rate Limit

30 request per menit per IP. Melebihi batas ini server merespons **HTTP 429** dengan header `Retry-After` (detik).

```bash
curl -i "https://api.rynaqrtz.my.id/api/anime/samehadaku?action=home"
# kalau kena limit:
# HTTP/1.1 429 Too Many Requests
# Retry-After: 42
```

## Endpoint Sistem

| Endpoint | Deskripsi |
|---|---|
| `GET /` | Info dasar API |
| `GET /health` | Health check (uptime, versi Node, memory, status database) |
| `GET /list` | Daftar seluruh endpoint yang aktif beserta parameternya, live sesuai kondisi server saat ini |

```bash
curl "https://api.rynaqrtz.my.id/health"
```

## Anime

5 sumber, dipanggil dengan pola yang sama:

```
GET /api/anime/{sumber}?action={action}&{parameter lain}
```

Sumber yang tersedia: `samehadaku`, `animeindo`, `animekuindo`, `anoboy`, `nontonanimex`.

`slug` untuk `detail`/`episode` selalu diambil dari hasil `home` atau `search` — bukan ditebak manual.

```bash
curl "https://api.rynaqrtz.my.id/api/anime/samehadaku?action=search&query=naruto"
curl "https://api.rynaqrtz.my.id/api/anime/samehadaku?action=detail&slug=naruto-shippuden"
```

Contoh respons `action=search`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "results": [
      { "title": "Naruto Shippuden", "link": "...", "slug": "naruto-shippuden", "cover": "..." }
    ],
    "pagination": { "currentPage": 1, "totalPages": 5, "hasNextPage": true }
  }
}
```

### samehadaku

| Action | Parameter | Contoh |
|---|---|---|
| `home` | `page` | `/api/anime/samehadaku?action=home&page=1` |
| `terbaru` | `page` | `/api/anime/samehadaku?action=terbaru&page=1` |
| `ongoing` | `page` | `/api/anime/samehadaku?action=ongoing&page=1` |
| `completed` | `page` | `/api/anime/samehadaku?action=completed&page=1` |
| `batch` | `page` | `/api/anime/samehadaku?action=batch&page=1` |
| `schedule` | `day` (mis. `monday`) | `/api/anime/samehadaku?action=schedule&day=monday` |
| `search` | `query` wajib | `/api/anime/samehadaku?action=search&query=naruto` |
| `detail` | `slug` wajib | `/api/anime/samehadaku?action=detail&slug=naruto-shippuden` |
| `episode` | `slug` wajib | `/api/anime/samehadaku?action=episode&slug=naruto-episode-1` |
| `genre` | `slug` wajib, `page` | `/api/anime/samehadaku?action=genre&slug=action&page=1` |
| `watch` | `slug` wajib | `/api/anime/samehadaku?action=watch&slug=naruto-episode-1` |

### animeindo

| Action | Parameter | Contoh |
|---|---|---|
| `home` | `page` | `/api/anime/animeindo?action=home&page=1` |
| `ongoing` | `page` | `/api/anime/animeindo?action=ongoing&page=1` |
| `schedule` | — | `/api/anime/animeindo?action=schedule` |
| `popular` | `page` | `/api/anime/animeindo?action=popular&page=1` |
| `rating` | `page` | `/api/anime/animeindo?action=rating&page=1` |
| `update` | `page` | `/api/anime/animeindo?action=update&page=1` |
| `completed` | `page` | `/api/anime/animeindo?action=completed&page=1` |
| `movie` | `page` | `/api/anime/animeindo?action=movie&page=1` |
| `tv` | `page` | `/api/anime/animeindo?action=tv&page=1` |
| `search` | `query` wajib | `/api/anime/animeindo?action=search&query=naruto` |
| `detail` | `slug` wajib | `/api/anime/animeindo?action=detail&slug=nama-slug` |
| `episode` | `slug` wajib | `/api/anime/animeindo?action=episode&slug=nama-slug` |

### animekuindo

| Action | Parameter | Contoh |
|---|---|---|
| `home` | `page` | `/api/anime/animekuindo?action=home&page=1` |
| `new` | `page` | `/api/anime/animekuindo?action=new&page=1` |
| `top` | `page` | `/api/anime/animekuindo?action=top&page=1` |
| `search` | `query` wajib | `/api/anime/animekuindo?action=search&query=naruto` |
| `genres` | — | `/api/anime/animekuindo?action=genres` |
| `genre` | `slug` wajib, `page` | `/api/anime/animekuindo?action=genre&slug=action&page=1` |
| `schedule` | — | `/api/anime/animekuindo?action=schedule` |
| `detail` | `slug` wajib | `/api/anime/animekuindo?action=detail&slug=nama-slug` |
| `episode` | `url` wajib (URL episode lengkap) | `/api/anime/animekuindo?action=episode&url=https://...` |

### anoboy

| Action | Parameter | Contoh |
|---|---|---|
| `home` | `page` | `/api/anime/anoboy?action=home&page=1` |
| `terbaru` | `page` | `/api/anime/anoboy?action=terbaru&page=1` |
| `ongoing` | `page` | `/api/anime/anoboy?action=ongoing&page=1` |
| `complete` | `page` | `/api/anime/anoboy?action=complete&page=1` |
| `episodes` | `page` | `/api/anime/anoboy?action=episodes&page=1` |
| `jadwal` | — | `/api/anime/anoboy?action=jadwal` |
| `search` | `query` wajib | `/api/anime/anoboy?action=search&query=naruto` |
| `detail` | `slug` wajib | `/api/anime/anoboy?action=detail&slug=nama-slug` |
| `episode` | `slug` wajib, `episode` wajib | `/api/anime/anoboy?action=episode&slug=nama-slug&episode=1` |
| `all` | — | `/api/anime/anoboy?action=all` |

### nontonanimex

| Action | Parameter | Contoh |
|---|---|---|
| `home` | `page` | `/api/anime/nontonanimex?action=home&page=1` |
| `terbaru` | `page` | `/api/anime/nontonanimex?action=terbaru&page=1` |
| `jadwal` | — | `/api/anime/nontonanimex?action=jadwal` |
| `ongoing` | `page` | `/api/anime/nontonanimex?action=ongoing&page=1` |
| `complete` | `page` | `/api/anime/nontonanimex?action=complete&page=1` |
| `genre` | `slug` wajib, `page` | `/api/anime/nontonanimex?action=genre&slug=action&page=1` |
| `search` | `query` wajib | `/api/anime/nontonanimex?action=search&query=naruto` |
| `detail` | `slug` wajib | `/api/anime/nontonanimex?action=detail&slug=nama-slug` |
| `episode` | `slug` wajib | `/api/anime/nontonanimex?action=episode&slug=nama-slug` |

## Donghua

```
GET /api/donghua/donghub?action={action}&{parameter lain}
```

| Action | Parameter | Contoh |
|---|---|---|
| `home` | — | `/api/donghua/donghub?action=home` |
| `schedule` | — | `/api/donghua/donghub?action=schedule` |
| `genres` | — | `/api/donghua/donghub?action=genres` |
| `search` | `query` wajib, `page` | `/api/donghua/donghub?action=search&query=battle+through+the+heaven` |
| `genre` | `slug` wajib, `page` | `/api/donghua/donghub?action=genre&slug=action&page=1` |
| `detail` | `slug` wajib | `/api/donghua/donghub?action=detail&slug=nama-slug-donghua` |
| `episode` | `slug` wajib | `/api/donghua/donghub?action=episode&slug=nama-slug-episode` |

```bash
curl "https://api.rynaqrtz.my.id/api/donghua/donghub?action=search&query=battle+through+the+heaven"
```

## Nonton

Film & series (live-action), sumber LK21.

```
GET /api/nonton/lk21?action={action}&{parameter lain}
```

`slug`/`url` untuk `detail` dan `stream` selalu diambil dari hasil `home`, `browse`, atau `search` — bukan ditebak manual.

| Action | Parameter | Contoh |
|---|---|---|
| `home` | — | `/api/nonton/lk21?action=home` |
| `browse` | `path` (default `/populer`), `page`, `type` (`movie`/`series`/`both`) | `/api/nonton/lk21?action=browse&path=/latest&page=1&type=movie` |
| `genres` | — | `/api/nonton/lk21?action=genres` |
| `countries` | — | `/api/nonton/lk21?action=countries` |
| `years` | — | `/api/nonton/lk21?action=years` |
| `search` | `query` wajib, `page` | `/api/nonton/lk21?action=search&query=agent+shaan` |
| `detail` | `url` wajib (URL lengkap dari hasil home/browse/search) | `/api/nonton/lk21?action=detail&url=https://tv10.lk21official.cc/agent-shaan-elite-pursuit-2026` |
| `stream` | `url` wajib (URL lengkap film/episode) | `/api/nonton/lk21?action=stream&url=https://tv10.lk21official.cc/agent-shaan-elite-pursuit-2026` |

Path yang bisa dipakai untuk `browse`: `/populer`, `/latest`, `/latest-series`, `/rating`, `/release`, `/most-commented`, `/top-movie-today`, `/genre/{slug}`, `/country/{slug}`, `/year/{tahun}`, `/quality/{slug}`.

```bash
curl "https://api.rynaqrtz.my.id/api/nonton/lk21?action=home"
curl "https://api.rynaqrtz.my.id/api/nonton/lk21?action=search&query=agent+shaan"
```

Contoh respons `action=search`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "query": "agent shaan",
    "page": 1,
    "totalPages": 1,
    "items": [
      { "title": "Agent Shaan: Elite Pursuit", "slug": "agent-shaan-elite-pursuit-2026", "url": "https://tv10.lk21official.cc/agent-shaan-elite-pursuit-2026", "year": "2026", "rating": "7.2", "quality": "HD", "poster": "https://..." }
    ]
  }
}
```

```bash
curl "https://api.rynaqrtz.my.id/api/nonton/lk21?action=detail&url=https://tv10.lk21official.cc/agent-shaan-elite-pursuit-2026"
```

Contoh respons `action=detail` (film — untuk series, hasilnya punya `seasons` berisi daftar episode, bukan `download`/`players` langsung):
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "type": "movie",
    "url": "https://tv10.lk21official.cc/agent-shaan-elite-pursuit-2026",
    "title": "Agent Shaan: Elite Pursuit",
    "rating": "7.2",
    "genres": ["Action", "Thriller"],
    "country": ["USA"],
    "synopsis": "...",
    "poster": "https://...",
    "download": "https://...",
    "players": [{ "server": "server1", "url": "https://..." }]
  }
}
```

```bash
curl "https://api.rynaqrtz.my.id/api/nonton/lk21?action=stream&url=https://tv10.lk21official.cc/agent-shaan-elite-pursuit-2026"
```

Contoh respons `action=stream`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "url": "https://tv10.lk21official.cc/agent-shaan-elite-pursuit-2026",
    "title": "Agent Shaan: Elite Pursuit",
    "players": [{ "server": "server1", "url": "https://...", "active": true }],
    "nav": [{ "text": "EPISODE BERIKUTNYA", "url": "https://..." }]
  }
}
```

## Download

| Endpoint | Parameter | Contoh |
|---|---|---|
| `GET /api/download/tiktok` | `url` wajib | `/api/download/tiktok?url=https://vt.tiktok.com/xxxxx` |
| `GET /api/download/facebook` | `url` wajib | `/api/download/facebook?url=https://www.facebook.com/watch?v=xxxxx` |
| `GET /api/download/pin` | `url` wajib (Pinterest) | `/api/download/pin?url=https://pin.it/xxxxx` |
| `GET /api/download/soundcloud` | `url` wajib | `/api/download/soundcloud?url=https://soundcloud.com/artis/judul` |
| `GET /api/download/spotify` | `url` wajib (link `open.spotify.com`) | `/api/download/spotify?url=https://open.spotify.com/intl-id/track/xxxxx` |

```bash
curl "https://api.rynaqrtz.my.id/api/download/tiktok?url=https://vt.tiktok.com/xxxxx"
```

Contoh respons `tiktok`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "title": "Judul video",
    "cover": "https://...",
    "duration": "15 detik",
    "video_hd": "https://...",
    "download_url": "https://...",
    "audio_mp3": "https://...",
    "author": "namauser"
  }
}
```

```bash
curl "https://api.rynaqrtz.my.id/api/download/spotify?url=https://open.spotify.com/intl-id/track/3AAAGS7iM1ekDywqdYMJG2"
```

Contoh respons `spotify`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "title": "Judul Lagu",
    "artist": "Nama Artis",
    "cover": "https://...",
    "download": "https://..."
  }
}
```

## AI

| Endpoint | Parameter | Contoh |
|---|---|---|
| `GET /api/ai/deepseek` | `prompt` wajib, `model` (`v4-flash` / `r1`, default `v4-flash`) | `/api/ai/deepseek?prompt=halo&model=r1` |
| `GET /api/ai/sakana` | `prompt` wajib, `model`, `search` (`1` aktifkan pencarian web), `thinking` (`1` tampilkan reasoning) | `/api/ai/sakana?prompt=cuaca+jakarta+hari+ini&search=1` |
| `GET /api/ai/rewind` | `prompt` wajib, `model` (default `qwen/qwen-2.5-7b-instruct`) | `/api/ai/rewind?prompt=halo` |
| `GET /api/ai/notrack` | `prompt` wajib, `persona` (`normal`/`concise`/`detailed`/`creative`, default `normal`), `chat_id` (lanjutkan percakapan sebelumnya) | `/api/ai/notrack?prompt=jelaskan+relativitas&persona=concise` |
| `GET /api/ai/deepai` | `prompt` wajib (kecuali `action=models`), `model` (default `standard`) | `/api/ai/deepai?prompt=halo&model=llama-4-scout` |
| `GET /api/ai/deepai?action=models` | — | Daftar model terkini dari provider |
| `GET /api/ai/surfsense` | `prompt` wajib | `/api/ai/surfsense?prompt=halo` |
| `GET /api/ai/omegatech` | `message` wajib, `sessionId` (opsional, lanjutkan percakapan, maks 5 pesan per sesi) | `/api/ai/omegatech?message=halo` |

```bash
curl "https://api.rynaqrtz.my.id/api/ai/deepseek?prompt=halo"
curl "https://api.rynaqrtz.my.id/api/ai/notrack?prompt=jelaskan+relativitas&persona=concise"
```

Model DeepAI yang tersedia (cek `?action=models` untuk daftar terbaru langsung dari provider):

| Model ID | Nama |
|---|---|
| `standard` | Standard |
| `deepseek-v3.2` | DeepSeek V3.2 |
| `gemini-2.5-flash-lite` | Gemini 2.5 Flash Lite |
| `gemma-4` | Gemma 4 |
| `gpt-4.1-nano` | GPT-4.1 Nano |
| `gpt-oss-120b` | GPT OSS 120B |
| `gpt-5-nano` | GPT-5 Nano |
| `llama-3.3-70b-instruct` | Llama 3.3 70B Instruct |
| `llama-3.1-8b-instant` | Llama 3.1 8B Instant |
| `llama-4-scout` | Llama 4 Scout |

Contoh respons `deepai`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "model": "llama-4-scout",
    "model_name": "Llama 4 Scout",
    "response": "Halo! Ada yang bisa saya bantu?"
  }
}
```

Contoh respons `notrack` (simpan `chat_id` untuk melanjutkan percakapan yang sama):
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "chat_id": "abc123",
    "persona": "concise",
    "response": "Relativitas adalah..."
  }
}
```

Contoh respons `surfsense`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "prompt": "halo",
    "result": "Halo! Ada yang bisa saya bantu?"
  }
}
```

## Tools

### `POST /api/tools/transcribe`

Ubah audio jadi teks (speech-to-text).

| Parameter | Wajib | Keterangan |
|---|---|---|
| `audio` | salah satu wajib | File audio, `multipart/form-data`. Format: flac, mp3, mp4, mpeg, mpga, m4a, ogg, wav, webm. Maks 25MB |
| `url` | salah satu wajib | URL audio publik, dipakai kalau tidak kirim file |
| `language` | tidak | Kode bahasa (mis. `id`, `ja`, `en`), default `auto` |
| `quality` | tidak | `fast` atau `accurate`, default `fast` |

Kirim salah satu antara `audio` atau `url`.

```bash
curl -X POST "https://api.rynaqrtz.my.id/api/tools/transcribe" \
  -F "audio=@rekaman.mp3" \
  -F "language=id"
```

```bash
curl -X POST "https://api.rynaqrtz.my.id/api/tools/transcribe" \
  -F "url=https://contoh.com/audio.mp3" \
  -F "language=ja"
```

Contoh dari browser:
```js
const form = new FormData();
form.append('audio', blob, 'rekaman.webm');
form.append('language', 'ja');

const res = await fetch('https://api.rynaqrtz.my.id/api/tools/transcribe', {
  method: 'POST',
  body: form
});
const data = await res.json();
```

Contoh respons:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "text": "こんにちは、元気ですか？",
    "language": "ja",
    "model": "whisper-large-v3-turbo"
  }
}
```

### `GET /api/tools/omegatech-transcribe`

Alternatif `/api/tools/transcribe` lewat provider berbeda (Omegatech). Berguna sebagai fallback kalau Groq sedang limit. Hanya menerima URL audio publik, tidak menerima upload file langsung.

| Parameter | Wajib | Keterangan |
|---|---|---|
| `audioUrl` | ya | URL audio publik |
| `languageCode` | tidak | Kode bahasa (mis. `id`, `ja`, `en`), default `auto` |
| `scenario` | tidak | Default `auto` |

```bash
curl "https://api.rynaqrtz.my.id/api/tools/omegatech-transcribe?audioUrl=https://contoh.com/audio.mp3&languageCode=ja"
```

Contoh respons:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "audioUrl": "https://contoh.com/audio.mp3",
    "languageCode": "ja",
    "durationMinutes": 1,
    "transcription": "こんにちは。リリーです。"
  }
}
```

### `GET /api/tools/tts`

Translate teks dan/atau ubah jadi audio (text-to-speech).

| Parameter | Wajib | Keterangan |
|---|---|---|
| `text` | ya | Teks sumber |
| `from` | tidak | Kode bahasa sumber, default `auto` |
| `to` | tidak | Kode bahasa tujuan, default `id` |
| `mode` | tidak | `text-to-text` (JSON) atau `text-to-audio` (file MP3), default `text-to-text` |
| `translate` | tidak | `1`/`0`, translate dulu sebelum TTS, default `1` |

```bash
curl "https://api.rynaqrtz.my.id/api/tools/tts?text=Selamat+pagi&from=id&to=ja&mode=text-to-text"
```

```bash
curl "https://api.rynaqrtz.my.id/api/tools/tts?text=Selamat+pagi&from=id&to=ja&mode=text-to-audio" \
  --output suara.mp3
```

Mode `text-to-audio` mengembalikan file MP3 langsung (`Content-Type: audio/mpeg`), bisa dipakai langsung sebagai `src` audio:
```js
const audio = new Audio(
  `https://api.rynaqrtz.my.id/api/tools/tts?text=${encodeURIComponent('Selamat pagi')}&from=id&to=ja&mode=text-to-audio`
);
audio.play();
```

Contoh respons `mode=text-to-text`:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "from": "id",
    "to": "ja",
    "result": "おはようございます",
    "pronunciation": "Ohayō gozaimasu",
    "correction": null
  }
}
```

### `POST /api/ai/sensei`

Audio ucapan murid ditranskrip, dijawab AI dalam bahasa Jepang, lalu dikirim balik sebagai teks + terjemahan + romaji + audio, dalam satu request.

| Parameter | Wajib | Keterangan |
|---|---|---|
| `audio` | salah satu wajib | File audio ucapan murid |
| `url` | salah satu wajib | URL audio publik, dipakai kalau tidak kirim file |
| `level` | tidak | `pemula`/`menengah`/`mahir`, default `pemula` |

```bash
curl -X POST "https://api.rynaqrtz.my.id/api/ai/sensei" \
  -F "audio=@ucapan.webm" \
  -F "level=pemula"
```

```js
async function askSensei(blob, level = 'pemula') {
  const form = new FormData();
  form.append('audio', blob, 'rekaman.webm');
  form.append('level', level);

  const res = await fetch('https://api.rynaqrtz.my.id/api/ai/sensei', {
    method: 'POST',
    body: form
  });
  const data = await res.json();
  if (!data.status) throw new Error(data.message);

  const audio = new Audio(`data:audio/mp3;base64,${data.result.sensei_audio_base64}`);
  audio.play();

  return data.result;
}
```

Contoh respons:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "user_text": "わたし わ がくせい です",
    "sensei_text": "こんにちは！「わたしは学生です」が正しい言い方ですよ。頑張ってくださいね！",
    "translation": "Halo! Ucapan yang benar adalah \"watashi wa gakusei desu\". Semangat ya!",
    "romaji": "Kon'nichiwa! Watashi wa gakusei desu ga tadashii iikata desu yo. Ganbatte kudasai ne!",
    "sensei_audio_base64": "SUQzBAAAAAAAI1RTU0U...",
    "audio_format": "mp3"
  }
}
```

`sensei_audio_base64` adalah audio MP3 dalam bentuk base64, siap dipakai langsung sebagai `data:audio/mp3;base64,...`.

### `GET /api/tools/screenshot`

Screenshot fullpage sebuah website (dark mode, cookie banner disembunyikan).

| Parameter | Wajib | Keterangan |
|---|---|---|
| `url` | ya | URL website target |

```bash
curl "https://api.rynaqrtz.my.id/api/tools/screenshot?url=https://example.com"
```

Contoh respons:
```json
{
  "status": true,
  "creator": "rynaqrtz",
  "result": {
    "url": "https://storage.urlbox.com/xxxxx.png"
  }
}
```

## Maker

Dua endpoint di bawah **tidak mengembalikan JSON** — hasilnya langsung file gambar (`Content-Type: image/png`). Buka URL-nya langsung di browser, atau simpan sebagai file lewat kode.

| Endpoint | Parameter | Contoh |
|---|---|---|
| `GET /api/maker/fakedev` | `name` wajib, `bio` wajib, `img` wajib (URL foto) | `/api/maker/fakedev?name=Budi&bio=Backend+Developer&img=https://...jpg` |
| `GET /api/maker/jsoncard` | `name` wajib, `title` wajib, `email` wajib, `link` wajib | `/api/maker/jsoncard?name=Budi&title=Fullstack+Dev&email=budi@mail.com&link=github.com/budi` |

```bash
curl "https://api.rynaqrtz.my.id/api/maker/fakedev?name=Budi&bio=Backend+Developer&img=https://example.com/foto.jpg" \
  --output kartu.png
```

---

Dibuat oleh **rynaqrtz**.
