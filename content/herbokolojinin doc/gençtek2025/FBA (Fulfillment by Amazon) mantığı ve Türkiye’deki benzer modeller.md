**FBA (Fulfillment by Amazon)** modeli, e-ticaret satıcılarının lojistik süreçlerini Amazon’un deposuna ve sistemine devrettiği bir hizmet modelidir. Bu modelin mantığı ve Türkiye’deki benzer sistemleriyle ilgili detaylı bir teknik analiz aşağıda sunulmuştur:

---

## 🔹 **FBA (Fulfillment by Amazon) Nedir?**

FBA, satıcıların ürünlerini Amazon'un lojistik merkezlerine gönderdiği, sipariş, paketleme, kargo ve iade süreçlerinin tamamen Amazon tarafından yürütüldüğü bir hizmet modelidir.

**Nasıl çalışır (adım adım):**

1. **Satıcı ürünleri Amazon deposuna gönderir.**
2. **Amazon, bu ürünleri kendi depolarında stoklar.**
3. **Müşteri siparişi verdiğinde,** Amazon:
   - Ürünü paketler,
   - Kargolar,
   - Müşteri hizmetleri ve iadeleri yönetir.
4. Satıcıya, satış tamamlandıktan sonra ödemesi yapılır (komisyon ve FBA ücretleri düşülerek).

---

## 🔹 **Türkiye’deki Benzer Modeller (FBA Alternatifleri)**

Türkiye'de Amazon kadar entegre olmasa da benzer hizmeti sunan **pazar yerleri** ve **3. parti lojistik firmaları** (3PL) bulunmaktadır:

### 1. **Trendyol FBO (Fulfillment by Trendyol)**

- Satıcılar ürünlerini Trendyol’un lojistik merkezine gönderir.
- Sipariş, kargo, iade ve müşteri hizmetleri Trendyol tarafından yönetilir.
- Trendyol’un hızlı teslimat sisteminden faydalanma.
- Prime benzeri avantajlar (görünürlük artışı, daha hızlı kargo).
- İade süreçleri kolaylaştırılmıştır.
- Depo ücretleri yüksek olabilir.
- Satıcı ürün üzerinde daha az kontrole sahip olur.

---

### 2. **Hepsiburada FBS (Fulfillment by Seller) & FHB (Fulfillment by Hepsiburada)**

- **FBS:** Satıcı kendi deposundan gönderir. (Klasik model)
- **FHB:** Hepsiburada deposuna ürün gönderilir, FBA’ye benzer yapı.

**FHB’nin FBA’ye benzer tarafları:**

- Ürünlerin stoklanması, kargolanması, iadesi Hepsiburada tarafından yapılır.
- Hızlı teslimat ve Hepsijet avantajları.
- Müşteri memnuniyet oranı yükselir.

---

### 3. **n11 ve ÇiçekSepeti Lojistik Modelleri**

- Bu platformlar kendi fulfillment modellerini henüz FBA kadar ileri düzeyde yaygınlaştırmamıştır.
- Ancak **entegrasyonlu kargo anlaşmaları** ve **stok takibi yazılımları** ile destek veriyorlar.
- parti lojistik çözümlerle (OPLOG, KolayDepo, Hubtic vb.) desteklenir.

---

## 🔹 **Türkiye’de 3. Parti Lojistik Şirketleri (3PL Alternatifleri)**

FBA modeli olmasa bile, bazı firmalar benzer hizmeti dış kaynak olarak sunar bu firmalar komisyon bazlı çalışır

|   |   |   |
|---|---|---|
|Firma Adı|Hizmetler|Açıklama|
|**OPLOG**|Depolama, sipariş karşılama, kargolama|E-ihracat destekli sistem, API ile entegrasyon|
|**KolayDepo**|Depo + fulfillment|E-ticaret altyapılarına uyumlu|
|**ShipEntegra**|Kargo ve fulfillment|Yurt dışına odaklı lojistik destek|
|**Hubtic**|Fulfillment + kargo|Avrupa’ya ihracata özel çözümler sunar|

---

- **Türkiye’deki sistemler FBA’nin mantığını taklit etmekte**, ancak **henüz aynı ölçek ve entegrasyon derinliğine ulaşamamıştır**.
- Trendyol FBO ve Hepsiburada FHB, FBA’nin Türkiye versiyonu olarak öne çıkıyor.
- E-ihracat yapacaklar için OPLOG gibi firmalar FBA’nin yerini tutabilir.
- Türkiye'de FBA benzeri yapıların ölçeklenmesi için daha fazla **otomasyon**, **müşteri hizmeti entegrasyonu** ve **uluslararası altyapı** gerekiyor.:/

---

## 1. **WMS (Warehouse Management System) – Depo Yönetim Sistemi**

### 🔧 Tanım:

Depo içerisindeki tüm stokların, rafların, hareketlerin, elleçleme süreçlerinin yönetimini sağlar.

Raf yerleştirme algoritmaları (FIFO, LIFO, FEFO vb.)

Barkod/QR kod entegrasyonu

Stok doğrulama ve sayım otomasyonu

Siparişe göre otomatik picking/packing talimatları

Java / .NET tabanlı back-end

React/Angular tabanlı dashboard'lar

Entegre veri tabanı: PostgreSQL, Oracle DB veya MongoDB

### Türkiye’de Kullanım:

Trendyol ve Hepsiburada, kendi iç geliştirilmiş WMS’lerini kullanır.

OPLOG, KolayDepo gibi 3PL şirketler genellikle özel lisanslı veya açık kaynaklı WMS çözümleri (Odoo, Zoho Inventory, NetSuite) ile çalışır.

---

## 🔹 2. **OMS (Order Management System) – Sipariş Yönetim Sistemi**

### 🔧 Tanım:

Farklı kanallardan gelen siparişleri birleştirir, sipariş akışını depo, kargo, fatura ve müşteri destek ile entegre yürütür.

- Çoklu kanal (multi-channel) sipariş toplama
- Sipariş durumu takibi ve API webhook'ları
- Otomatik fatura/irsaliye entegrasyonu
- Fraud kontrol modülleri
- Mikroservis mimarisi
- Kafka/RabbitMQ (sipariş olayı tetikleme)
- Restful API / GraphQL ile platform entegrasyonu
- Elasticsearch ile sipariş sorgulama

### ✅ Türkiye’de Kullanım:

- Trendyol: Satıcı Paneli + OMS API
- Hepsiburada: Kendi OMS altyapısı
- 3PL firmaları: Entegratör platformlar (Ticimax, İdeasoft, Entegra)

---

## 🔹 3. **TMS (Transportation Management System) – Taşıma Yönetimi**

Kargo süreçlerini planlayan, izleyen, teslimatları optimize eden sistemdir.

- HepsiJet: İç geliştirme TMS
- Trendyol Express: Navigasyon destekli iç sistem
- 3PL: Kargo firmaları ile API temelli TMS bağlantıları (Yurtiçi, Aras, MNG)

---

## 🔹 4. **ERP (Enterprise Resource Planning) – Kaynak Planlama**

### 🔧 Tanım:

Fulfillment operasyonlarında muhasebe, stok, satın alma ve insan kaynağı gibi yan süreçlerin koordinasyonunu sağlar.

### 🚀 Özellikleri:

- Satın alma talepleri
- Muhasebe entegrasyonu (e-fatura, e-irsaliye)
- Personel vardiya planlama
- Karlılık analizi

### 💡 Kullanılan ERP’ler:

- SAP Business One / S4HANA
- Logo Tiger Enterprise
- Netsis, Mikro (KOBİ ölçeğinde)

### ✅ Türkiye’de Kullanım:

- Büyük oyuncular (Trendyol, Hepsiburada): SAP ve Oracle ERP
- 3PL veya küçük pazar yerleri: Mikro, Logo, Nebim

---

## 🔹 5. **API Gateway & Entegrasyon Platformları**

### 🔧 Tanım:

Farklı sistemler (WMS, TMS, OMS, ERP) ve dış platformlar (kargo, e-fatura, pazar yerleri) arasında bağlantı sağlar.

### 💡 Kullanılan Teknolojiler:

- AWS API Gateway, Kong, Tyk
- OAuth 2.0 + JWT güvenlik protokolleri
- Swagger / OpenAPI dokümantasyonu
- Webhook tabanlı olay yönetimi

### ✅ Türkiye’de:

- Trendyol / Hepsiburada: REST API'ler ile satıcı entegrasyonu
- Entegra, StockMount gibi ara katman yazılımlar üzerinden API routing

---

## 🔹 6. **Veri Tabanı ve Gerçek Zamanlı İzleme Sistemleri**

- **DBMS:** PostgreSQL, MySQL, Oracle, MongoDB (stoklar, siparişler, kullanıcılar için)
- **Cache:** Redis, Memcached (yüksek trafikli sipariş sistemlerinde hızlı okuma için)
- **Stream:** Kafka, Apache Pulsar (sipariş akışı, kargo bildirimleri)
- **Dashboard:** Grafana, Kibana (operasyonel izleme)

---

## 🔹 Örnek Mimarî

```Mermaid
graph TD
    A[Kullanıcı Siparişi] --> B[OMS]
    B --> D[WMS Depodan çek]
    D --> T[Barkodlama]
    T --> J[Picking]
    J --> E[TMS Kargoya yönlendir]
    E --> F[API üzerinden Kargo Takip]
    F --> G[Müşteri & ERP bildirimi]
```

---

## 🔍 Değerlendirme ve Uyarı

Bu sistemlerin yüksek performansla çalışabilmesi için:

- **Yüksek erişilebilirlik (HA)** ve **failover** tasarımı olmalı.
- **Gerçek zamanlı loglama** ve **otomatik hata bildirimi (Sentry, Prometheus)** kritik.
- **Veri güvenliği**, özellikle müşteri ve ödeme bilgileri için, GDPR/KVKK uyumlu olmalı.
- Tüm sistemler **CI/CD pipeline** ile güncellenmeli (Jenkins, GitHub Actions, GitLab CI).

---

İstersen bu sistemlerin örnek bir mimari diyagramını çizebilir veya senin mobil uygulamana uyarlanabilir versiyonunu planlayabilirim. Uygulamanın hedef iş modeli nedir, dropshipping mi, depolu mu?
