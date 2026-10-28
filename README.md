# 🐾 Konum Tabanlı Evcil Hayvan Bakım, Topluluk ve Pazaryeri Veritabanı Mimarisi

> Fırat Üniversitesi · Veritabanı Laboratuvarı · Dönem Projesi

Evcil hayvan sahiplerini, bakıcıları ve veterinerleri tek bir ilişkisel veritabanı üzerinde buluşturan; konuma göre bakıcı/klinik eşleştirmesi yapan, topluluk etkileşimi ve basit bir ürün mağazası içeren bir veritabanı tasarımıdır.

**Projenin odağı:** İlişkisel veritabanı tasarımı ve SQL programlama. Web API ve frontend, veritabanı işlemlerinin uygulama üzerinden çalıştığını göstermek için hazırlanmış basit bir katmandır.

---

## 🎯 Proje Kapsamı

### Aktörler
- **Evcil hayvan sahibi:** hayvanını kaydeder, bakıcı arar, rezervasyon yapar, alışveriş yapar
- **Bakıcı:** hizmet ve müsaitlik tanımlar, rezervasyonları yönetir
- **Veteriner / klinik:** konumuyla listelenir, hizmet sunar
- **Admin:** kullanıcı ve içerik denetimi yapar, ürün ve stokları yönetir

### Modüller
1. **Kullanıcı ve Konum:** kullanıcılar, roller, adresler, coğrafi koordinatlar
2. **Evcil Hayvan:** hayvan profili, tür/ırk, aşı ve sağlık kayıtları
3. **Bakım Hizmeti ve Rezervasyon:** hizmetler, müsaitlik, rezervasyon, ödeme durumu
4. **Topluluk:** gönderiler, yorumlar, beğeniler, etkinlikler, değerlendirmeler
5. **Pazaryeri (basit mağaza):** evcil hayvan ürünleri (mama, kap, tasma, oyuncak vb.), kategoriler, stok, sipariş ve sipariş kalemleri. Ödeme yalnızca sipariş üzerinde bir durum alanıdır (`payment_status`); sepet, ayrı satıcı yönetimi ve gerçek ödeme entegrasyonu kapsam dışıdır.
6. **Mesajlaşma ve Bildirim** _(opsiyonel: çekirdek modüller tamamlanırsa eklenecek)_

### Konum tabanlı özellikler
- SQL Server `geography` veri tipi ve spatial index
- `STDistance()` ile "X km çevremdeki müsait bakıcılar / klinikler" sorguları
- Spatial index öncesi ve sonrası performans karşılaştırması (`/benchmarks`)

---

## ⭐ Ek Özellik (zaman kalırsa): Puan ve Sıralama Sistemi

Çekirdek gereksinimler tamamlandıktan sonra eklenecek, tamamen veritabanı ağırlıklı bir özelliktir. Amaç: hizmet sonrası verilen puanlara göre veteriner ve bakıcıların arama sonuçlarında üst sıralara çıkması.

- **Sahte puan engeli:** Yalnızca tamamlanmış bir rezervasyonu olan kullanıcı değerlendirme yapabilir (`reviews.booking_id`, bir rezervasyona bir yorum için `UNIQUE`)
- **Otomatik ortalama:** Yeni yorum eklenince trigger, veterinerin/bakıcının `avg_rating` ve `review_count` alanlarını günceller
- **Adil sıralama:** Ham ortalama yerine ağırlıklı ortalama kullanılır, böylece tek yorumlu 5 puan, çok yorumlu 4.8 puanın önüne geçmez:
  `skor = (yorum_sayısı × ortalama + 10 × genel_ortalama) / (yorum_sayısı + 10)`
- **Konum + puan birlikte:** "5 km içindeki en iyi veterinerler" gibi sorgular (yakınlık ve skor birleşik sıralama)
- **Sıralama sorguları:** Kategori bazında en iyi 5 gibi sonuçlar için `RANK()` / `ROW_NUMBER()` ve view

| Parça | Nerede? |
|---|---|
| `reviews` tablosu ve kısıtlar | `/sql/01_schema.sql` |
| Ortalama güncelleyen trigger | `/sql/04_triggers.sql` |
| Skor hesaplayan function | `/sql/03_functions_procedures.sql` |
| Sıralama view'ı (`RANK()`) | `/sql/05_views.sql` |
| Konum + puan sorgusu | `/sql/07_queries.sql` |

---

## ✅ Ödev Gereksinimleri Karşılama Tablosu

| Gereksinim | Durum | Nerede? |
|---|---|---|
| E-R diyagramı | ⬜ | `/docs/er-diagram` |
| Normalizasyon (en az 3NF) | ⬜ | `/docs/normalization.md` |
| Veri sözlüğü | ⬜ | `/docs/data-dictionary.md` |
| CREATE (tablo, kısıtlar) | ⬜ | `/sql/01_schema.sql` |
| INSERT / UPDATE / DELETE | ⬜ | `/sql/06_seed.sql`, `/sql/07_queries.sql` |
| SELECT (JOIN) | ⬜ | `/sql/07_queries.sql` |
| Basit olmayan alt sorgu (subquery) | ⬜ | `/sql/07_queries.sql` |
| View | ⬜ | `/sql/05_views.sql` |
| Function | ⬜ | `/sql/03_functions_procedures.sql` |
| Stored procedure | ⬜ | `/sql/03_functions_procedures.sql` |
| Trigger | ⬜ | `/sql/04_triggers.sql` |
| Transaction | ⬜ | `/sql/08_transactions.sql` |
| Index | ⬜ | `/sql/02_constraints_indexes.sql` |
| Yetkilendirme (rol / izin) | ⬜ | `/sql/09_roles.sql` |
| Spatial index benchmark | ⬜ | `/benchmarks` |
| Web API | ⬜ | `/api` |
| Basit frontend | ⬜ | `/frontend` |
| Ek: Puan ve sıralama sistemi _(opsiyonel)_ | ⬜ | `/sql` (bkz. Ek Özellik bölümü) |
| Aşama raporu | ⬜ | `/docs/reports` |

_(Tamamlanan satırlar ✅ ile güncellenecek.)_

---

## 🧰 Teknoloji

| Katman | Seçim |
|---|---|
| Veritabanı | Microsoft SQL Server 2022 Express + SSMS (T-SQL) |
| Web API | _(belirlenecek: ASP.NET Core / Node.js Express)_ |
| Frontend | _(belirlenecek: düz HTML-JS / React)_ |
| Çalıştırma | Docker Compose _(opsiyonel)_ |

---

## 📁 Repo Yapısı

```
/docs          → ER diyagramı, normalizasyon, veri sözlüğü, aşama raporları
/sql
  01_schema.sql
  02_constraints_indexes.sql
  03_functions_procedures.sql
  04_triggers.sql
  05_views.sql
  06_seed.sql
  07_queries.sql
  08_transactions.sql
  09_roles.sql
/benchmarks    → spatial index öncesi/sonrası ölçümler
/api           → Web API
/frontend      → basit arayüz
docker-compose.yml
README.md
```

---

## 🚀 Kurulum

> _Bu bölüm SQL ve API hazır olunca doldurulacak._

1. SQL Server 2022 Express ve SSMS kurulu olmalı
2. `/sql` altındaki dosyaları numara sırasıyla çalıştırın
3. API ve frontend için ilgili klasörlerdeki talimatlara bakın

---

## 🗓️ Takvim

| Aşama | Açıklama | Tarih | Durum |
|---|---|---|---|
| 1. Notlandırma | Proje grubu ve konu belirleme | 11 Ekim 23.59 | _(durum)_ |
| 2. Notlandırma | _(DEBSİS'e göre doldurulacak)_ | _(tarih)_ | ⬜ |
| 3. Notlandırma | _(DEBSİS'e göre doldurulacak)_ | _(tarih)_ | ⬜ |

### Dahili plan

| Hafta | Hedef |
|---|---|
| 1 | Ortak ER diyagramı, ortak tablolar (`users`, `addresses`), isimlendirme kuralı |
| 2 | Her üye kendi modülünün `CREATE` + kısıtlarını ve seed verisini yazar, normalizasyon bölümleri |
| 3 | Function, procedure, trigger, view, transaction, sorgular, spatial benchmark |
| 4 | API, frontend, rapor, README ve son kontrol |
| Zaman kalırsa | Puan ve sıralama sistemi (reviews, trigger, skor function, sıralama view'ı), ardından diğer ek fikirler |

---

## 🤝 Çalışma Kuralları

- `main` branch'ine doğrudan push yapılmaz
- Her görev bir **issue**, her issue bir **branch** ve **pull request** ile ilerler
- Branch adı: `feature/<kisi>-<konu>` (örn. `feature/ali-rezervasyon-trigger`)
- Her PR'ı ekipten en az bir kişi inceler (herkes her modüle hâkim olmalı)
- Commit mesajları anlamlı olmalı, düzenli commit atılmalı
- Rapor en sonda değil, her aşamada `/docs` içinde yazılır
- Tasarım özgündür; hazır şema, geçmiş yıl projesi veya internetten alınmış içerik kullanılmaz

### İsimlendirme kuralları
- Tablo ve sütun adları **İngilizce** ve `snake_case` olur (örn. `pet_vaccinations`, `created_at`)
- Tablo adları çoğul, birincil anahtar `id`, yabancı anahtar `<tablo_tekil>_id` (örn. `user_id`)
- SQL nesne önekleri: `fn_` (function), `sp_` (procedure), `trg_` (trigger), `vw_` (view), `ix_` (index)

---

## 🔐 Güvenlik ve Gizlilik Notları

- Parolalar düz metin saklanmaz, hash'lenir
- Konum ve telefon gibi kişisel veriler KVKK kapsamında ele alınır
- Soft delete (`is_deleted`) ve `created_at` / `updated_at` alanları kullanılır
- Rol bazlı yetkilendirme `/sql/09_roles.sql` içinde tanımlanır
