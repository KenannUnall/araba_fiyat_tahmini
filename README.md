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


ilan_id = Araç ilanının id değerini belirtmektedir.
ilan_basligi = Aracın ilan başlığıdır.
fiyat = Tahmin edilecek değerdir. Bağımlı değişken olarak da adlandırılabilir.
fiyat_kuru = Fiyatın hangi para birimine denk olduğunu belirtmektedir.
ilan_tarihi = Aracın ilana koyulduğu tarihini (Yıl-Ay-Gün Saat) belirtmektedir.
ilan_kategorisi = Aracın türünü, markasını, modelini, silindir hacmini vb. özellikleri içerir.
arac_taglari = Aracın modelini, markasını, ve seri özelliklerini içerir.
ilan_konumu = Aracın bulunduğu, ilanın verildiği il/ilçe değerini içerir.
arac_ozellikleri = Aracın km değerini, hangi yıla ait model olduğunu vb. özelliklerini içerir.
Bu kısımda df.info() ile boş değer var mı, kolonların veri tipleri vb. özellikleri inceliyoruz. Ardından df.nunique() ile her bir kolon için kaç adet farklı değer olduğunu elde ediyoruz. Bu işlem örneğin fiyat_kuru kolonunun her veri de aynı olmasından dolayı öznitelik olarak önemli bir anlam taşımadığını yani herhangi bir veriye veya veri grubuna özgü olmadığından modelin öğrenme aşamasında önemli bir rol oynamayacaktır. Bu yüzden dolayı ilerleyen aşamalarda bu kolon kaldırılacaktır. df.isnull().sum() ile hiçbir sütunda boş veri olmadığı görülmektedir. Tabi ilerleyen aşamalarda json formattaki verilerin ayırılma işleminden sonra boş değer oluşabilme olasılığından dolayı bu aşama tekrarlanacaktır. 
 
Bu kod parçası ile Json formattaki kolonların ayrıştırma işlemi gerçekleştirilmektedir. Birinci olarak ilan_kategorisi sütununda işlem gerçekleştirilmektedir. İlk aşama json formattaki veriler ast.literal_eval(i) ile Python veri yapısına dönüştürülmektedir. Bu kısımda Auid A3 modelli araçlarda ayrıştırma aşamasında kayma gerçekleştiğinden dolayı ‘A3’ etiketi satir_labels listesine eklenmemektedir. Ardından sırası ile arac_tagleri ve arac_ozellikleri sütunlarındaki veriler ast.literal_eval(i) ile Python veri yapısına dönüştürülüp listeye  eklenmektedir. Bu listeye eklenen öznitelikler daha sonra dataframe formatına çevrilerek gerçek veri seti ile axis=1 doğrultusunda concat işlemleri gerçekleştirilecektir.

 

Listelerde yer alan öznitelikleri incelemek için her 3 listenin de ilk 5 değeri ekrana yazdırılmıştır. 
 



Bu kısımda ilk olarak dataframelerin birleştirilmesi ve kolon isimlerinin isimlendirilmesi yapılmaktadır. Birleştirme axis=1 şeklinde yani dikey de yan yana bir birleştirme şeklinde olacaktır.  İlk kolon çıkarımı "ilan_basligi","ilan_konumu","ilan_id","fiyat_kuru","ilan_kategorisi","arac_tagleri","arac_ozellikleri" adlı kolonlar olmuştur. Çıkarılma nedenleri herhangi bir anlam taşımamaları veya üzerlerinden öznitelik çıkarımı yaptıktan sonra o kolonlara gerek kalmamasıdır. Bu kısımda marka kolonunda boş değerler oluşmakta olup tespit edildikten sonra tüm boş marka değerleri ‘Renault’ olduğu görülmüş ve ‘Renault’ değeri ile doldurulmuştur. Son kısımda Model kolonundan silindir hacim sayısal değerleri alınmıştır. Bu aşamada Auid markalı araçlarda bu silindir hacim değerleri diğer türlere göre değişik olduğundan dolayı diğer türdeki markaların silindir hacim değerlerine benzetilme işlemi gerçekleştirilmiştir.
 

Daha sonra birbirleri ile aynı değerlere sahip olan kolonların kaldırılma işlemi gerçekleştirilmiştir. İlan tarihi kolonu yıl-ay-gün formatına çevrilmiştir. ‘km’ kolonu sayısal değere çevrileceğinden dolayı ilk olarak sayısal olmayan değerler kaldırılmış daha sonrasında float veri tipine dönüştürülmüştür. Aynı işlem yil_model içinde gerçekleştirilmiştir. Son olarak json formatındaki verilerin ayrıştırılması sırasında meydana gelen bazı kaymaların gerçek değerleri atanmış ve sonrasında float veri tipine dönüştürülmüştür.
 





parse_power() fonksiyonu, motor_gucu(hp) değerlerin sayısal formata(float) dönüştürülmesi  için değerleri uygun formata getirir. Ardından az sayıda bulunan değerlerin veri setinden atılma işlemi gerçekleştirilmiştir. Ve son olarak anlamsız kolon isimleri daha anlamlı isimler ile değiştirilmiştir.
 
Elde edilen veri setinin ilk 5 örneği ve örneklerin değerleri aşağıdaki gibidir.
 
Aşağı kısımda veri setinin kolon özellikleri ve boş değerleri incelenmiştir.
 
VERİ SETİNİN GÖRSELLEŞTİRİLMESİ
 
 
 
 
 
 
 
 
 

AYKIRI DEĞERLERİN KALDIRILMASI 
Bu kısımda aykırı değer sadece fiyat kolonu üzerinde uygulanmıştır. Aykırı değer alt ve üst limitleri grafikler incelenerek manuel olarak seçilmiştir. Alt değer min 100000 iken üst değer olarak 2900000 olarak seçilmiştir. 
 
 
 
 




YENİ ÖZNİTELİK OLUŞTURMA
Bu kısımda 3 adet yeni öznitelik oluşturulmuştur. 
Beygir Gücü = motor gücü / silindir hacmi
Araç Yaşı = Bulunduğumuz yıl (2023) – yil_model
Yıpranma Oranı = km / (araç yaşı + 0.1) 
Burada yıl olarak tahminin yapılacağı yıl alınmaktadır. datatime adlı kütüphane ile şuan da bulunduğumuz yılı aldık. Yıpranma oranının hesaplanmasında 0.1 eklenmesinin nedeni paydanın 0 yani sıfır bir araç geldiğinde yıpranma oranının değeri sonsuz olmasıdır. 
 

ONE-HOT ENCODİNG İŞLEMİ
İlk olarak veri setimizi bağımsız değişken(X), bağımlı değişken(Y) şeklinde ayırıyoruz ve ardından bağımısız değişkenlere one-hot encoding işlemi uyguluyoruz ve kategorik değişkenleri sayısal hale getiriyoruz. Bu aşamada one-hot encoding uygulama nedenimiz belirtilen kolondaki kategorik değerlerin birbirleri üzerinde bir büyüklük(sıralama) olmaması. İşlem sonucunda bağımsız değişkenlerimiz 27 kolona çıkmıştır.
 







KORELASYON MATRİSİ
 
Elde ettiğimiz özniteliklerin korelasyon değerlerine baktığımızda aslında marka değerlerine, araç yaşına ihtiyacımız olmadığını görmekteyiz. Çünkü korelasyonu yüksek olduğu kolonlar bulunmaktadır.
 

 
Bu öznitelikler kaldırıldıktan sonra korelasyon matrisinin son hali alttaki görseldeki gibidir.
 




Ardından bağımsız değişkenlerden de bu öznitelikler kaldırılmıştır ve bağımsız değişkenlerin son hali aşağıdaki gibidir.
 

SCALE İŞLEMİ
Bu kısımda, X ve Y değeri numpy dizisine çevrilmiş ve ardından scale işlemi gerçekleştirilmiştir. 
Scale İşlemi (Ölçeklendirme): Scale işlemi, veri noktalarını belirli bir aralığa dönüştürmek için kullanılan bir veri ön işleme tekniğidir. Genellikle veri setinin farklı özelliklerinin farklı ölçeklerde olması durumunda kullanılır. Örneğin, bir özellik 0-1 aralığında diğer özellik ise 100-1000 aralığında olabilir. Bu durumda scale işlemi kullanılarak her iki özelliği aynı ölçekte temsil etmek mümkündür.
Min-Max Scale Yöntemi: Min-Max scale yöntemi, scale işlemi için yaygın olarak kullanılan bir yöntemdir. Bu yöntemde, veri noktaları belirli bir aralığa dönüştürülür. Genellikle 0-1 aralığı tercih edilir.
 

TRAIN – TEST SPLIT
Bu kısımda test_size, veri setindeki veri sayısının yüksek olmasından dolayı 0.1 olarak belirlenmiştir.
Train set shape - X_train: (500013, 20)  Y_train: (500013, 1) 
Test set shape - X_test: (55557, 20)  Y_test: (55557, 1)

 



MAKİNE ÖĞRENMESİ ALGORİTMALARI VE ELDE EDİLEN SONUÇLAR
İlk aşamada kullanılacak olan algoritmaları ve metrikleri projemize dahil ediyoruz. Toplam olarak 6 adet algoritma kullanılmış ve sonuçlar elde edilmiştir.
  

Lineer Regresyon
Lineer regresyon, bağımlı ve bağımsız değişkenler arasındaki doğrusal ilişkiyi modelleyen bir istatistiksel yöntemdir. Bu yöntem, veri noktalarını bir doğru üzerinde en iyi şekilde temsil etmek için kullanılır. Bağımlı değişken, tahmin etmek istediğimiz değişkeni temsil ederken, bağımsız değişkenler ise tahmin etmeye yardımcı olan değişkenleri temsil eder. Lineer regresyon, veri analizi, tahmin yapma ve ilişkileri anlama gibi birçok uygulamada kullanılır.
 
 

Ridge Regresyon
Ridge regresyon, lineer regresyonu geliştiren bir modeldir ve aşırı uyum sorununu azaltmak için katsayıları sınırlayan bir düzenlendirme terimi kullanır. Bu yöntem, modelin genelleme yeteneğini artırır ve daha istikrarlı tahminler yapmayı sağlar.
 
 

Lasso Regresyon
Lasso regresyonu, lineer regresyonu geliştiren bir modeldir ve aşırı uyum sorununu çözmek ve değişken seçimini gerçekleştirmek için katsayıları sınırlayan bir düzenlendirme terimi kullanır. Bu yöntem, modelin daha basit ve yorumlanabilir olmasını sağlar ve anlamsız değişkenleri çıkarabilir.
 
 

Random Forest Regresyon
Random forest regresyonu, özellikle regresyon problemleri için etkili bir yöntemdir. Birden çok ağacın birleştirilmesi, tek bir ağaca kıyasla daha yüksek bir tahmin doğruluğu sağlar. Ayrıca, bu yöntem aşırı uyum sorununa karşı dirençlidir ve genellikle iyi genelleştirme performansı gösterir.
 
 




KNN Regresyon
KNN regresyonunda, veri setindeki örnekler arasındaki benzerlik ölçüsü kullanılır. Tahmin yapılacak yeni bir örnek geldiğinde, bu örneğe en yakın K sayıda komşu seçilir. Komşuların hedef değişken değerleri kullanılarak, tahmin edilmek istenen örneğin hedef değeri hesaplanır. Genellikle komşuların hedef değerlerinin ortalaması veya ağırlıklı ortalaması kullanılır.
KNN regresyonunda, K değeri modelin karmaşıklığını belirler. Küçük K değerleri modeli daha esnek hale getirirken, büyük K değerleri modeli daha az esnek hale getirir. K değeri seçimi, genellikle deneme yanılma yoluyla veya çapraz doğrulama teknikleriyle yapılır.
 
 

Gradient Boosting Regresyon
Gradient boosting regresyonu, zayıf tahmin modellerini bir araya getirerek daha güçlü bir tahmin modeli oluşturan bir regresyon yöntemidir. Bu yöntemde, başlangıçta basit bir tahmin modeli oluşturulur, genellikle bir karar ağacıdır. Ardından, her bir sonraki tahmin modeli, önceki modelin hatalarını düzeltmek için eğitilir. Bu düzeltmeler, gradyan (gradient) adı verilen bir değerle belirlenir. Gradyan, gerçek değerlerle tahmin değerleri arasındaki farkı ifade eder.
 
 



ÖNERİ SİSTEMİ
Öneri sisteminin yapılması için tkinter kütüphanesi kullanılmıştır. data isimli veri setinin tüm kolonları kullanıcıdan alınmamaktadır. Bazı değerler kullanıcıdan alınan değerlere göre arka plan da hesaplanmaktadır. Burada araba modelleri, yakit türleri vb. değerler radio_button ile alınırken km, yil_model gibi değerler textbox’a girilen değeri almaktadır.
 
 
 
 









           
