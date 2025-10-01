# ECC-NIN-ChatAgent--Denny-Risnandar---09-
📰 Workflow n8n ini dibuat untuk mengecek sebuah harga setiap barang dari sebuah chatbot yang bersumber dari Marketplace Tokopedia
---

<img width="1598" height="791" alt="image" src="https://github.com/user-attachments/assets/dd2d8b3a-9057-46a6-8258-6c96231bec5b" />

---
menampilkan informasi dari
1. Produk top teratas
2. Toko penyedia barang
3. Harga barang
4. Link Pembelian
---

Alur Workflow

1. WebHook (Triger)
Kali ini akan menggunakan Webhook berupa Chatbot sederhana, yang mana ada berupa inputan. Inputan ini menjadi mesin sumber mencari informasi
Misalnya kita ketikan pada pesan tersebut : "tolong tampilkan harga raket badminton yonex".
adapun parameter pada webhook tsb seperti :

<img width="1000" height="500" alt="image" src="https://github.com/user-attachments/assets/d0cdb851-d4dc-42f1-91fe-b3ae2925a89c" />




2. AI AGENT
maka inputan tersebut akan di teruskan ke "AI AGENT"

adapun setting parameter sbg berikut
```
you have tools:
1. JinaAI -> use this to scrape the web using query parameter specified in that tools

#Rules
- give your output as is from JinaAI output
- do not modify or add any details, only give output from JinaAI

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ProductExtraction",
  "description": "Schema for extracted product data from marketplace text",
  "type": "array",
  "items": {
    "type": "object",
    "properties": {
      "category": {
        "type": "string",
        "description": "category of product, such as tablet, laptop, pulpen, baju, etc"
      },"product_name": {
        "type": "string",
        "description": "Clean name of the product, without seller info, ratings, or location"
      },
      "product_image": {
        "type": "string",
        "description": "url of the product image"
      },
      "seller_name": {
        "type": "string",
        "description": "name of the seller shop"
      },
      "product_price": {
        "type": ["integer", "null"],
        "description": "Price in Indonesian Rupiah as an integer (no Rp, no separators). If missing, null."
      },
      "source_url": {
        "type": "string",
        "format": "uri",
        "description": "The product source URL (Tokopedia, Shopee, etc.)"
      }
    },
    "required": ["product_name","seller_name", "product_price", "source_url"],
    "additionalProperties": false
  },
  "minItems": 1
}
```
Dalam AI AGENT tersebut kita pasang 
2A. Chat Model ; Google gemini chat model

2B. Http Request ; https://r.jina.ai/https://www.tokopedia.com/search


3. Information Extractor

   <img width="750" height="400" alt="image" src="https://github.com/user-attachments/assets/c0e13ddc-0864-4388-9674-9f6c56f08139" />


nformation Extractor pada n8n adalah sebuah node atau alat yang didukung AI untuk mengekstraksi data terstruktur secara otomatis dari teks tidak terstruktur






Telegram Trigger
Workflow akan aktif setiap kali ada user yang mengirim pesan ke bot Telegram.
Pesan user (misalnya: “Bandung”) akan diteruskan ke workflow.
AI Agent (Google Gemini + Tools)
Pesan dari user masuk ke AI Agent.
AI Agent diberi instruksi khusus:
Membuat URL API OpenWeather sesuai nama kota yang diketik user.
Memanggil GetData (HTTP Request Tool) untuk mengambil data cuaca dari API.
Setelah dapat data cuaca, AI Agent menganalisis dan menyusun laporan cuaca dalam bahasa Indonesia dengan format mudah dipahami + emoji.
GetData (HTTP Request Tool)
Node ini dipanggil oleh AI Agent untuk benar-benar mengakses API OpenWeather.
Data mentah cuaca (suhu, kelembaban, kondisi, angin, dsb.) dikirim kembali ke AI Agent.
AI Agent – Analisis Cuaca
Dari data API tadi, AI Agent membuat ringkasan:
Nama Kota
Suhu, Kelembaban, Tekanan Udara
Kondisi cuaca (cerah, hujan, berawan, dll)
Kecepatan & arah angin
Analisis singkat (nyaman, perlu waspada, dll)
Tambah emoji ☀️☔💨❄️ biar lebih jelas.
Send Telegram Message
Hasil analisis cuaca dikirim balik ke user melalui bot Telegram.
Jadi user langsung dapat jawaban berupa laporan cuaca yang rapi.
📌 KESIMPULAN
Workflow ini bekerja seperti asisten cuaca otomatis:

User: ketik nama kota ke bot Telegram.
Workflow: ambil data cuaca → analisis → balas ke user dengan laporan singkat
⚡ Jadi inti workflow: Telegram input → AI Agent + API cuaca → Telegram output.

Output Keadaan Cuaca


