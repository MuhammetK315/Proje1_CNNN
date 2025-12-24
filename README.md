# CNN Görüntü Sınıflandırma Projesi

Bu proje, kendi veri seti ile CNN (Convolutional Neural Networks) mimarileri kullanarak görüntü sınıflandırma modellerinin geliştirilmesini içermektedir.

## 📋 Proje Yapısı

```
Proje1_CNNns/
├── dataset/
│   ├── class1/          # İlk sınıfın görüntüleri
│   └── class2/          # İkinci sınıfın görüntüleri
├── model1.ipynb         # Transfer Learning Modeli (VGG16)
├── model2.ipynb         # Basit CNN Mimarisi
├── model3.ipynb         # Geliştirilmiş CNN (Hiperparametre Optimizasyonu)
├── README.md            # Bu dosya
└── [Eğitilmiş modeller .h5 dosyaları]
```

## 📊 Veri Seti Hazırlama

### Gereklilikler:
- **Her sınıf için minimum 50 görüntü** (toplam minimum 100)
- **Görüntü boyutu:** 128×128 piksel (en az 64×64)
- **Görüntü formatı:** JPG, PNG
- **Çeşitlilik:** Farklı açılar, ışık koşulları, arka planlar

### Veri Yapısı:
```
dataset/
├── class1/
│   ├── img1.jpg
│   ├── img2.jpg
│   └── ...
└── class2/
    ├── img1.jpg
    ├── img2.jpg
    └── ...
```

**Not:** Tüm veriler kendi çektiğiniz, özgün olmalıdır. İnternet'ten indirilen veri setleri kabul edilmez.

---

## 🤖 Modeller

### Model 1: Transfer Learning (VGG16 + Fine-tuning)

**Mimarisi:**
- VGG16 (ImageNet ağırlıkları ile)
- Global Average Pooling
- Dense katmanları (256 → sınıf sayısı)
- Dropout: 0.3

**Özellikleri:**
- Önceden eğitilmiş ağırlıklardan yararlanır
- Daha az veri ile yüksek doğruluk sağlar
- Transferable özellikler öğrenir
- Eğitim süresi: ~1-2 dakika

**Beklenen Performans:** 85-95% (veri seti kalitesine bağlı)

---

### Model 2: Basit CNN (Sıfırdan Eğitilmiş)

**Mimarisi:**
```
Input (128×128×3)
    ↓
Conv2D(32) → Conv2D(32) → MaxPool(2×2) → Dropout(0.25)
    ↓
Conv2D(64) → Conv2D(64) → MaxPool(2×2) → Dropout(0.25)
    ↓
Flatten → Dense(128) → Dropout(0.5) → Dense(num_classes)
```

**Parametreler:**
- Filtreler: 32, 64
- Batch Size: 32
- Öğrenme Oranı: 0.001
- Epoch: 30

**Beklenen Performans:** 70-80%

---

### Model 3: Geliştirilmiş CNN (Hiperparametre Optimizasyonu)

**Denenen Konfigürasyonlar:**

| Config | Filtreler | Batch Size | LR | Dropout | Beklenen Sonuç |
|--------|-----------|------------|-----|---------|----------------|
| 1 | 32/64 | 32 | 0.001 | 0.25/0.5 | Baseline |
| 2 | 64/128 | 32 | 0.001 | 0.25/0.5 | Daha Derin |
| 3 | 32/64 | 64 | 0.0005 | 0.3/0.4 | Farklı Hızı |
| 4 | 64/128 | 64 | 0.0005 | 0.3/0.4 | En Optimize |
| 5 | 48/96 | 48 | 0.0008 | 0.3/0.5 | Hybrid |

**Veri Artırımı Teknikleri:**
- Rotation: ±15°
- Width/Height Shift: ±10%
- Horizontal Flip: %50
- Zoom: 10-20%

**Beklenen Performans:** 80-90% (Config_4 genellikle en iyi)

---

## 🚀 Kullanım

### 1. Gerekli Kütüphaneler
```bash
pip install tensorflow keras numpy pandas matplotlib scikit-learn
```

### 2. Veri Seti Hazırlama
1. `dataset/class1/` ve `dataset/class2/` klasörlerini oluşturun
2. Çektiğiniz görüntüleri uygun klasörlere kaydedin
3. Görüntülerin en az 64×64 piksel olduğundan emin olun

### 3. Modelleri Çalıştırma

**Model 1 (Transfer Learning):**
```python
# model1.ipynb dosyasını açın ve tüm hücreleri çalıştırın
# Otomatik olarak:
# - Veri setini yükler
# - VGG16 modelini oluşturur
# - Modeli eğitir
# - Grafikler çizer
# - Sonuçları raporlar
```

**Model 2 (Basit CNN):**
```python
# model2.ipynb dosyasını açın ve tüm hücreleri çalıştırın
# Aynı adımları takip eder
```

**Model 3 (Geliştirilmiş CNN):**
```python
# model3.ipynb dosyasını açın
# 5 farklı konfigürasyonla otomatik eğitim yapılır
# Sonuçlar tablosu ve grafikler otomatik oluşturulur
```

---

## 📈 Sonuçların Yorumlanması

### Başarı Kriteleri:
- ✅ Model1 > %85 test doğruluğu
- ✅ Model2 > %70 test doğruluğu  
- ✅ Model3 > Model2 doğruluğu
- ✅ Veri artırımı ile overfitting azalması

### Olası Sorunlar ve Çözümler:

| Sorun | Olası Sebep | Çözüm |
|-------|-----------|--------|
| Düşük Doğruluk (<50%) | Yetersiz veri | Daha fazla görüntü toplayın |
| Overfitting (train>>val) | Modelin çok karmaşık | Dropout arttırın, veri artırımı ekleyin |
| Underfitting (train≈val, düşük) | Modelin çok basit | Filtre sayısını arttırın, epoch arttırın |
| Veri boyutu sorunları | Görüntü boyutu uyumsuz | Tüm görüntüleri 128×128'e yeniden boyutlandırın |

---

## 📊 Model Performans Karşılaştırması

### Beklenen Sonuç Senaryosu:

**Senaryo A: Transfer Learning Başarılı**
```
Model 1: 92% (Transfer Learning Avantajı)
Model 2: 78% (Sıfırdan Eğitim)
Model 3: 85% (Hiperparametre Optimizasyonu)

Sebep: VGG16, ImageNet'ten transfer edilen özellikler kullanır
```

**Senaryo B: Hiperparametre Optimizasyonu Başarılı**
```
Model 1: 88% (Transfer Learning)
Model 2: 75% (Baseline)
Model 3: 91% (Optimize Parametreler + Veri Artırımı)

Sebep: Model3'ün veri artırımı ve hiperparametre tuning'i daha etkili
```

---

## 📝 Sözlü Sunum Hazırlığı

### Sorulacak Sorular:
1. **"Hangi model neden daha iyi sonuç verdi?"**
   - Cevap: Test doğruluğu ve grafikler kullanarak açıklayın

2. **"Transfer learning model1 neden avantaj/dezavantaj sağladı?"**
   - Avantaj: ImageNet ağırlıkları, daha az eğitim veri gerekir
   - Dezavantaj: Veri setinizin ImageNet'e benzememesi

3. **"Model3'te yapılan hiperparametre değişiklikleri hangi etkiye sahip?"**
   - Filtre sayısı: Capacity arttırır
   - Batch size: Learning rate ve stabilite etkiler
   - Öğrenme oranı: Convergence hızı
   - Dropout: Overfitting kontrolü

4. **"Model3, Model2'den neden daha iyi/kötü?"**
   - Grafikler ve sayılarla destekleyin
   - Veri artırımının etkisini açıklayın

---

## 🔍 Kod Açıklamaları

### Önemli Fonksiyonlar:

**Veri Yükleme:**
```python
# Görüntüleri klasörlerden yükler ve normalize eder
# Eğitim/test setine böler (80/20)
```

**Model Mimarisi:**
```python
# Sequential: Katmanları sırayla ekler
# Conv2D: Konvolüsyonal katman
# MaxPooling2D: Boyut indirme
# Dropout: Overfitting önleme
# Dense: Tam bağlı katman
```

**Eğitim:**
```python
# ImageDataGenerator: Online veri artırımı
# fit(): Modeli eğitir
# evaluate(): Test performansı ölçer
```

---

## 📁 Çıktı Dosyaları

**Eğitim sonunda oluşturulacak dosyalar:**
- `model1_transfer_learning.h5` - Eğitilmiş Transfer Learning Modeli
- `model2_simple_cnn.h5` - Eğitilmiş Basit CNN Modeli  
- `model3_optimized.h5` - En iyi Optimize Model
- `model3_hyperparameter_results.csv` - Hiperparametre Özet Tablosu

---

## ⚠️ Önemli Notlar

✅ **YAPMANIZ GEREKENLER:**
- Tüm kodu kendiniz yazın/anlayın
- Özgün veri seti oluşturun
- Hiperparametre değişiklikleri yapın
- Sonuçlarınızı açıklayabilin

❌ **YAPMAYACAĞINIZ ŞEYLER:**
- Hazır notebook kullanmayın
- İnternet'ten veri indirmeyin
- Kodu kopyala-yapıştır yapmayın
- Koda dair açıklama yapamamayın

---

## 📚 Kaynaklar

- [TensorFlow/Keras Dokumentasyonu](https://www.tensorflow.org/api_docs)
- [VGG16 Hakkında](https://arxiv.org/abs/1409.1556)
- [ImageDataGenerator](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator)
- [Hiperparametre Tuning](https://www.tensorflow.org/guide/keras/tuning_overview)

---

## 📞 Hata Giderme

**Problem: ModuleNotFoundError**
```bash
pip install --upgrade tensorflow keras numpy pandas matplotlib scikit-learn
```

**Problem: Görüntü yükleme hatası**
- Görüntü formatının doğru olduğunu kontrol edin (.jpg, .png)
- Dosya adında özel karakterler olup olmadığını kontrol edin

**Problem: Bellek yetersiz hatası**
- Batch size'ı azaltın
- Görüntü boyutunu küçültün
- Epoch sayısını azaltın

---

## 🎯 Başarı Belirtileri

✅ Tüm 3 model başarıyla eğitilmiş  
✅ Test doğruluk grafikleri gösterilmiş  
✅ Model3 hiperparametre tablosu oluşturulmuş  
✅ Kod anlaşılabilir ve iyi yapılandırılmış  
✅ README.md detaylı açıklamalar içeriyor  
✅ GitHub'a yüklenmiş ve erişilebilir  

---

**Oluşturucu:** Muhammet KABACALI
Numara = 2112729007
github = https://github.com/MuhammetK315/Proje1_CNNN.git
