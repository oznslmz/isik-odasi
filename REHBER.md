# Işık Odası — Mobil Uygulamaya Dönüştürme Rehberi

Bu klasör oyunun uygulama-hazır halidir. İki aşamalı bir yol izliyoruz:
önce PWA (bugün telefonuna kurulabilir), sonra Capacitor ile mağaza paketi.

## Klasör içeriği

- index.html            → oyunun kendisi (ilerleme kaydı + çevrimdışı destek eklendi)
- manifest.webmanifest  → PWA kimliği (isim, ikon, dikey ekran kilidi)
- sw.js                 → service worker (çevrimdışı oynama)
- icons/                → uygulama ikonları (192, 512, maskable, apple-touch)
- capacitor.config.json → Aşama 2 için Capacitor yapılandırması
- package.json          → Aşama 2 için npm betikleri

---

## AŞAMA 1 — PWA: bugün telefonuna kur (ücretsiz, mağazasız)

PWA'nın çalışması için dosyaların bir HTTPS adresinde yayınlanması gerekir.
En kolay ücretsiz yol GitHub Pages:

1. github.com'da hesap aç (varsa atla) ve "isik-odasi" adında yeni bir
   repository oluştur (Public).
2. Bu klasördeki TÜM dosyaları repoya yükle ("uploading an existing file"
   bağlantısıyla sürükle-bırak yeterli; icons klasörünü de yükle).
3. Repo > Settings > Pages > Source: "Deploy from a branch",
   Branch: main, klasör: / (root) → Save.
4. 1-2 dakika sonra adresin hazır:
   https://KULLANICIADIN.github.io/isik-odasi/
5. Telefonda bu adresi aç:
   - Android/Chrome: menü (⋮) > "Ana ekrana ekle" / "Uygulamayı yükle"
   - iPhone/Safari: Paylaş > "Ana Ekrana Ekle"
   Artık tam ekran, çevrimdışı çalışan, ikonuyla ana ekranda duran bir
   uygulaman var. Yıldız ilerlemesi cihazda saklanır.

Alternatif barındırma: Netlify Drop (app.netlify.com/drop — klasörü
sürükle, anında yayında) veya Cloudflare Pages.

---

## AŞAMA 2 — Capacitor: Google Play / App Store paketi

PWA'yı beğenip mağazaya çıkmak istersen:

### Gereksinimler
- Node.js (LTS) → nodejs.org
- Android için: Android Studio (ücretsiz) + Google Play geliştirici
  hesabı (25 USD, tek seferlik)
- iOS için: Mac + Xcode + Apple Developer hesabı (99 USD/yıl)

### Android adımları
```bash
cd isik-odasi-app
npm install

# web dosyalarını Capacitor'ın beklediği www klasörüne kopyala
mkdir www
cp index.html manifest.webmanifest sw.js www/
cp -r icons www/

npx cap add android      # android projesi oluşturur
npx cap sync android
npx cap open android     # Android Studio açılır
```
Android Studio'da:
1. Build > Generate Signed Bundle/APK → önce test için APK üret,
   telefonuna at, dene.
2. Mağaza için AAB (Android App Bundle) üret ve imzala
   (keystore dosyanı KAYBETME — güncellemeler için şart).
3. play.google.com/console → uygulama oluştur, AAB'yi yükle,
   ekran görüntüleri + açıklama + gizlilik politikası ekle, incelemeye gönder.

### iOS adımları (Mac gerekir)
```bash
npm install @capacitor/ios
npx cap add ios
npx cap open ios
```
Xcode'da imzalama ayarlarını yap, arşivle, App Store Connect'e yükle.

---

## Mağaza öncesi kontrol listesi

- [ ] Uygulama adı ve appId (capacitor.config.json içinde
      com.ozan.isikodasi — istersen değiştir)
- [ ] Gizlilik politikası sayfası (veri toplamıyoruz; tek paragraflık
      bir sayfa yeterli, GitHub Pages'e koyulabilir)
- [ ] En az 2-3 ekran görüntüsü (telefonda çekip kullan)
- [ ] İçerik derecelendirme anketi (Play Console içinde; bulmaca oyunu
      için "Herkes" çıkar)
- [ ] Sürüm notları

## Sonraki geliştirme fikirleri (mağaza sonrası)

- Titreşim geri bildirimi (Capacitor Haptics eklentisi)
- Daha fazla bölüm + bölüm kilidi (yıldızla açılan odalar)
- Editör bölümlerini oyun içinde kaydetme/listeleme
- Reklam (AdMob) veya "reklamsız sürüm" satın alımı — istersen sonra
  birlikte ekleriz
