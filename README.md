# İkinci El Araç Fiyat Tahmini

*Predicting used car prices with machine learning (Random Forest, XGBoost, Linear Regression). Notebook in Turkish.*

Manisa Celal Bayar Üniversitesi, Büyük Veri Analistliği bitirme projesi.

Bir ikinci el aracın satış fiyatını marka, model, yıl, kasa tipi ve kat ettiği mesafe gibi özelliklerinden tahmin eden ve fiyatı en çok hangi özelliklerin belirlediğini inceleyen bir makine öğrenmesi projesi.

## Sonuçlar

| Model | R² (test) | Ortalama mutlak hata (MAE) |
|---|---|---|
| Doğrusal Regresyon | %46,6 | 4.324 $ |
| XGBoost | %88,5 | 1.773 $ |
| **Random Forest** | **%91,9** | **1.446 $** |

- En iyi model **Random Forest**. Eğitim (%93,6) ve test (%91,9) başarısı birbirine yakın, yani model veriyi ezberlemiyor.
- Fiyatı en çok etkileyen özellik **aracın kat ettiği mesafe** (`odometer`). Ardından **kasa tipi** ve **model yılı** geliyor.
- Doğrusal regresyonun düşük kalması, fiyat ile özellikler arasındaki ilişkinin doğrusal olmadığını gösteriyor.

## Yöntem

1. **Keşifçi veri analizi:** Eksik değerler, özet istatistikler, fiyat dağılımı, mesafe ve yıl ile fiyat ilişkisi.
2. **Temizleme:** Fiyatla ilgisi olmayan sütunlar (`vin`, `seller`, `saledate`, `state`) çıkarıldı. Eksik değerli satırlar, 500 $'ın altındaki satışlar ve 500.000 milin üzerindeki araçlar elendi. Satış fiyatındaki aykırı değerler IQR yöntemiyle filtrelendi.
3. **Kodlama:** Kategorik sütunlar (marka, model, donanım, kasa, renk, iç döşeme) `LabelEncoder` ile sayıya çevrildi.
4. **Model girdileri:** `mmr` sütunu, satıştan önce hesaplanmış bir piyasa değeri tahmini olduğu için veri sızıntısını önlemek amacıyla modelden çıkarıldı. `transmission` ve `condition` bu sürümde modele dahil edilmedi.
5. **Modelleme:** Veri %80 eğitim, %20 test olarak ayrıldı. Üç model eğitilip R² ve MAE ile karşılaştırıldı.
6. **Değerlendirme:** Gerçek ve tahmin edilen fiyat grafiği, özellik önem sıralaması ve hata dağılımı.

## Kullanılan araçlar

Python, Pandas, NumPy, scikit-learn, XGBoost, Matplotlib, Seaborn, Google Colab

## Veri seti

`car_prices.csv`, ABD'de gerçekleşmiş ikinci el araç satışlarını içerir (fiyatlar dolar, mesafe mil cinsinden). Dosya 25 MB'tan büyük olduğu için depoda yer almıyor:

**[car_prices.csv dosyasını Google Drive'dan indir](https://drive.google.com/file/d/1aVu17u_T_hIAk2yQCP7UTDhcXO4iCcf2/view?usp=sharing)**

## Çalıştırma

1. `ikinci_el_arac_fiyat_tahmini.ipynb` dosyasını Google Colab'da ya da Jupyter'de aç.
2. Veri setini indir ve 2. bölümdeki dosya yolunu kendi konumuna göre güncelle.
3. Hücreleri sırayla çalıştır.

## Geliştirme fikirleri

- `condition` ve `transmission` sütunlarını modele ekleyip etkisini ölçmek
- Satış anındaki araç yaşı gibi yeni özellikler türetmek
- Doğrusal model için one-hot ya da target encoding denemek
- Çapraz doğrulama (cross-validation) ile sonuçların kararlılığını ölçmek
