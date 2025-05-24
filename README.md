# Uçuş Gecikmesi Regresyon Modeli(flight-delay-regression)
Makine öğrenmesi ile uçak kalkış gecikmelerini sayısal olarak tahmin etme.

## Proje Hakkında
* **Kaggle Adresi**: [Projeyi Kaggle'da Gör](https://www.kaggle.com/code/tugbakaratas/flight-delay-regression)
* Projenin amacı makine öğrenmesi yöntemini kullanarak uçağın kalkışının ne kadar gecikeceğini tahmin etmektir. Bunun için Kaggle platformunda bulunan veri seti kullanılmış olup gecikmeleri sayısal olarak tahmin etmesi için regresyon modeli kullanılmıştır.

## Kullanılan Veri Seti
* **Kullanılan Data Adresi**: [Kaggle'daki Flight Delay Dataset](https://www.kaggle.com/datasets/niszarkiah/flight-dataset)
*  Veri setinde uçuşu yapan uçağın bağlı olduğu şirket adı, uçağın kalkış yaptığı ve vardığı havaalanı isimleri, uçağın uçuş numarası ve kuyruk numarası, uçuş yaptığı tarih, planlanan ve gerçekleşen kalkış-varış saatleri ve bunnların ne kadar geciktiği, uçuşun ne kadar sürdüğü, uçuşun mesafesi bilgileri vardır.
*  Veri setinde kalışın ve varışın gerçekleştiği ve planlanan kalkış ve varış saatleri farklı formatta ele alınmıştır. Örneğin 1.0 değeri 00:01 de gerçekleştiğini söylemektedir.
*  Veri setindeki carrier, tailnum, origin, dest, time_hour kolonlarının veri türü object tir, diğer kolonlar ise float veya int türündedir.
*  Toplamda 435352 veri vardır. dep_time, dep_delay, arr_time, arr_delay, tail_num, air_time kolonlarında ise null olan değerler bulunmaktadır.

## Kullanılan Teknolojiler
* Projeyi yaparken **Python** dili kullanılmıştır. Pek çok kütüphaneyi desteklemesi ve kolay anlaşılması nedeni ile python dili tercih edilmiştir.
* Verileri gözlemlemek ve işlemek için **NumPy ve Pandas** kütüphaneleri, dosya yolunu doğru şekilde oluşturmak için **os** modülü, veri görselleştirme, grafik çizme için **Seaborn ve Matplotlib** kütüphanesi, Python da en çok kullanılan makine öğrenmesi kütüphanesi olan **scikit-learn (sklearn)** kütüphanesini veri ön işleme, veri bölme, model eğitme, model değerlendirme için kullanılmıştır.
* Kod geliştirme işlemlerini **Kaggle** platformu ve **Goggle Colab** üzerinden gerçekleştirilmiştir. Goggle Colab platformunu tercih etmemdeki sebep **T4 GPU** özelliğini kullanmama izin vermesidir.

## Veri Önişleme
* Veri setimde bazı verilerin NaN olduğunu görmüştüm bu nedenle o satırları silmekle işle başlandı.
* Saat verileri farklı bir formattaydı o değerler datetime türüne çevrilip daha sonra model eğitimde kullanılacağı için saniye türüne çevrilmiştir.
* Uçuşun gerçekleştiği tarih bilgisi doğrudan modele verilemeyeceği için o tarihin haftanın hangi gününe denk geldiği tespit edilip day_of_week adlı yeni bir kolon oluşturulmuştur.
* Uçuş ile ilgili modele de etki edeceği düşünülen ve object türünde olan carrier, origin ve dest kolonlarının değerleri **Label Encoding** metodu kullanılarak sayısallaştırılmıştır.
* Eğitim verileri sayısal olarak aynı ölçeğe getirmek için **özellik ölçeklendirme (feature scaling)** yapılmış olup Standart Normal Dağılım kullanılmıştır.
* Bazı kolonlarda aykırı değerler tespit edilmiştir ancak aykırı değerlerin modele negatif yönde etki etmeyeceği saptanmış ve müdahale edilmemiştir.

## Model Oluşturma
* Regresyon problemi üzerine durulduğu için ve çok sayıda veri olduğu için veriler arasındaki bağlantıyı öğrenmesi için **Random Forest** modeli tercih edilmiştir.
* Ayrıca Random Forest modeli karmaşık ilişkileri ve doğrusal olmayan yapıları rahatça yakalar. Birden fazla karar ağacı kullanılarak overfitting (aşırı öğrenme) durumunu en aza indirir.
* Kullanılan bu modelde 100 adet karar ağacı kullanılmıştır. Ve her denemede aynı sonucu vemesi için random_state parametresi 42 olarak belirlenmiştir.
* Modeli eğitirken sonuca etki edecek kolonlar şunlardır: *arr_delay, sched_dep_time_clean, sched_arr_time_clean, air_time, day_of_week, carrier_encoded, origin_encoded, dest_encoded, distance*
* Modelin sonuç kolonu ise *dep_delay*'dir.
* Veri seti %80 eğitim (train), %20 test olmak üzere ayrılmıştır.

## Model Değerlendirme
* Modelin performansını değerlendirmek için 4 adet değerlendirme metrikleri kullanılmıştır.
* **MAE (Mean Absolute Error):** Tahminlerin ortalama mutlak hatası.
* **MSE (Mean Squared Error):** Hataların karelerinin ortalaması.
* **RMSE (Root Mean Squared Error):** MSE’nin karekökü, hata birimini aynı tutar.
* **R² Score (Determination Coefficient):** Modelin açıklayıcılık oranı.
* Bu değerlendirme metriklerine göre sonuçlarımız:
  * MAE: 7.229883387936477,
  * MSE: 149.18471977081978,
  * RMSE: 12.214119688738103,
  * R² Score: 0.947966803206839 gelmiştir.
