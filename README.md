# **Uygulama Raporu**  
## **1. Proje Amacı**

Bu proje, Flutter kullanılarak iki ekranlı basit bir mobil uygulama geliştirme örneğidir. Kullanıcı, birinci ekranda yer alan bir butona tıkladığında ikinci ekrana yönlendirilir. Amaç, Flutter'da sayfa geçişlerinin (`Navigator`) nasıl yapıldığını ve `StatelessWidget` bileşenlerinin kullanımını göstermektir.

## **2. Kullanılan Teknolojiler**

- **Flutter SDK**
- **Dart dili**
- **Material Design bileşenleri**

## **3. Ana Bileşenler ve Açıklamaları**

### 3.1 `main()` Fonksiyonu

Uygulamanın giriş noktasıdır. `runApp()` fonksiyonu ile `MyApp` widget'ı çalıştırılır.

```dart
void main() {
  runApp(const MyApp());
}
```

### 3.2 `MyApp` Sınıfı

Uygulamanın ana yapı taşıdır. `MaterialApp` bileşenini içerir ve tema ile ana ekran tanımlanır.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'EruBS434AssignedProject',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const FirstScreen(),
    );
  }
}
```

- `MaterialApp`: Uygulamanın tüm yapı taşlarını barındırır.
- `theme`: Uygulama temasını belirler.
- `home`: Başlangıçta gösterilecek ekran.

---

### 3.3 `FirstScreen` Sınıfı

İlk ekranda, kullanıcıya bir buton sunulmakta ve bu butona basıldığında ikinci ekrana yönlendirme yapılmaktadır.

```dart
class FirstScreen extends StatelessWidget {
  const FirstScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('İlk Ekran'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SecondScreen()),
            );
          },
          child: const Text('İkinci Ekrana Git'),
        ),
      ),
    );
  }
}
```

- `Scaffold`: Sayfanın temel yapısını oluşturur.
- `AppBar`: Sayfa başlığını gösterir.
- `ElevatedButton`: Tıklanabilir buton.
- `Navigator.push(...)`: Sayfalar arası geçişi sağlar.

---

### 3.4 `SecondScreen` Sınıfı

İkinci ekranda sadece bir `Text` widget'ı bulunmaktadır.

```dart
class SecondScreen extends StatelessWidget {
  const SecondScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('İkinci Ekran'),
      ),
      body: const Center(
        child: Text('Bu ikinci ekran!'),
      ),
    );
  }
}
```

Bu ekran yalnızca bir bilgi ekranıdır. Geri gitmek için Android'de sistemdeki “geri” butonu yeterlidir.

---

## **4. Uygulama Akışı**

1. Uygulama başlatılır, `FirstScreen` görüntülenir.
2. Kullanıcı "İkinci Ekrana Git" butonuna basar.
3. `SecondScreen` açılır.
4. Kullanıcı geri düğmesine bastığında `FirstScreen`’e döner.
