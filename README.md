<h1>VERİSETİ YÜKLEME VE VERİ ÖN İŞLEME</h1>
<p>
Projemize ilk olarak, her aşamada kullanabileceğimiz, tüm veri ön işleme, makine öğrenmesi vb. alanlarda oldukça popüler olarak kullanılan kütüphanelerimizi dahil ederek başlıyoruz. Bu kütüphaneleri kısaca açıklamak gerekirse;
<ul>
 <li>NumPy = Python için bir bilimsel hesaplama kütüphanesidir. Çok boyutlu dizilerin hızlı işlenmesini sağlar ve matematiksel operasyonlar için kullanılır.</li>
<li>Pandas= Python için geliştirilmiş bir veri analizi ve manipülasyon kütüphanesidir. Veri tablolarını ve zaman serilerini işlemek için kullanılır. Pandas, veri okuma, filtreleme, sıralama, birleştirme, gruplama gibi işlemleri kolayca gerçekleştirebilir ve veri analizi için gerekli olan birçok fonksiyonu içerir.</li>
<li>Matplotlib= Veri görselleştirme kütüphanesidir. Verileri grafikler, çizimler ve görsel sunumlar aracılığıyla görselleştirmek için kullanılır. Matplotlib, çizgi grafikleri, sütun grafikleri, dağılım grafikleri, histogramlar, pasta grafikleri ve daha birçok grafik türünü içinde barındırır.</li>
<li>Seaborn= Seaborn’ de Matplotlib gibi veri görselleştirme kütüphanesidir. Matplotlib’e göre daha spesifiktir. Seaborn, Matplotlib kütüphanesine göre daha yüksek seviyeli bir arayüz sunarak istatistiksel veri görselleştirmelerini kolaylaştırır.</li>
</ul>
Aşağı kısımda csv formatındaki veri setimizi df değişkenine atıyoruz. Daha sonra ilk 5 değeri inceliyoruz.

</p>

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/c7ecb8e0-2ef2-4c4a-91b0-b31d59f17e98)
 

Veri seti 556576 adet veri ve 9 kolon(öznitelik)’ den oluşmaktadır. Öznitelikler incelendiğinde 3 adet kolon(öznitelik) json formatında veri tutmaktadır. Yani içinde birçok öznitelik değerinde bilgi taşımaktadırlar.  İlerleyen aşamalarda bu json formatındaki verileri ayırma işlemi gerçekleştirilerek farklı öznitelikler elde dilecek ve öznitelik sayısı arttırılacaktır.

<p>
 
ilan_id = Araç ilanının id değerini belirtmektedir.<br>
ilan_basligi = Aracın ilan başlığıdır.<br>
fiyat = Tahmin edilecek değerdir. Bağımlı değişken olarak da adlandırılabilir.<br>
fiyat_kuru = Fiyatın hangi para birimine denk olduğunu belirtmektedir.<br>
ilan_tarihi = Aracın ilana koyulduğu tarihini (Yıl-Ay-Gün Saat) belirtmektedir.<br>
ilan_kategorisi = Aracın türünü, markasını, modelini, silindir hacmini vb. özellikleri içerir.<br>
arac_taglari = Aracın modelini, markasını, ve seri özelliklerini içerir.<br>
ilan_konumu = Aracın bulunduğu, ilanın verildiği il/ilçe değerini içerir.<br>
arac_ozellikleri = Aracın km değerini, hangi yıla ait model olduğunu vb. özelliklerini içerir.<br>
Bu kısımda df.info() ile boş değer var mı, kolonların veri tipleri vb. özellikleri inceliyoruz. Ardından df.nunique() ile her bir kolon için kaç adet farklı değer olduğunu elde ediyoruz. Bu işlem örneğin fiyat_kuru kolonunun her veri de aynı olmasından dolayı öznitelik olarak önemli bir anlam taşımadığını yani herhangi bir veriye veya veri grubuna özgü olmadığından modelin öğrenme aşamasında önemli bir rol oynamayacaktır. Bu yüzden dolayı ilerleyen aşamalarda bu kolon kaldırılacaktır. df.isnull().sum() ile hiçbir sütunda boş veri olmadığı görülmektedir. Tabi ilerleyen aşamalarda json formattaki verilerin ayırılma işleminden sonra boş değer oluşabilme olasılığından dolayı bu aşama tekrarlanacaktır. 
 
</p>

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/de13629a-acb7-40a3-b6fa-acf19c8445b5)


Aşağdaki kod parçası ile Json formattaki kolonların ayrıştırma işlemi gerçekleştirilmektedir. Birinci olarak ilan_kategorisi sütununda işlem gerçekleştirilmektedir. İlk aşama json formattaki veriler ast.literal_eval(i) ile Python veri yapısına dönüştürülmektedir. Bu kısımda Auid A3 modelli araçlarda ayrıştırma aşamasında kayma gerçekleştiğinden dolayı ‘A3’ etiketi satir_labels listesine eklenmemektedir. Ardından sırası ile arac_tagleri ve arac_ozellikleri sütunlarındaki veriler ast.literal_eval(i) ile Python veri yapısına dönüştürülüp listeye  eklenmektedir. Bu listeye eklenen öznitelikler daha sonra dataframe formatına çevrilerek gerçek veri seti ile axis=1 doğrultusunda concat işlemleri gerçekleştirilecektir.

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/5d62bd10-f96d-4cac-9b05-df366578f473)


Listelerde yer alan öznitelikleri incelemek için her 3 listenin de ilk 5 değeri ekrana yazdırılmıştır. 
 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/ec9ee27e-fb86-4a11-b8a4-48d2617f8f52) 



Bu kısımda ilk olarak dataframelerin birleştirilmesi ve kolon isimlerinin isimlendirilmesi yapılmaktadır. Birleştirme axis=1 şeklinde yani dikey de yan yana bir birleştirme şeklinde olacaktır.  İlk kolon çıkarımı "ilan_basligi","ilan_konumu","ilan_id","fiyat_kuru","ilan_kategorisi","arac_tagleri","arac_ozellikleri" adlı kolonlar olmuştur. Çıkarılma nedenleri herhangi bir anlam taşımamaları veya üzerlerinden öznitelik çıkarımı yaptıktan sonra o kolonlara gerek kalmamasıdır. Bu kısımda marka kolonunda boş değerler oluşmakta olup tespit edildikten sonra tüm boş marka değerleri ‘Renault’ olduğu görülmüş ve ‘Renault’ değeri ile doldurulmuştur. Son kısımda Model kolonundan silindir hacim sayısal değerleri alınmıştır. Bu aşamada Auid markalı araçlarda bu silindir hacim değerleri diğer türlere göre değişik olduğundan dolayı diğer türdeki markaların silindir hacim değerlerine benzetilme işlemi gerçekleştirilmiştir.
 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/6434e2e3-f258-4c75-ba0c-e32987d20802) 


Daha sonra birbirleri ile aynı değerlere sahip olan kolonların kaldırılma işlemi gerçekleştirilmiştir. İlan tarihi kolonu yıl-ay-gün formatına çevrilmiştir. ‘km’ kolonu sayısal değere çevrileceğinden dolayı ilk olarak sayısal olmayan değerler kaldırılmış daha sonrasında float veri tipine dönüştürülmüştür. Aynı işlem yil_model içinde gerçekleştirilmiştir. Son olarak json formatındaki verilerin ayrıştırılması sırasında meydana gelen bazı kaymaların gerçek değerleri atanmış ve sonrasında float veri tipine dönüştürülmüştür.
 
 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/98e7b894-2ab3-445f-9a73-919ff6c51a1a)




parse_power() fonksiyonu, motor_gucu(hp) değerlerin sayısal formata(float) dönüştürülmesi  için değerleri uygun formata getirir. Ardından az sayıda bulunan değerlerin veri setinden atılma işlemi gerçekleştirilmiştir. Ve son olarak anlamsız kolon isimleri daha anlamlı isimler ile değiştirilmiştir.

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/a776916e-bad7-41da-8991-753221c658f5) 

 
Elde edilen veri setinin ilk 5 örneği ve örneklerin değerleri aşağıdaki gibidir.

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/439cdee4-6f54-4b97-b993-bdaa6bc6abbb) 

 
Aşağı kısımda veri setinin kolon özellikleri ve boş değerleri incelenmiştir.

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/42d83c89-ebe6-487b-9b4d-0c2bed04ff2b) 

 
<h1>VERİ SETİNİN GÖRSELLEŞTİRİLMESİ</h1>
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/dfa97117-fcde-4387-9aca-45962ed58317)

 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/dd09bee7-8b64-4c99-b5dd-15519d5be218)

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/8cf2e514-d7c2-46ab-b62d-f6e83a4e5912)

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/0c86faa4-2793-4ae5-b091-a380dd19a3ba)

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/6da47b8f-3a86-4ae0-b635-b575fc4fb4cc)

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/980c8025-3c3f-44fb-b1e4-10cf1ca69b51)


![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/fba0cdf8-bfee-4172-9379-0d65edfde5ec)

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/b497b103-f3c0-4774-a642-2abc7e1c9985)
 

AYKIRI DEĞERLERİN KALDIRILMASI 
Bu kısımda aykırı değer sadece fiyat kolonu üzerinde uygulanmıştır. Aykırı değer alt ve üst limitleri grafikler incelenerek manuel olarak seçilmiştir. Alt değer min 100000 iken üst değer olarak 2900000 olarak seçilmiştir. 
 
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/8c5a45ce-a342-46c3-b88e-75fbcbc88440)

 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/870774df-dd28-4c77-8b33-2add78b9582e)


![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/06a1460f-78b2-43a5-a489-e80688e34cf8)

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/6a3329ca-8a71-4ffa-b3ca-5ceb4550dcc0)



YENİ ÖZNİTELİK OLUŞTURMA
Bu kısımda 3 adet yeni öznitelik oluşturulmuştur. 
Beygir Gücü = motor gücü / silindir hacmi
Araç Yaşı = Bulunduğumuz yıl (2023) – yil_model
Yıpranma Oranı = km / (araç yaşı + 0.1) 
Burada yıl olarak tahminin yapılacağı yıl alınmaktadır. datatime adlı kütüphane ile şuan da bulunduğumuz yılı aldık. Yıpranma oranının hesaplanmasında 0.1 eklenmesinin nedeni paydanın 0 yani sıfır bir araç geldiğinde yıpranma oranının değeri sonsuz olmasıdır. 
 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/66639ca8-9f44-4cbd-ad88-af33a84cf8c5)


<h1>ONE-HOT ENCODİNG İŞLEMİ</h1>
İlk olarak veri setimizi bağımsız değişken(X), bağımlı değişken(Y) şeklinde ayırıyoruz ve ardından bağımısız değişkenlere one-hot encoding işlemi uyguluyoruz ve kategorik değişkenleri sayısal hale getiriyoruz. Bu aşamada one-hot encoding uygulama nedenimiz belirtilen kolondaki kategorik değerlerin birbirleri üzerinde bir büyüklük(sıralama) olmaması. İşlem sonucunda bağımsız değişkenlerimiz 27 kolona çıkmıştır.
 


![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/d0a6d2ab-614c-4049-b847-75d389221f26)





<h1>KORELASYON MATRİSİ</h1>
 
Elde ettiğimiz özniteliklerin korelasyon değerlerine baktığımızda aslında marka değerlerine, araç yaşına ihtiyacımız olmadığını görmekteyiz. Çünkü korelasyonu yüksek olduğu kolonlar bulunmaktadır.

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/2190ba02-6acc-4870-9443-fd66503a003f)

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/d3c278e4-11ae-4a0c-93c0-467cd2effc70)

 
Bu öznitelikler kaldırıldıktan sonra korelasyon matrisinin son hali alttaki görseldeki gibidir.
 

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/bad6c23a-51e4-4ce7-ac29-e2d8e6e25910)



Ardından bağımsız değişkenlerden de bu öznitelikler kaldırılmıştır ve bağımsız değişkenlerin son hali aşağıdaki gibidir.

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/209b9494-22b6-4924-b7d6-60f3722dc80b)


<h1>SCALE İŞLEMİ</h1>
Bu kısımda, X ve Y değeri numpy dizisine çevrilmiş ve ardından scale işlemi gerçekleştirilmiştir. 
Scale İşlemi (Ölçeklendirme): Scale işlemi, veri noktalarını belirli bir aralığa dönüştürmek için kullanılan bir veri ön işleme tekniğidir. Genellikle veri setinin farklı özelliklerinin farklı ölçeklerde olması durumunda kullanılır. Örneğin, bir özellik 0-1 aralığında diğer özellik ise 100-1000 aralığında olabilir. Bu durumda scale işlemi kullanılarak her iki özelliği aynı ölçekte temsil etmek mümkündür.
Min-Max Scale Yöntemi: Min-Max scale yöntemi, scale işlemi için yaygın olarak kullanılan bir yöntemdir. Bu yöntemde, veri noktaları belirli bir aralığa dönüştürülür. Genellikle 0-1 aralığı tercih edilir.

 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/679a1162-933e-483d-b36d-fda6c6ae1325)


<h1>TRAIN – TEST SPLIT</h1>
Bu kısımda test_size, veri setindeki veri sayısının yüksek olmasından dolayı 0.1 olarak belirlenmiştir.
Train set shape - X_train: (500013, 20)  Y_train: (500013, 1) 
Test set shape - X_test: (55557, 20)  Y_test: (55557, 1)

 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/f493531f-0f71-4465-8ba8-8a858ec08623)



<h1>MAKİNE ÖĞRENMESİ ALGORİTMALARI VE ELDE EDİLEN SONUÇLAR</h1>
İlk aşamada kullanılacak olan algoritmaları ve metrikleri projemize dahil ediyoruz. Toplam olarak 6 adet algoritma kullanılmış ve sonuçlar elde edilmiştir.
  
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/4efd37db-cfd8-486a-9dd0-c6be0f311edd)


Lineer Regresyon
Lineer regresyon, bağımlı ve bağımsız değişkenler arasındaki doğrusal ilişkiyi modelleyen bir istatistiksel yöntemdir. Bu yöntem, veri noktalarını bir doğru üzerinde en iyi şekilde temsil etmek için kullanılır. Bağımlı değişken, tahmin etmek istediğimiz değişkeni temsil ederken, bağımsız değişkenler ise tahmin etmeye yardımcı olan değişkenleri temsil eder. Lineer regresyon, veri analizi, tahmin yapma ve ilişkileri anlama gibi birçok uygulamada kullanılır.
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/26417518-44fd-4e92-88e4-c53b8c43d71f)
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/ad6009b1-04d3-4bdb-901b-5c9dacf9803c)


Ridge Regresyon
Ridge regresyon, lineer regresyonu geliştiren bir modeldir ve aşırı uyum sorununu azaltmak için katsayıları sınırlayan bir düzenlendirme terimi kullanır. Bu yöntem, modelin genelleme yeteneğini artırır ve daha istikrarlı tahminler yapmayı sağlar.
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/3243fb70-881e-49b4-b80c-ee2f2cbc0107)
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/6ad53f16-f213-4039-a7b7-a83ffe9404eb)


Lasso Regresyon
Lasso regresyonu, lineer regresyonu geliştiren bir modeldir ve aşırı uyum sorununu çözmek ve değişken seçimini gerçekleştirmek için katsayıları sınırlayan bir düzenlendirme terimi kullanır. Bu yöntem, modelin daha basit ve yorumlanabilir olmasını sağlar ve anlamsız değişkenleri çıkarabilir.
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/b146eba0-eed6-48ec-bc3e-296120b5f22d)
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/17d3f229-01b5-4cd8-ae3f-ef69aea3f3bb)


Random Forest Regresyon
Random forest regresyonu, özellikle regresyon problemleri için etkili bir yöntemdir. Birden çok ağacın birleştirilmesi, tek bir ağaca kıyasla daha yüksek bir tahmin doğruluğu sağlar. Ayrıca, bu yöntem aşırı uyum sorununa karşı dirençlidir ve genellikle iyi genelleştirme performansı gösterir.
 
 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/cfa588e5-5ca3-4302-85bc-41142a9ded50)

![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/328d8aaa-bcf9-434c-a22d-1a9897d34b6f)




KNN Regresyon
KNN regresyonunda, veri setindeki örnekler arasındaki benzerlik ölçüsü kullanılır. Tahmin yapılacak yeni bir örnek geldiğinde, bu örneğe en yakın K sayıda komşu seçilir. Komşuların hedef değişken değerleri kullanılarak, tahmin edilmek istenen örneğin hedef değeri hesaplanır. Genellikle komşuların hedef değerlerinin ortalaması veya ağırlıklı ortalaması kullanılır.
KNN regresyonunda, K değeri modelin karmaşıklığını belirler. Küçük K değerleri modeli daha esnek hale getirirken, büyük K değerleri modeli daha az esnek hale getirir. K değeri seçimi, genellikle deneme yanılma yoluyla veya çapraz doğrulama teknikleriyle yapılır.
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/9ca1a5a0-0dfb-494a-8d99-bafa9ac222c1)
 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/605c6e56-3ef6-428a-9142-9305f769f8d3)


Gradient Boosting Regresyon
Gradient boosting regresyonu, zayıf tahmin modellerini bir araya getirerek daha güçlü bir tahmin modeli oluşturan bir regresyon yöntemidir. Bu yöntemde, başlangıçta basit bir tahmin modeli oluşturulur, genellikle bir karar ağacıdır. Ardından, her bir sonraki tahmin modeli, önceki modelin hatalarını düzeltmek için eğitilir. Bu düzeltmeler, gradyan (gradient) adı verilen bir değerle belirlenir. Gradyan, gerçek değerlerle tahmin değerleri arasındaki farkı ifade eder.
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/ff7779b1-237c-4a1a-9779-841d28a8d5cf)
 
![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/919e3482-53fb-455a-b16b-2fdb6d79d455)




ÖNERİ SİSTEMİ
Öneri sisteminin yapılması için tkinter kütüphanesi kullanılmıştır. data isimli veri setinin tüm kolonları kullanıcıdan alınmamaktadır. Bazı değerler kullanıcıdan alınan değerlere göre arka plan da hesaplanmaktadır. Burada araba modelleri, yakit türleri vb. değerler radio_button ile alınırken km, yil_model gibi değerler textbox’a girilen değeri almaktadır.
 
 
 ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/8f2f2aea-09fd-4c1d-acda-ee729b3b7609)             ![image](https://github.com/KenannUnall/araba_fiyat_tahmini/assets/83499398/26bcb294-9317-43bb-b7a3-dea7732bd3f7)

 









           
