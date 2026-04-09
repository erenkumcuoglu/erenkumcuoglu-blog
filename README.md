# erenkumcuoglu.com/blog/ — Blog

263 blog yazısı (2007–2011 arşivi) + Hugo + Decap CMS + Netlify.
SEO için subdomain değil, alt dizin (/blog/) yaklaşımı kullanılıyor.

## Hızlı Kurulum

### Adım 1: Blog repo'sunu GitHub'a yükle
1. Bu klasörün İÇİNDEKİ dosyaları (klasörün kendisini değil) GitHub'a yeni repo olarak yükle
2. Repo adı: `erenkumcuoglu-blog`

### Adım 2: Blog'u Netlify'a deploy et
1. [app.netlify.com](https://app.netlify.com) → "Add new site" → "Import an existing project"
2. GitHub'dan `erenkumcuoglu-blog` repo'sunu seç
3. Build ayarları otomatik algılanacak:
   - Build command: `hugo --minify`
   - Publish directory: `public`
4. "Deploy site" tıkla
5. Netlify sana bir URL verecek (mesela `silly-name-123.netlify.app`) — not al

### Adım 3: Ana siteden /blog/ yönlendirmesi (PROXY)
Ana sitenin (erenkumcuoglu.com) repo'sundaki `netlify.toml` dosyasına
şu kuralı ekle (dosyanın EN BAŞINA — sıralama önemli):

```toml
# Blog proxy — /blog/ isteklerini blog sitesine yönlendir
[[redirects]]
  from = "/blog/*"
  to = "https://BLOG-SITE-ADI.netlify.app/blog/:splat"
  status = 200
  force = true
```

`BLOG-SITE-ADI` kısmını Adım 2'de aldığın Netlify URL'iyle değiştir.

ÖNEMLİ: `status = 200` bu kuralı "rewrite" yapar (redirect değil).
Yani kullanıcı ve Google sadece `erenkumcuoglu.com/blog/` görür.
Arka planda Netlify blog sitesinden içeriği çeker.

### Adım 4: Decap CMS editörünü aktif et
1. Blog sitesinin Netlify dashboard'unda "Integrations" → Identity → Enable
2. Registration: "Invite only" seç
3. Kendini davet et (email gir)
4. Email'deki daveti kabul et, şifre belirle
5. "Integrations" → Git Gateway → Enable
6. `erenkumcuoglu.com/blog/admin/` adresinden editöre giriş yap

### Adım 5 (opsiyonel): Ana sitedeki blog linkini güncelle
Ana sitenin HTML'inde navbardaki blog linkini güncelle:
```html
<a href="/blog/">Blog</a>
```

## Yeni Yazı Nasıl Eklenir?

### Yöntem 1: Decap CMS (Tarayıcıdan — WordPress rahatlığında)
1. `erenkumcuoglu.com/blog/admin/` adresine git
2. "Yeni Blog Yazıları" tıkla
3. Başlık, tarih, kategori ve içeriği gir
4. "Publish" tıkla — otomatik deploy edilir

### Yöntem 2: Doğrudan Markdown (GitHub'dan)
1. GitHub'da `content/blog/` klasörüne git
2. "Add file" → "Create new file" tıkla
3. Dosya adı: `2026-04-09-yazi-basligi.md`
4. İçerik:
```markdown
---
title: "Yazı Başlığı"
date: 2026-04-09
categories: ["Strateji", "Fintech"]
draft: false
---

Yazı içeriği buraya...
```
5. "Commit new file" tıkla → Netlify otomatik deploy eder

## Neden /blog/ alt dizini (subdomain değil)?

- `erenkumcuoglu.com/blog/` → Google bunu ana sitenin parçası sayar
- `blog.erenkumcuoglu.com` → Google bunu AYRI bir site sayar
- Alt dizin, ana domain'in tüm SEO otoritesini miras alır
- Backlink'ler, domain rating — hepsi tek bir yerde toplanır

## Proje Yapısı
```
erenkumcuoglu-blog/
├── hugo.toml              # Hugo konfigürasyonu (baseURL: /blog/)
├── netlify.toml           # Netlify deploy ayarları
├── content/
│   └── blog/              # 263 arşiv yazısı + yeni yazılar
├── static/
│   ├── admin/             # Decap CMS editör arayüzü
│   └── images/            # Blog görselleri
└── themes/
    └── erenblog/          # Custom tema
```

## Video İçerik (İleride)
```markdown
{{< youtube VIDEO_ID >}}
```

## Maliyet
Tamamen ücretsiz: Hugo + Netlify Free + GitHub Free + Decap CMS
