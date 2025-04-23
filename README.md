# Flutter Uygulamayı Play Store'da Yayınlama

### 1. Uygulama Bilgilerini Düzenleyelim

Önce `android/app/build.gradle` dosyasını açıp bilgileri kendimize göre ayarlıyoruz:

```gradle
applicationId "com.seninpaketin.uygulamaadi"
versionCode 1
versionName "1.0.0"
minSdkVersion 21
```

Burada `applicationId` uygulamanın kimliği, `versionCode` ve `versionName` sürüm bilgileri, `minSdkVersion` ise minimum Android sürümü. Bunları projene uygun dolduralım.

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

Şimdi uygulamamızı Play Store’a yüklemek için Google Play Console’a girelim. Adres şu: https://play.google.com/console. Eğer hesabın yoksa, bir Google hesabı ile kaydolman gerekiyor. Tek seferlik bir kayıt ücreti var, bunu ödeyip hesabını aktif edelim. Şimdi adım adım neler yapacağız, bakalım:

- **Yeni Uygulama Oluşturma**: Console’da sol menüden “Tüm uygulamalar”a tıklayıp “Uygulama oluştur” butonuna basalım. Uygulamanın adını girelim (bu, Play Store’da görünecek isim). Dil seçimi ve uygulama türünü (örneğin, uygulama mı oyun mu) belirleyelim. Ücretli mi ücretsiz mi olacağına da burada karar verelim.

- **AAB Dosyasını Yükleme**: Uygulama oluşturulduktan sonra, sol menüden “Üretim” sekmesine gidelim. Burada “Sürüm oluştur” butonuna tıklayıp “Uygulama paketi” seçeneğini seçelim. Daha önce oluşturduğumuz `.aab` dosyasını buraya sürükleyip bırakalım veya dosya seçerek yükleyelim. Yükleme tamamlanınca sürüm notlarını ekleyelim (örneğin, “İlk sürüm, temel özellikler eklendi” gibi kısa bir açıklama).

- **Mağaza Sayfasını Düzenleme**: Sol menüden “Mağaza varlığı” kısmına gidelim. Burada şunları yapalım:

  - **Uygulama İkonu**: 512x512 piksel boyutunda bir PNG dosyası yükleyelim. İkon net ve dikkat çekici olsun.
  - **Ekran Görüntüleri**: Uygulamanın nasıl göründüğünü gösteren en az 2, en fazla 8 ekran görüntüsü ekleyelim. Telefon, tablet veya diğer cihazlar için ayrı görüntüler yükleyebiliriz. 1080x1920 gibi yüksek çözünürlüklü resimler ideal.
  - **Açıklama**: Uygulamanın ne işe yaradığını anlatan kısa ve uzun bir açıklama yazalım. Kısa açıklama 80 karakteri geçmesin, uzun açıklama ise 4000 karaktere kadar olabilir. Kullanıcıyı çekecek, sade bir dil kullanalım.
  - **Kategoriler ve Etiketler**: Uygulamanın kategorisini (örneğin, Eğitim, Yaşam Tarzı) seçelim ve ilgili etiketler ekleyelim.
  - **Gizlilik Politikası**: Eğer uygulaman veri topluyorsa, bir gizlilik politikası bağlantısı eklememiz lazım. Basit bir gizlilik politikası oluşturup bir web sitesine yükleyebiliriz (örneğin, Google Sites kullanarak).

- **İçerik Derecelendirmesi**: Sol menüden “İçerik derecelendirmesi” kısmına girelim. Burada bir anket dolduracağız. Uygulamanın içeriği (örneğin, şiddet, dil, satın alma) hakkında sorular sorulacak. Doğru cevaplayalım, bu, uygulamanın yaş sınırı için önemli.

- **Uygulama Erişimi ve İzinler**: Uygulamanın herkese açık mı yoksa belirli kullanıcılarla sınırlı mı olacağını seçelim. Ayrıca, uygulamanın kullandığı izinleri (örneğin, kamera, konum) açıklayalım. Play Store, bu konuda şeffaf olmamızı istiyor.

- **İncelemeye Gönderme**: Tüm bilgileri doldurduktan sonra, sağ üstteki “İncelemeye gönder” butonuna basalım. Önce bir özet ekranı çıkacak, her şeyin doğru olduğundan emin olalım. Eğer eksik bir şey varsa, sistem bize söyleyecek. Sonra “Üretime gönder” deyip beklemeye başlayalım.