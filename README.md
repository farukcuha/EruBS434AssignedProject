# Proje : Flutter Navigation ile Sayfa Geçişi : Erciyes Üniversitesi Fakülte : Mühendislik Fakültesi Bölüm : Bilgisayar Mühendisliği Ders : Mobile Application Development Dersi Veren : Dr. Öğr. Üyesi. [Fehim KÖYLÜ] Öğrenci : Ahmet Faruk Çuha Öğrenci No : 1030510500 Tarih : 30.04.2025

# Flutter Uygulamayı Play Store'da Yayınlama

### 1. Uygulama Bilgilerini Düzenleyelim

`android/app/build.gradle` dosyasını açıp bilgileri kendimize göre ayarlıyoruz:

```gradle
applicationId "com.seninpaketin.uygulamaadi"
versionCode 1
versionName "1.0.0"
minSdkVersion 21
```

`applicationId` uygulamanın kimliği, `versionCode` ve `versionName` sürüm bilgileri, `minSdkVersion` ise minimum Android sürümü. Bunları projeye uygun dolduralım.

### 2. Uygulamayı İmzalamak

Play Store için uygulamayı imzalamamız gerekiyor. Terminalde şu komutu çalıştıralım:

```bash
keytool -genkey -v -keystore key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

Bu, `key.jks` adında bir imza dosyası oluşturur. Sorulan bilgileri dikkatlice girelim. Sonra `android` klasörüne `key.properties` dosyası oluşturup şunları yazalım:

```properties
storePassword=seninsifren
keyPassword=seninsifren
keyAlias=key
storeFile=key.jks
```

Ardından `android/app/build.gradle` dosyasına imza ayarlarını ekleyelim:

```gradle
signingConfigs {
    release {
        keyAlias keystoreProperties['keyAlias']
        keyPassword keystoreProperties['keyPassword']
        storeFile file(keystoreProperties['storeFile'])
        storePassword keystoreProperties['storePassword']
    }
}
```

Böylece imza işi tamam, devam edelim.

### 3. Yayın Paketini Hazırlayalım

Uygulamamızı paketlemek için terminalde şu komutu çalıştıralım:

```bash
flutter build appbundle
```

Bu, `.aab` (Android App Bundle) dosyasını oluşturur. Play Store AAB’yi tercih ettiği için bunu kullanalım.

### 4. Google Play Console’a Gidelim (Detaylı)

Şimdi uygulamamızı Play Store’a yüklemek için Google Play Console’a girelim. Adres şu: https://play.google.com/console. Tek seferlik bir kayıt ücreti var, bunu ödeyip hesabını aktif edelim.

- **Yeni Uygulama Oluşturma**: Console’da sol menüden “Tüm uygulamalar”a tıklayıp “Uygulama oluştur” butonuna basalım. Uygulamanın adını girelim. Dil seçimi ve uygulama türünü belirleyelim. Ücretli mi ücretsiz mi olacağına da burada karar verelim.

- **AAB Dosyasını Yükleme**: Uygulama oluşturulduktan sonra, sol menüden “Üretim” sekmesine gidelim. Burada “Sürüm oluştur” butonuna tıklayıp “Uygulama paketi” seçeneğini seçelim. Daha önce oluşturduğumuz `.aab` dosyasını buraya sürükleyip bırakalım veya dosya seçerek yükleyelim. Yükleme tamamlanınca sürüm notlarını ekleyelim.

- **Mağaza Sayfasını Düzenleme**: Sol menüden “Mağaza varlığı” kısmına gidelim. Burada şunları yapalım:

  - **Uygulama İkonu**: 512x512 piksel boyutunda bir PNG dosyası yükleyelim.
  - **Ekran Görüntüleri**: Uygulamanın nasıl göründüğünü gösteren en az 2, en fazla 8 ekran görüntüsü ekleyelim.
  - **Açıklama**: Uygulamanın ne işe yaradığını anlatan kısa ve uzun bir açıklama yazalım. Kısa açıklama 80 karakteri geçmesin, uzun açıklama ise 4000 karaktere kadar olabilir.
  - **Kategoriler ve Etiketler**: Uygulamanın kategorisini seçelim ve ilgili etiketler ekleyelim.

- **İncelemeye Gönderme**: Tüm bilgileri doldurduktan sonra, sağ üstteki “İncelemeye gönder” butonuna basalım. Önce bir özet ekranı çıkacak, her şeyin doğru olduğundan emin olalım. Eğer eksik bir şey varsa, sistem bize söyleyecek. Sonra “Üretime gönder” deyip beklemeye başlayalım.
