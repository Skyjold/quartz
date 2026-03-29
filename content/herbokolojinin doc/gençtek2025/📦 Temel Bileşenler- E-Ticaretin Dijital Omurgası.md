---
publish: true
created: 2025-05-04T22:38:32.916+03:00
modified: 2026-03-10T15:12:37.166+03:00
published: 2026-03-10T15:12:37.166+03:00
---

## 🎨 **Frontend (Kullanıcı Arayüzü)**

Kullanıcıların doğrudan etkileşime girdiği, sitenin görünen yüzüdür.

Burada hem tasarım hem hız hem de UX/UI prensipleri devreye girer.

### Neden Önemli?

- 3 saniyede yüklenmeyen sayfa terk edilir (%53 oranında).
- Mobil uyumlu olmayan site, Google’da sıralama kaybeder.
- Erişilebilirlik (a11y) eksikliği, kullanıcı deneyimini bozar.

### Yaygın Teknolojiler:

|   |   |   |
|---|---|---|
|Teknoloji|Ne İçin Kullanılır?|Avantaj|
|**React.js**|Dinamik bileşen tabanlı yapılar|SPA (Single Page App) için idealdir|
|**Next.js**|React tabanlı framework|SSR (Server Side Rendering) + SEO|
|**TailwindCSS**|Stil altyapısı|Utility-first, hızlı tasarım imkânı|
|**Framer Motion**|Animasyonlar|Etkileşimli, akıcı UI'lar|

### Kod Örneği (React + Tailwind):

```JavaScript
function ProductCard({ product }) {
  return (
    <div className="p-4 rounded-xl shadow-md hover:scale-105 transition">
      <img src={product.image} alt={product.name} className="rounded" />
      <h2 className="text-lg font-bold mt-2">{product.name}</h2>
      <p className="text-sm text-gray-500">{product.price}₺</p>
    </div>
  );
}
```

---

### ⚙️ **Backend (Sunucu Tarafı Uygulamalar)**

Frontend'e veri sağlayan, iş kurallarını yöneten kısımdır. Kullanıcı girişi, ürün stoğu kontrolü, ödeme akışı, sepet yönetimi gibi kritik işlevler burada çalışır.

### Backend Ne Yapar?

- API üretir (RESTful veya GraphQL)
- Yetkilendirme & Kimlik Doğrulama (JWT, OAuth)
- Sipariş & ürün yönetimi (CRUD operasyonları)
- Performans ve güvenlik önlemleri (Rate limiting, hashing)

### Popüler Teknolojiler:

|   |   |
|---|---|
|Teknoloji|Açıklama|
|**Node.js + Express**|JS tabanlı, hafif ve hızlı|
|**Django**|Python tabanlı, güvenli ve hızlı geliştirme|
|**Laravel**|PHP tabanlı, büyük projelerde güçlü yapı|
|**GraphQL**|REST’e alternatif esnek API sistemi|

### Örnek API Endpoint (Express.js):

```Plain
app.get('/api/products', async (req, res) => {
  const products = await Product.find();
  res.json(products);
});
```

---

## 🧠 **Veritabanı Sistemleri (Data Layer)**

E-ticaretin kalbi: ürünler, kullanıcılar, siparişler, stoklar burada saklanır. Tasarımı ne kadar temizse, performans o kadar yüksek olur.

### RDBMS vs NoSQL:

- **RDBMS** (MySQL, PostgreSQL): ilişkisel, ACID garantili, güvenli
- **NoSQL** (MongoDB, Firebase): esnek, hızlı, ölçeklenebilir

### Entity İlişki Şeması Örneği:

```Plain
[Users] ---< [Orders] >--- [Products]
   |                      |
[Addresses]         [OrderItems]
```

### Neden Normalizasyon?

- Veri tekrarını azaltır
- Tutarlılık sağlar
- Karmaşık sorgulara olanak verir

---

# 2. Entegrasyon Teknolojileri: Akıllı Otomasyonlar, Gerçek Zamanlı Hizmetler

E-ticaret sadece bir ürün yükleyip satışa çıkmak değildir. Siparişten kargoya, ödemeden stok güncellemesine kadar **çok sayıda sistem birbiriyle konuşmak zorundadır.** İşte bu noktada entegrasyon teknolojileri devreye girer.

---

## Ödeme Sistemleri Entegrasyonu

> "Kullanıcı ödeme yapamadıysa alışveriş deneyimi başlamamış demektir."

### Kullanılan Yapılar:

- **API Tabanlı Ödeme Geçitleri**

  → Stripe, PayPal, iyzico, PayTR, ParamPOS

  → 3D Secure, kart saklama, taksit seçenekleri

### Örnek İyziCo Entegrasyonu (Node.js):

```Plain
const iyzipay = new Iyzipay({...}); // auth bilgileri

const request = {
  locale: "tr",
  price: "250.00",
  paidPrice: "250.00",
  paymentCard: {
    cardHolderName: "BAHA BAHAR",
    cardNumber: "5528790000000008",
    expireMonth: "12",
    expireYear: "2030",
    cvc: "123"
  },
  ...
};

iyzipay.payment.create(request, (err, result) => {
  console.log(result.status); // success / failure
});
```

### Neden Önemli?

- **Terk edilmiş sepetlerin %70’i** ödeme ekranında yaşanır.
- Yerelleştirme: Türk kullanıcılar için iyzico, global için Stripe + PayPal tercih edilir.
- Ödeme hatası logları, dönüşüm oranlarını etkiler.

---

## 📦 Lojistik & Kargo Entegrasyonları

> “Siparişi almak kolay, teslim etmek sanattır.”

### Öne Çıkan API Sağlayıcılar:

- **Aras Kargo, Yurtiçi, MNG, PTT, Sürat API’leri**
- **Kargo takip otomasyonları** (Webhook + Tracking No)

### Otomasyon Faydaları:

- Otomatik kargo barkodu oluşturma
- Kargo durumuna göre müşteri bilgilendirme (SMS, mail)
- Depo yönetim sistemleriyle entegrasyon

### Kod Örneği: Kargo Takip Webhooku (Express.js)

```Plain
app.post('/webhook/kargo', (req, res) => {
  const status = req.body.status;
  const orderId = req.body.orderId;

  if (status === "DELIVERED") {
    Order.update({ id: orderId }, { status: "Teslim Edildi" });
  }

  res.sendStatus(200);
});
```

---

### 🔁 Otomasyon & No-Code Araçlar (kur, Unut)

### Entegre Edilebilecek Platformlar:

|   |   |
|---|---|
|Platform|Ne İşe Yarar?|
|**Zapier / Make.com**|Eylem tabanlı otomasyon: "Yeni sipariş → Google Sheets'e yaz"|
|**Pabbly**|Fiyat/özellik bakımından Zapier alternatifi|
|**IFTTT**|Basit koşul bazlı entegrasyonlar|
|**Shopify Flow** (Shopify özel)|Düşük kodlu workflow sistemi|

### Otomasyon Örnekleri:

- Yeni siparişte WhatsApp bildirim gönder
- Stok azaldığında otomatik uyarı maili at
- Müşteri satın aldıktan 3 gün sonra review linki gönder

---

### 🎯 ERP & CRM Entegrasyonu

> “Stok kontrolü olmayan e-ticaret, batmaya mahkûmdur.”

- **ERP**: Stok, fatura, üretim takibi (Logo, Nebim, Mikro)
- **CRM**: Müşteri geçmişi, kampanya takibi (Zoho, HubSpot, Salesforce)

### API Entegrasyon Gereksinimleri:

- Token bazlı kimlik doğrulama
- XML veya JSON veri formatı
- Webhook & event listener desteği

---

Tüm bu entegrasyonlar bir araya geldiğinde;

🚀 Sipariş → Ödeme → Stok düşüşü → Fatura kesimi → Kargo → Müşteri takibi

süreçleri neredeyse **manuel dokunmadan** işler.

---

# 🧱 3. Performans & Güvenlik Katmanı: Hızlı, Güvenli ve Dayanıklı Altyapı Tasarımı

> “Bir sistem ancak en yavaş bileşeni kadar hızlı, en zayıf halkası kadar güvenlidir.”

E-ticarette kullanıcı deneyimi, sadece güzel arayüzle değil; **gecikme süresi, uptime oranı ve siber saldırılara karşı alınan önlemlerle** ölçülür.

---

## ⚡ Performans Optimizasyonu

### 1️⃣ CDN (Content Delivery Network) Kullanımı

📌 Amaç: Statik dosyaları (resimler, JS/CSS dosyaları) coğrafi olarak kullanıcılara en yakın sunucudan iletmek.

**Popüler CDN Servisleri:**

- Cloudflare (aynı zamanda güvenlik katmanı da sağlar)
- Amazon CloudFront
- Vercel (otomatik CDN dağıtımı yapar)

### CDN Ne Sağlar?

- İlk yükleme süresini azaltır (TTFB düşer)
- DDoS ataklarına karşı buffer görevi görür
- Cache-Control başlıkları ile sayfa hızı artar

---

### 2️⃣ Lazy Loading & Image Optimization

📌 Amaç: Sayfa ilk açıldığında tüm görselleri yüklemek yerine sadece görünür alandakileri yüklemek

```HTML
<img src="ürün.jpg" loading="lazy" alt="ürün görseli" />
```

- **WebP** formatı kullan: JPEG’e göre %30–50 daha az boyut
- **Next.js**: `next/image` bileşeni otomatik optimizasyon sağlar

---

### 3️⃣ Frontend Performance Teknikleri

- JS bundling & tree-shaking (örn. Vite, esbuild)
- Critical CSS yükleme → Render Blocking önlenir
- Font preloading: `rel="preload"` ile kullanıcı bekletilmez
- HTTP/2 ile paralel dosya aktarımı

---

### 4️⃣ Backend Performans Teknikleri

- **Database Indexing**

  → `WHERE` sorguları için `BTREE`, `GIN`, `HASH` index kullan

- **Redis Caching**

  → En çok çekilen ürünler/sayfalar için bellek içi veri saklama

- **Asenkron Görevler (Queue)**

  → Sipariş sonrası mail gönderimi gibi görevler `Bull`, `Celery`, `RabbitMQ` ile arka plana atılır

```Plain
// Bull (Node.js)
orderQueue.add({ orderId: 123 }); // Mail tetikleme gibi işler için
```

---

## 🔐 Güvenlik Katmanı

### 1️⃣ HTTPS & SSL Zorunluluğu

- Her API endpoint’i TLS ile şifrelenmeli
- Let’s Encrypt ile ücretsiz SSL alınabilir
- HSTS (HTTP Strict Transport Security) ile downgrade saldırılarına karşı koruma

---

### 2️⃣ WAF (Web Application Firewall)

📌 Ne İşe Yarar?

- SQL Injection, XSS, CSRF gibi saldırılara karşı filtre görevi görür
- Rate limiting sağlar (botlara karşı koruma)

**Kullanılabilecek WAF Sistemleri:**

- Cloudflare WAF
- AWS WAF
- Nginx ModSecurity

---

### 3️⃣ Kimlik Doğrulama & Yetkilendirme

- **JWT (JSON Web Token):** Kullanıcı oturumları için modern yapı
- **OAuth 2.0:** Üçüncü taraf uygulama izinleri
- **Two-Factor Authentication (2FA):** Giriş güvenliğini artırır

### JWT Token Signature Örneği:

```Plain
const token = jwt.sign({ userId: 42 }, 'superSecret', { expiresIn: '2h' });
```

---

### 4️⃣ Güvenlik Açığı Taramaları

- OWASP ZAP (otomatik zafiyet tarayıcı)
- Snyk / SonarQube (kod taraması)
- Dependabot (npm güvenlik güncellemeleri için)

---

### 5️⃣ Loglama ve İzleme

- Uygulama hatalarını yakalamak için: Sentry, LogRocket
- Performans izleme: New Relic, Datadog
- Audit log (günlük): Kullanıcı hareketlerinin kaydı (KVKK uyumu için önemli)

---

## 📈 SLA ve Uptime Stratejileri

> "99.9% uptime = yılda 8 saat kesinti demek!"

- Load Balancer kullan (NGINX, HAProxy, AWS ELB)
- Health check endpoint'leri oluştur
- Failover sistemleri kur (backup DB, mirrored server)

---

## 📊 4. Veri Analitiği & Kullanıcı Davranış Takibi

> “Veri toplamayan e-ticaretçi kördür; veriyi anlamayan da aynı çukura düşer.”

Bir kullanıcı sitene girdiğinde, tıkladığı her yer, kaldığı her saniye, sepetten çıkışı bile **birer sinyaldir**.

Bu sinyalleri toplamak ve anlamlı aksiyonlara dönüştürmek için gelişmiş analitik sistemler kurmak şarttır.

---

### 🧠 Web Analitiği: Temelden Gelişmişe

### Google Analytics 4 (GA4)

- **Event tabanlı analiz:** Tıklama, sayfa görüntüleme, scroll derinliği, video izleme gibi eylemleri otomatik izler.
- **User ID:** Giriş yapan kullanıcıların cihazlar arası takibi
- **Realtime Dashboard:** Canlı kullanıcı, bölge, cihaz bilgisi

```Plain
gtag('event', 'purchase', {
  value: 100.0,
  currency: 'TRY',
  transaction_id: 'BAHA_0423'
});
```

### Setup İpuçları:

- GA4 property oluştur → `gtag.js` kodu içine ekle
- Google Tag Manager ile birlikte kullan (event tanımlamaları için low-code çözüm)

---

### 🔥 Kullanıcı Isı Haritası & Davranış Görselleştirme

### Araçlar:

|   |   |
|---|---|
|Araç|Ne Sağlar?|
|**Hotjar**|Mouse hareketi, tıklama haritası, anket|
|**Microsoft Clarity**|Ücretsiz ısı haritası + oturum kaydı|
|**FullStory**|Gelişmiş UX analizi, form takibi|

### Kullanım:

- "Neden alışveriş tamamlanmıyor?" sorusuna görsel yanıt verir
- CTA düğmelerine tıklama oranlarını analiz eder
- Mobilde mi masaüstünde mi davranış farklı?

---

### 🧾 Satın Alma ve Dönüşüm Hunileri

### Google Analytics Funnel / E-Commerce Tracking

- **Add to Cart → Checkout → Purchase** adımlarını takip et
- **Drop-off analysis:** Hangi adımda kullanıcı çıkıyor?
- **Segment bazlı takip:** Mobil kullanıcılar mı daha çok terk ediyor?

### Segmentasyon Örnekleri:

- **Cihaz:** Android kullanıcıları neden sepeti terk ediyor?
- **Saat dilimi:** Gece saatlerinde neden daha fazla dönüşüm oluyor?
- **Kampanya bazlı:** Instagram reklamından gelenler ne yapıyor?

---

### 🛠️ Özel Dashboard Kurulumu (Data Studio / Looker)

- GA4 + BigQuery ile tam veri erişimi sağlar
- Dashboard → Satışlar, trafik kaynakları, en çok görüntülenen ürünler
- KPI panosu: Günlük/aylık gelir, dönüşüm oranı, terk oranı

```SQL
SELECT
  user_pseudo_id,
  event_name,
  event_timestamp
FROM
  `my-project.analytics_123456.events_*`
WHERE
  event_name = 'add_to_cart'
```

---

### 📌 Kampanya Takibi & UTM Yapıları

> "Reklama para harcıyorsan, nereye gittiğini bilmek zorundasın."

### UTM Parametreleri:

```Plain
https://example.com?utm_source=instagram&utm_medium=cpc&utm_campaign=ramazan_indirimi
```

→ Kaynak, mecra ve kampanya takibi yapılır

→ GA4’te `source / medium / campaign` alanlarında görünür

---

### 🧠 BI (Business Intelligence) Araçları

> Gelişmiş e-ticaret operasyonlarında sadece frontend verisi yetmez.

### Tercih Edilen BI Araçları:

- **Metabase** – Açık kaynak, self-host edilebilir
- **Power BI** – Microsoft ekosistemiyle güçlü uyum
- **Tableau** – Görsel analizde dünya standardı
- **Superset** – SQL bilenler için Google Sheets yerine alternatif

### Ne için kullanılır?

- Ürün bazlı ROI analizi
- Satıcı performans raporu (Marketplace için)
- Dinamik stok raporu

---

### Veri Entegrasyon Araçları

- Segment.com: Tüm kullanıcı aktivitelerini tek API ile toplar, farklı platformlara dağıtır

ideal değil lakin iş yapar

- **Fivetran / Airbyte**: Veriyi Stripe, Shopify, PostgreSQL gibi kaynaklardan alır, BigQuery gibi depolara aktarır

fiyatlandırması bizim gibi bireyselleri için kötüdür

---

### A/B Test ve Optimize Süreçleri

### A/B Test Araçları:

- Google Optimize (GA4 ile entegre çalışır – ancak kapatıldı, yerini [VWO](https://vwo.com/) ve [Optimizely](https://www.optimizely.com/) aldı)
- VWO – Görsel editörle hızlı varyant testleri
- Optimizely – Gelişmiş hedefleme + hız analizi

### Ne Test Edilir?

- Ana sayfa görseli
- Sepet butonu rengi / konumu
- Ürün sıralama algoritması
- Fiyat psikolojisi (99,90₺ mi 100₺ mi?)

temel şeyler gibi gözüksede bilinen gerçeklerde kullanıcıların daha grı kutucukların değiştirilebilir olduğunu bilmeyen bireylerde aramızda ..

---

### Veri Gizliliği & KVKK/GPDR Uyumlu Analitik

- **Anonimleştirilmiş IP (IP masking)** kullan
- Cookie banner & açık rıza sistemi zorunlu
- Google Consent Mode entegrasyonu
- "Data subject request" endpointi oluştur (kullanıcı verisini silebilmeli)

## 1. **E-Ticaret Temel Yapısı**

[[herbokolojinin doc/gençtek2025/📦 Temel Bileşenler- E-Ticaretin Dijital Omurgası]]

[[herbokolojinin doc/gençtek2025/E-Ticaret sitesi altyapısı- Shopify, WooCommerce, Magento gibi platformlar]]

[[herbokolojinin doc/gençtek2025/Ürün listeleme, stok yönetimi, ödeme entegrasyonu İyzico, PayTR]]

[[Sepet ve ödeme süreci optimizasyonu (Conversion Rate Optimization)]]

## 2. **Pazaryeri Entegrasyonları**

[[herbokolojinin doc/gençtek2025/Trendyol, Hepsiburada, Amazon Türkiye, ÇiçekSepeti gibi yerel pazar yerlerini baz alan B2B mimarisi]]

[[herbokolojinin doc/gençtek2025/API bağlantıları ile ürün ve sipariş senkronizasyonu]]

[[herbokolojinin doc/gençtek2025/FBA (Fulfillment by Amazon) mantığı ve Türkiye’deki benzer modeller]]

## 3. **E-İhracat Temelleri**

[[herbokolojinin doc/gençtek2025/Mikro ihracat kavramı (ETGB — Elektronik Ticaret Gümrük Beyannamesi)]]

[[Yurt dışı ödeme sistemleri (PayPal, Wise, Stripe)]]

[[Yurt dışına kargo ve lojistik firmaları (UPS, DHL, PTT ETGB)]]

[[herbokolojinin doc/gençtek2025/İhracat teşvikleri ve devlet destekleri (Turquality, Eximbank kredileri)]]

## 4. **Global Pazaryerleri ve Satış Stratejileri**

[[Amazon Global, Etsy, eBay, Aliexpress üzerinde mağaza açmak]]

[[DDP (Delivery Duty Paid) ve DDU (Delivery Duty Unpaid) teslim şekilleri]]

[[Dropshipping ve Print-on-Demand modelleri]]

## 5. **Dijital Pazarlama & Reklamcılık**

[[Facebook Ads, Instagram Ads, Google Shopping kullanımı]]

[[herbokolojinin doc/gençtek2025/SEO (Arama Motoru Optimizasyonu) - Ürün açıklamalarını optimize etmek]]

[[herbokolojinin doc/gençtek2025/📦 Temel Bileşenler- E-Ticaretin Dijital Omurgası]]

[[Influencer Marketing ile e-ihracat desteklemek]]

[[herbokolojinin doc/gençtek2025/6. Yasal Mevzuatlar ve Riskler]]

[[Slayt Takibi]]

server sistemi için kişisel sayfa blog teknoloji yorumları devamlılıgı için

### **Başlıkta Kaçınılması Gereken Hatalar**

- **Spam Tetikleyiciler:** “Ücretsiz”, “Sınırlı Süre” gibi spam filtrelerini tetikleyebilecek kelimelerden kaçının. Bu tür kelimeler, e-postanızın spam klasörüne düşme riskini artırır.
- **Abartılı İfadeler:** Gerçekçi olmayan veya abartılı ifadeler, güvenilirliği azaltabilir. E-postanızın içeriği başlıkla uyumlu olmalı ve okuyucunun beklentilerini karşılamalıdır.
- **Çok Genel Başlıklar:** Çok genel veya belirsiz başlıklar, okuyucunun ilgisini çekmeyebilir. “Yeni Haberler” gibi belirsiz başlıklar yerine, daha spesifik ve ilgi çekici başlıklar kullanın.

### **rikte Kaçınılması Gereken Hatalar**

- **Çok Fazla Bilgi:** E-postanızda çok fazla bilgi vermekten kaçının. Kullanıcılar e-posta içeriklerinde hızlı ve kolay anlaşılır bilgiler arar. Bilgiyi basit tutun ve sadece en önemli noktaları vurgulayın.
- **Karmaşık Dil:** E-posta içeriğinizde karmaşık veya teknik dil kullanmaktan kaçının. Dilinizi sade ve anlaşılır tutun, böylece tüm okuyucular içeriğinizi kolayca anlayabilir.
- **Uzun Paragraflar:** Uzun ve sıkıcı paragraflar, okuyucuların dikkatini dağıtabilir. Paragrafları kısa tutun ve içeriği alt başlıklar, listeler veya madde işaretleri ile bölün.

**Çağrı-to-Aksiyon (CTA) Kullanımı**

İçeriğinizin en önemli bölümü, okuyucunun hangi adımı\
atmasını istediğinizi belirten CTA'dır. Net ve belirgin bir CTA, okuyucunun\
harekete geçmesini sağlar. CTA'nızı içeriğinizin sonunda veya en dikkat çeken\
noktada yerleştirin. Örneğin, "Şimdi Satın Al", "Detayları\
Gör" veya "İndirimi Kaçırma" gibi CTA ifadeleri kullanabilirsiniz.

## 📬 **1. E-Posta Pazarlama Araçları** (Sistem / Uygulama Katmanı)

Bu araçlar, e-posta kampanyalarını oluşturma, gönderme, kişiselleştirme ve analiz etme işlevlerini sağlar.

|   |   |   |
|---|---|---|
|Araç|Açıklama|Kullanım Senaryosu|
|**Mailchimp**|En popüler e-posta pazarlama aracı. Sürükle-bırak şablonlar, otomasyon, segmentasyon ve A/B test sunar.|B2C ve küçük-orta ölçekli B2B pazarlama|
|**Sendinblue**|SMS ve e-posta kampanyalarını tek panelde yönetir. Uygun fiyatlı, GDPR uyumlu.|Avrupa pazarında B2B/B2C firmalar|
|**MailerLite**|Sade, hızlı arayüz ve otomasyon yetenekleri. Ekonomik bir alternatiftir.|Başlangıç seviyesinde markalar|
|**Customer.io**|Davranış tabanlı otomasyon sistemiyle gelişmiş kullanıcı segmentasyonu sağlar.|Web uygulamaları ve SaaS sistemler|
|**Omnisend**|Özellikle e-ticaret sistemleriyle uyumlu. Shopify, WooCommerce ile entegre çalışır.|E-ticaret tabanlı kampanya yönetimi|

---

## 🔌 **2. Entegrasyonlar** (Veri Katmanı)

Bu araçların etkili çalışabilmesi için, sistemine (örneğin e-ticaret platformuna veya CRM'e) entegre olması gerekir. En yaygın entegrasyon türleri:

### a. **E-Ticaret Entegrasyonları**

|   |   |   |
|---|---|---|
|Sistem|Ne Sağlar?|Destekleyen Araçlar|
|**Shopify**|Sepet verisi, müşteri alışveriş davranışı, sipariş geçmişi gibi verileri alır.|Mailchimp, Klaviyo, Omnisend|
|**WooCommerce**|WordPress tabanlı siteler için kampanya entegrasyonu sağlar.|Mailchimp, MailerLite|
|**Magento**|Büyük ölçekli mağazalarda kullanılır. Kapsamlı müşteri verisi sağlar.|Dotdigital, Mailchimp|

### b. **CRM Entegrasyonları**

|   |   |   |
|---|---|---|
|Sistem|Ne Sağlar?|Açıklama|
|**HubSpot CRM**|Kişi listeleri, davranışsal veri, kampanya takibi|Pazarlama ve satış birimleri arasında entegrasyon sağlar|
|**Zoho CRM**|Segmentasyon, lead nurturing, satış otomasyonu|Uygun fiyatlı, modüler bir yapı sunar|
|**Salesforce**|Kurumsal pazarlama altyapısı|Yüksek maliyetli ama çok güçlü entegrasyonlar|

---

## ⚙️ **3. Otomasyon & Davranışsal E-Posta Sistemleri**

Bu sistemler, müşteri davranışına (ör. bir linke tıklama, sepete ürün ekleme, satın alma vs.) göre otomatik e-posta tetikler.

### Kullanılabilir Otomasyon Senaryoları:

|   |   |   |
|---|---|---|
|Otomasyon Türü|Açıklama|Araçlar|
|**Hoş geldin serisi**|Üye olan kullanıcıya ilk 3 gün boyunca otomatik e-postalar gönderilir|Mailchimp, Sendinblue|
|**Sepet hatırlatma**|Ürün sepete eklendi ama ödeme tamamlanmadıysa 1-3 gün sonra otomatik mail|Omnisend, Klaviyo|
|**Satın alma sonrası**|Siparişten 7 gün sonra yorum bırakma veya yeniden satın alma e-postası|Customer.io|
|**Segment bazlı kampanya**|“Sadece yüksek harcama yapanlar” segmentine özel kampanya gönderimi|HubSpot, Mailchimp|

---

## 📊 **4. Analitik ve İzleme Araçları**

Etki ölçümü ve strateji revizyonu için e-posta kampanyalarının detaylı analizine ihtiyaç vardır.

|   |   |   |
|---|---|---|
|Araç|Sağladığı Veri|Uyumlu Sistemler|
|**Google Analytics (GA4)**|E-postadan gelen kullanıcılar, davranış akışı, dönüşüm oranı|Tüm e-posta araçlarıyla UTM etiketleme üzerinden|
|**Mailchimp Reports**|Açılma, tıklanma, hemen çıkma oranı gibi detaylar|Mailchimp kullanıcıları|
|**Hotjar + Email combo**|E-posta ile gelen kullanıcıların site içi hareketleri (ısı haritası)|Web uygulamaları ve mağazalar için|

---

## 📦 E-ticaret Örneği: Entegrasyon Haritası (Basitleştirilmiş)

```Mermaid
graph TD
A[Shopify Mağaza] --> B[Mailchimp]
A --> C[Google Analytics]
A --> D[Customer.io]
B --> E[Segment Bazlı Gönderim]
D --> F[Sepet Hatırlatma Otomasyonu]
B --> G[Kampanya Raporlama]
```

---

## ✅ Hangi Araçları Ne Zaman Seçmelisin?

|   |   |
|---|---|
|İhtiyacın|Önerilen Araçlar|
|Başlangıç seviyesinde e-posta pazarlama|Mailchimp + WooCommerce|
|Yüksek hacimli e-ticaret kampanyaları|Omnisend veya Klaviyo + Shopify|
|ERP/CRM ile tam entegre bir yapı|HubSpot + Zoho CRM + Sendinblue|
|Türkçe destekli, SMS ile birlikte|Related Digital, Euromessage (yerli)|

---
