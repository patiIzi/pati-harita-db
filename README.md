# 🐾 Konum Tabanlı Evcil Hayvan Bakım, Topluluk ve Pazaryeri Veritabanı Mimarisi

> Fırat Üniversitesi · Veritabanı Laboratuvarı · Dönem Projesi

Evcil hayvan sahiplerini, bakıcıları, veterinerleri ve satıcıları tek bir ilişkisel veritabanı üzerinde buluşturan; konuma göre bakıcı/klinik eşleştirmesi yapan, topluluk etkileşimi ve pazaryeri içeren bir veritabanı tasarımıdır.

**Projenin odağı:** İlişkisel veritabanı tasarımı ve SQL programlama. Web API ve frontend, veritabanı işlemlerinin uygulama üzerinden çalıştığını göstermek için hazırlanmış basit bir katmandır.

---

## 🎯 Proje Kapsamı

### Aktörler
- **Evcil hayvan sahibi:** hayvanını kaydeder, bakıcı arar, rezervasyon yapar, alışveriş yapar
- **Bakıcı:** hizmet ve müsaitlik tanımlar, rezervasyonları yönetir
- **Veteriner / klinik:** konumuyla listelenir, hizmet sunar
- **Satıcı:** ürün ekler, stok ve siparişleri yönetir
- **Admin:** kullanıcı ve içerik denetimi yapar

### Modüller
1. **Kullanıcı ve Konum:** kullanıcılar, roller, adresler, coğrafi koordinatlar
2. **Evcil Hayvan:** hayvan profili, tür/ırk, aşı ve sağlık kayıtları
3. **Bakım Hizmeti ve Rezervasyon:** hizmetler, müsaitlik, rezervasyon, ödeme durumu
4. **Topluluk:** gönderiler, yorumlar, beğeniler, etkinlikler, değerlendirmeler
5. **Pazaryeri:** kategoriler, ürünler, stok, sepet, sipariş, ödeme
6. **Mesajlaşma ve Bildirim** _(kapsama göre eklenecek)_

### Konum tabanlı özellikler
- SQL Server `geography` veri tipi ve spatial index
- `STDistance()` ile "X km çevremdeki müsait bakıcılar / klinikler" sorguları
- Spatial index öncesi ve sonrası performans karşılaştırması (`/benchmarks`)

---

## ✅ Ödev Gereksinimleri Karşılama Tablosu

| Gereksinim | Durum | Nerede? |
|---|---|---|
| E-R diyagramı | ⬜ | `/docs/er-diagram` |
| Normalizasyon (en az 3NF) | ⬜ | `/docs/normalization.md` |
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
| Web API | ⬜ | `/api` |
| Basit frontend | ⬜ | `/frontend` |
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

---

## 🤝 Çalışma Kuralları

- Her görev bir **issue**, her issue bir **branch** ve **pull request** ile ilerler
- Her PR'ı ekipten en az bir kişi inceler (herkes her modüle hâkim olmalı)
- Commit mesajları anlamlı olmalı, düzenli commit atılmalı
- Rapor en sonda değil, her aşamada `/docs` içinde yazılır
- Tasarım özgündür; hazır şema, geçmiş yıl projesi veya internetten alınmış içerik kullanılmaz

---

## 🔐 Güvenlik ve Gizlilik Notları

- Parolalar düz metin saklanmaz, hash'lenir
- Konum ve telefon gibi kişisel veriler KVKK kapsamında ele alınır
- Soft delete (`is_deleted`) ve `created_at` / `updated_at` alanları kullanılır
- Rol bazlı yetkilendirme `/sql/09_roles.sql` içinde tanımlanır
