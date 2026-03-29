## 1️⃣ Ürün Listeleme Sistemleri

### 🔧 Teknik Temel:

- Veritabanında ürünler genellikle `products` tablosunda tutulur.
- Ürün varyantları (örneğin renk, boyut) için ayrı `product_variants` tablosu kullanılır.
- Kategori sistemi için `categories` ve `product_category_relation` (çoklu kategori desteği için) tabloları gerekir.

### 🧩 İşlevsel Gereksinimler (B2B’ye özel):

|   |   |
|---|---|
|Özellik|Açıklama|
|Varyant desteği|Renk, ebat, voltaj gibi özelliklerle birlikte çoklu ürün listelenmesi|
|Toplu yükleme|Excel / CSV üzerinden ürün, varyant, fiyat bilgileriyle yükleme|
|Fiyat grupları|Her müşteri grubu için farklı fiyat (örn. bayi, distribütör, son kullanıcı)|
|Gizli kataloglar|Bazı ürünlerin sadece belirli müşteri tiplerine gösterilmesi|
|Teknik dökümanlar|Ürünle birlikte PDF, CAD dosyası gibi döküman yüklenebilme|
|API üzerinden ürün çağırma|ERP veya PIM sisteminden otomatik ürün aktarımı|

---

## 2️⃣ Stok Yönetimi

### 🔧 Altyapı:

- Temel olarak ürün tablosunda `stock_quantity` gibi bir alanla yönetilebilir.
- Ama gelişmiş sistemlerde her varyant için ayrı stok tutulur: `product_variant_stock`.
- Eğer depo / lokasyon farkı varsa: `stock_locations` + `product_stock_location` ilişkileri kurulur.

### 🧩 Gelişmiş B2B Özellikler:

|   |   |
|---|---|
|Özellik|Açıklama|
|Gerçek zamanlı stok takibi|ERP ile entegrasyon varsa, anlık stok bilgisi API ile alınır|
|Minimum sipariş adedi|Örneğin “minimum 10 adet sipariş verilebilir” kuralı tanımlanabilir|
|Rezervasyon|Sipariş verilmiş ama onay bekleyen stok, sistemde rezerve tutulur|
|Çoklu depo yönetimi|İstanbul ve İzmir depolarında ayrı ayrı stok takibi yapılabilir|
|Tedarik zinciri izleme|Ürün stokta yoksa ne zaman geleceği tahmini gösterilir|
|Otomatik stok senkronizasyonu|ERP/Muhasebe sisteminden API ile veri çekme (örneğin Mikro, Logo)|

---

## 3️⃣ Ödeme Entegrasyonu (İyzico & PayTR)

### 🎯 Amaç:

B2B yapıda ödeme sistemleri çoğunlukla **vadeli / kapalı ödeme sistemleri** olsa da bazı müşterilere özel olarak **kredi kartı ile ödeme** seçeneği tanınır. İşte burada İyzico & PayTR gibi servisler devreye girer.

---

### 🔌 Teknik Entegrasyon

### a. **İyzico**

- REST API ile çalışır. Başlıca uç noktalar:
  - `initializePaymentRequest`: Ödeme formu başlatılır.
  - `retrievePayment`: Ödeme sonucu alınır.
  - `callback_url`: Ödeme sonucu sonrası yönlendirme yapılır.
- Frontend: JavaScript üzerinden **iframe ile ödeme formu** açılır.
- Backend: Node.js, PHP, Python gibi dillerle kolayca entegre edilebilir.

### b. **PayTR**

- iframe veya popup modda gömülü ödeme formları desteklenir.
- SHA-256 tabanlı güvenlik token'ı gerekir.
- `merchant_id`, `merchant_key`, `merchant_salt` kullanılarak hash oluşturulur.
- Geliştirici belgeleri açık ve Türkçe desteklidir.

---

### 🧩 B2B’ye Özel Ödeme Senaryoları

|   |   |
|---|---|
|Senaryo|Çözüm|
|Kapalı ödeme (vadeli)|Sistem içi ödeme “onaya” düşer, İyzico’ya yönlendirme yapılmaz|
|Kredi kartı ile ödeme|İyzico veya PayTR iframe’i üzerinden canlı tahsilat|
|Ön ödeme / depozito|Siparişin %30’u online, kalanı manuel/faturalı takip yapılabilir|
|Otomatik fatura oluşturma|Ödeme sonrası sistemde e-fatura API’si tetiklenebilir|
|Müşteriye özel POS oranı|Ödeme ekranında bayi bazlı POS komisyonu yansıtılabilir|

---

## 🎯 Örnek Sistem Akışı: Sipariş ve Ödeme

```Mermaid
graph TD
A[Müşteri Ürünü Seçer] --> B[Sipariş Sepete Eklenir]
B --> C{Stok Yeterli mi?}
C -- false --> D[Sistem Bekleyen Sipariş Olarak Kaydeder]
C -- true --> E[İyzico / PayTR Ödeme Ekranı Açılır]
E --> F[Ödeme Onayı Alınır]
F --> G[Sipariş ERP'ye Gönderilir]
G --> H[Fatura Kesilir ve Kargo Süreci Başlar]
```
