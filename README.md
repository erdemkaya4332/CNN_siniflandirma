# BLG-407 Makine Öğrenmesi - Görüntü Sınıflandırma Projesi

Bu proje, **BLG-407 Makine Öğrenmesi** dersi kapsamında hazırlanmış bir görüntü sınıflandırma projesidir. Proje, kesici/delici ve taşıyıcı/servis eşyalarının sınıflandırılması için üç farklı yaklaşımı karşılaştırmaktadır.

## 📋 Proje Bilgileri

- **Öğrenci:** Mustafa Erdem Kaya
- **Okul Numarası:** 2212721009
- **Ders:** BLG-407 Makine Öğrenmesi
- **GitHub Repo:** `https://github.com/kullanici_adi/CNN_siniflandirma`

## 📁 Proje Yapısı

```
ödev1/
├── dataset/
│   ├── Kesici_Delici/
│   │   └── img_001.jpg - img_100.jpg (100 görüntü)
│   └── Taşıyıcı_Servis/
│       └── img_001.jpg - img_100.jpg (100 görüntü)
├── model1.ipynb          # VGG16 Transfer Learning
├── model2.ipynb          # Sıfırdan Basit CNN
├── model3.ipynb          # Veri Artırma ve Hiperparametre Optimizasyonu
└── README.md
```

## 📊 Veri Seti

Proje, iki farklı sınıftan oluşan dengeli bir görüntü sınıflandırma veri seti içermektedir:

- **Kesici_Delici**: 100 adet görüntü
- **Taşıyıcı_Servis**: 100 adet görüntü

**Toplam:** 200 görüntü

### Veri Seti Özellikleri

- Tüm görüntüler **128x128 piksel** boyutundadır
- Görüntü formatı: **JPG**
- Veri seti **%80 eğitim, %20 doğrulama** olarak bölünmüştür
  - Eğitim: 160 görüntü
  - Doğrulama: 40 görüntü

## 🤖 Modeller

Proje, üç farklı yaklaşımı karşılaştırmak için üç ayrı model içermektedir:

### Model 1: VGG16 Transfer Learning (`model1.ipynb`)

**Yaklaşım:** Transfer Learning (Önceden Eğitilmiş Model)

**Özellikler:**
- VGG16 mimarisi (ImageNet ağırlıkları ile)
- Feature Extraction yaklaşımı (base model dondurulmuş)
- Sadece son katmanlar eğitilmiştir
- 5 epoch ile eğitilmiştir

**Mimari:**
- VGG16 Base Model (dondurulmuş)
- Flatten
- Dense(256, ReLU)
- Dropout(0.5)
- Dense(1, Sigmoid)

**Test Sonuçları:**
- Test Doğruluğu: **%97.50**
- Test Kaybı: **0.0967**

### Model 2: Sıfırdan Basit CNN (`model2.ipynb`)

**Yaklaşım:** Sıfırdan Eğitim (Transfer Learning kullanılmadan)

**Özellikler:**
- Basit CNN mimarisi
- Veri artırma kullanılmamıştır
- Tüm katmanlar sıfırdan eğitilmiştir
- 15 epoch ile eğitilmiştir

**Mimari:**
- Conv2D(32, 3x3) + MaxPooling2D
- Conv2D(64, 3x3) + MaxPooling2D
- Conv2D(64, 3x3) + MaxPooling2D
- Flatten
- Dense(64, ReLU)
- Dense(1, Sigmoid)

**Test Sonuçları:**
- Test Doğruluğu: **%87.50**
- Test Kaybı: **0.4330**

### Model 3: Veri Artırma ve Hiperparametre Optimizasyonu (`model3.ipynb`)

**Yaklaşım:** Model 2'nin Geliştirilmiş Versiyonu

**Özellikler:**
- Veri artırma (Data Augmentation) teknikleri uygulanmıştır
- Daha derin ve geniş mimari
- Dropout katmanı ile overfitting önleme
- Optimize edilmiş hiperparametreler
- 50 epoch ile eğitilmiştir

**Veri Artırma Teknikleri:**
- Rotation: ±20 derece
- Width/Height Shift: %10
- Height Shift: %10
- Zoom: %10
- Horizontal Flip: True
- Vertical Flip: True
- Fill Mode: 'nearest'

**Mimari:**
- Conv2D(64, 3x3) + MaxPooling2D (Model 2'de 32 idi)
- Conv2D(128, 3x3) + MaxPooling2D (Model 2'de 64 idi)
- Conv2D(128, 3x3) + MaxPooling2D (YENİ EKLENEN)
- Flatten
- Dropout(0.5) (YENİ EKLENEN - overfitting önleme)
- Dense(128, ReLU) (Model 2'de 64 idi)
- Dense(1, Sigmoid)

**Hiperparametreler:**
- Öğrenme Oranı: 0.0005 (Model 2'de 0.001 idi)
- Optimizer: Adam (özel learning rate ile)
- Epoch: 50 (Model 2'de 15 idi)
- Callbacks: ModelCheckpoint, ReduceLROnPlateau, EarlyStopping

**Test Sonuçları:**
- Test Doğruluğu: **%95.00**
- Test Kaybı: **0.2777**

## 🚀 Kullanım

### Gereksinimler

Bu projeyi çalıştırmak için aşağıdaki Python kütüphanelerine ihtiyacınız vardır:

```bash
pip install tensorflow keras numpy pillow matplotlib scikit-learn
```

### Google Colab Kullanımı

Notebook dosyaları Google Colab üzerinde çalıştırılmak üzere hazırlanmıştır. Her notebook dosyası:

1. Google Drive'ı mount eder
2. Veri setini yükler
3. Modeli eğitir
4. Sonuçları görselleştirir


## 📈 Model Karşılaştırması

| Model | Yaklaşım | Test Accuracy | Test Loss | Epoch |
|-------|----------|---------------|-----------|-------|
| Model 1 | VGG16 Transfer Learning | **97.50%** | 0.0967 | 5 |
| Model 2 | Sıfırdan Basit CNN | 87.50% | 0.4330 | 15 |
| Model 3 | Veri Artırma + Optimizasyon | **95.00%** | 0.2777 | 50 |

**Sonuç:** 
- **Model 1 (Transfer Learning)** en yüksek performansı göstermiştir (%97.50)
- **Model 3 (Veri Artırma + Optimizasyon)** Model 2'ye göre %7.5 iyileşme sağlamıştır (%87.50 → %95.00)
- Transfer Learning yaklaşımı küçük veri setlerinde en etkili yöntemdir
- Veri artırma ve hiperparametre optimizasyonu, sıfırdan eğitimde önemli performans artışı sağlamıştır

## 📝 Notlar

- Tüm modeller **binary classification** (ikili sınıflandırma) yapmaktadır
- Model ağırlıkları `ModelCheckpoint` ile otomatik olarak kaydedilmektedir
- Her model için eğitim ve doğrulama grafikleri oluşturulmaktadır
- Confusion matrix ve classification report ile detaylı analiz yapılabilir

## 🔧 Teknik Detaylar

### Kullanılan Kütüphaneler

- **TensorFlow/Keras**: Derin öğrenme modeli oluşturma ve eğitme
- **ImageDataGenerator**: Veri ön işleme ve veri artırma
- **Matplotlib**: Grafik ve görselleştirme
- **Scikit-learn**: Model performans metrikleri
- **NumPy**: Sayısal işlemler

### Optimizasyon Parametreleri

- **Optimizer:** Adam
- **Loss Function:** Binary Crossentropy
- **Metrics:** Accuracy
- **Batch Size:** 32
- **Image Size:** 128x128

## 📄 Lisans

Bu proje eğitim amaçlı hazırlanmıştır.

## 👤 İletişim

- **Öğrenci:** Mustafa Erdem Kaya
- **Okul Numarası:** 2212721009
