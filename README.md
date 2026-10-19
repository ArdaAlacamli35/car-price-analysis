# İkinci El Araç Fiyat Tahmini

*Used car price prediction on ~530k US auction sales with Gradient Boosting, Random Forest and Linear Regression. Notebook in Turkish.*

Arda Alaçamlı, Büyük Veri Analistliği öğrencisi (Manisa Celal Bayar Üniversitesi). İlk versiyonu dönem projesi olarak hazırlandı, sonra baştan yeniden geliştirildi.

**Soru:** Bir ikinci el aracın satış fiyatını markası, modeli, yaşı, kilometresi ve durumu gibi bilgilerden tahmin edebilir miyim? Fiyatı en çok ne etkiliyor?

## Sonuçlar

Temizlikten sonra kalan **528.743 satış** ile üç model denendi:

| Model | R² (test) | Ortalama hata (MAE) |
|---|---|---|
| **Gradient Boosting** | **%95,8** | **1.199 $** |
| Random Forest | %95,3 | 1.225 $ |
| Doğrusal Regresyon | %69,7 | 3.504 $ |

- Fiyatı en çok etkileyenler: **mesafe**, **kasa tipi** ve **marka**.
- Eğitim (%96,5) ve test (%95,8) başarısı neredeyse aynı, yani model ezberlemiyor.
- 10-50 bin $ arasındaki araçlarda ortalama hata fiyatın yaklaşık %7'si.

## Neler yaptım

1. **Veriye ilk bakış:** Eksik değerler, mantıksız sayılar (999.999 mil, 1 $'lık satışlar).
2. **Temizleme:**
   - Sütunları kaymış satırları sildim.
   - `Ford` / `ford` gibi yazım farklarını birleştirdim, boş renk ve vites bilgisini `bilinmiyor` yaptım.
   - **`condition` sütunu iki farklı ölçekte girilmişti** (1-5 ve 10-49). Tek ölçeğe getirince durumun fiyatla korelasyonu 0,32'den 0,54'e çıktı.
   - 500 $ altı / 100.000 $ üstü fiyatları ve mantıksız mesafeleri çıkardım.
3. **Yeni sütun:** Satış tarihinden aracın satıldığı andaki **yaşını** hesapladım.
4. **Grafiklerle keşif:** Fiyat dağılımı, yaş, mesafe, durum ve markaya göre fiyat.
5. **Modeller:** Doğrusal Regresyon, Random Forest ve Gradient Boosting. `mmr` sütunu (önceden hesaplanmış piyasa değeri) fiyatın neredeyse aynısı olduğu için modele verilmedi.
6. **Değerlendirme:** Gerçek ve tahmin karşılaştırması, fiyat aralığına göre hata, fiyatı belirleyen bilgiler.
7. **Deneme:** Özellikleri verilen bir aracın fiyatını tahmin eden küçük bir fonksiyon.

## Kullanılan araçlar

Python, Pandas, NumPy, scikit-learn, Matplotlib, Seaborn

## Veri seti

`car_prices.csv`: ABD'de gerçekleşmiş yaklaşık 560 bin ikinci el araç satışı (fiyatlar dolar, mesafe mil cinsinden). Dosya 25 MB'tan büyük olduğu için depoda yer almıyor:

**[car_prices.csv dosyasını Google Drive'dan indir](https://drive.google.com/file/d/1aVu17u_T_hIAk2yQCP7UTDhcXO4iCcf2/view?usp=sharing)**

## Çalıştırma

1. `ikinci_el_arac_fiyat_tahmini.ipynb` dosyasını Jupyter ya da Google Colab'da aç.
2. Veri setini indirip notebook ile aynı klasöre koy. Colab'da çalışıyorsan 2. bölümdeki notu takip et.
3. Hücreleri sırayla çalıştır (tamamı birkaç dakika sürer).
