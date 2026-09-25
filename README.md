# İnşaat Maliyet Tahmin Aracı — Streamlit Uygulaması

Hafta 2 dersinde Google Colab'da eğitip Drive'a kaydettiğimiz lineer regresyon
modelinin, **kod bilmeyen biri için de kullanılabilir bir web arayüzüne**
taşınmış hâli. Ders notunun "Model Eğitildikten Sonra Nasıl Kullanılır? — 1.
Modeli Saklamak" bölümünde bahsedilen Streamlit seçeneğinin somut uygulamasıdır.

## Klasördeki dosyalar

```
streamlit_app/
├── app.py                  # Streamlit uygulamasının kendisi
├── train_models.py         # Modelleri eğitip models/ klasörüne kaydeden script
├── requirements.txt        # Gerekli Python paketleri
├── models/
│   ├── maliyet_modeli_basit.pkl      # Hücre 5'teki model (alan + kat)
│   ├── maliyet_modeli_gelismis.pkl   # Hücre 5 (Devam) modeli (+zemin+yıl)
│   └── meta.json                     # Min/max aralıkları, R²/MAE, katsayılar
└── README.md                # Bu dosya
```

Modeller, gerçek `insaat_proje_verileri.csv` dosyanızla, **Hafta 2 ders
notundaki Hücre 5 ve Hücre 5 (Devam) kodlarıyla birebir aynı şekilde**
eğitilmiştir. Sonuçlar derste gördüğümüz sayılarla tam eşleşir:

| | Test R² | Test MAE |
|---|---|---|
| Basit model | -1,686 | 147.900.998 TL |
| Gelişmiş model | -0,647 | 115.058.404 TL |

## Yerel bilgisayarda çalıştırma

```bash
pip install -r requirements.txt
streamlit run app.py
```

Tarayıcınızda otomatik olarak `http://localhost:8501` açılır.

Veri setiniz değiştiyse veya modelleri yeniden eğitmek isterseniz:

```bash
python train_models.py
```

(Bu komut `insaat_proje_verileri.csv` dosyasının nerede olduğunu
`train_models.py` içindeki `DATA_PATH` satırından ayarlamanızı gerektirebilir.)

## Ücretsiz, herkese açık bir link ile paylaşma (Streamlit Community Cloud)

Öğrencilerin veya kod bilmeyen birinin (örn. bir proje müdürünün) sadece bir
linke tıklayarak bu aracı kullanabilmesi için:

1. Bu `streamlit_app` klasörünü bir GitHub deposuna (repository) yükleyin
   (Colab/Drive değil — Streamlit Cloud GitHub'dan okur).
2. [share.streamlit.io](https://share.streamlit.io) adresine gidin, GitHub
   hesabınızla giriş yapın.
3. "New app" → deponuzu ve `app.py` dosyasını seçin → Deploy.
4. Birkaç dakika içinde `https://[uygulama-adiniz].streamlit.app` şeklinde
   herkese açık, ücretsiz bir link elde edersiniz.

Bu adım, ders notundaki "Google Drive + Colab yaklaşımı" ile "Streamlit"
seçeneği arasındaki farkı tam olarak gösterir: birincisi hâlâ Python/Colab
bilgisi gerektirir, ikincisi ise herkesin kullanabileceği bir araca dönüşür.

## Uygulamanın öğretim amacıyla bağlantısı

Uygulama, Hafta 2'de işlenen üç konuyu canlı olarak sergiler:

- **Kaydetme/Yükleme mekaniği**: `joblib.load()` ile aynı model Colab dışında
  kullanılıyor.
- **Ekstrapolasyon uyarısı**: Girdiler eğitim verisinin (alan_m2: 413-3386,
  kat_sayisi: 2-15) dışına çıkarsa uygulama kırmızı bir uyarı gösterir — tam
  olarak ders notundaki "150 m², 6 kat → -45.660.235 TL, ANLAMSIZ" örneğini
  tetikler.
- **Overfitting karşılaştırması**: Gelişmiş modeli seçtiğinizde, kenar
  çubuğunda Train R² (0,821) ile Test R² (-0,647) arasındaki büyük farkı
  görürsünüz — aynı ekranda iki modeli karşılaştırarak "az veri + çok
  değişken = ezber riski" dersini tekrar hatırlatır.
