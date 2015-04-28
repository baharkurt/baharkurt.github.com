---
layout: post
title: ANDROID SÖZLÜK UYGULAMASI       
---


Android sözlük uygulamasının yapılabilmesi için kelimelerin tutulacağı bir vertabanına ihtiyaç vardır.
SQLite açık kaynak kodlu bir  veritabanıdır. SQLite her android cihazı içerisinde gömülüdür. SQLite veritabanını kullanırken veritabanı için kurulum gerektirmez.

Sadece veritabanını oluştururken ve update işlemi yaparken SQL sorgusu yazılır. Diğer işlemler Android platformu tarafından otomatik olarak yapılır.

Veritabanı oluşturmak için tavsiye edilen yöntem SQLiteOpen Helper sınıfından türeyen yeni bir sınıf oluşturmaktır. SQLiteOpenHelper sınıfını kullandığınızda database oluşturma ve güncelleme işlemlerinin doğru zamanlarda ve doğru şekilde yapılması işini sisteme devredilmiş olur .Bu sınıfın sıkça kullanılan  iki metodu vardır.

`onCreate`: Veritabanı ilk oluşturulduğu anda çağrılan bir fonksiyondur. Burada tablo  oluşturma işlemleri yerine getirilir. 

`onUpgrade`: Uygulamanın farklı bir sürümü geliştirildiğinde veritabanı yapısında da güncelleme yapılabilir. Güncelleme sırasında bu metod çağrılır. Bu metod içinde tabloları drop etme(silme) ve yeniden oluşturma işlemleri yapılabilir. 
Uygulama kodları adım adım aşağıda verlmiştir.



### 1.UYGULAMA VERİTABANININ OLUŞTURULMASI

----------

Öncelikle uygulamanın database kısmını oluşturmak için SQLiteOpenHelper sınıfından DatabaseHelper isimli yeni bir sınıf extend edilir.



<img src ="http://i.hizliresim.com/vEg9DD.png">

1.satıra kadar olan bölümde  veritabanı adı, tablo adı,kolon isimleri ve veritabanı versiyonu için birer static sabit tanımı yapılır. 1.satırdaki sabit içerisinde de tablonun create sql cümleciği koyulur. Bu sql cümleciğinin, değer olarak aşağıdaki sql cümleciğinden hiçbir farkı yoktur.

    CREATE  DATABASE SOZLUK;
    USE SOZLUK;
    CREATE  TABLE KELIME(
    COLUNM_ID INTEGER PRIMARY KEY AUTOINCREMENT,
    AD  TEXT NOT NULL,
    ACIKLAMA TEXT);*



2.satırda, veri tabanı güncellemelerinde kullanılacak olan tablo silme cümlesini içeren bir sabit tanımı daha eklenir.3. satırda veritabanı adını ve versiyonunu içinde bulunduran bir constructor tanımı yapılır.

4.satırda daha öncede bahsettiğimiz gibi, veritabanı ilk oluşturulacağı zaman çağrılacaktır. Burada kelime tablosunu oluşturacak olan `DATABASE_CREATE ` sql ifadesini, SQLiteDatabase sınıfının `execSQL` metoduna argüman olarak verilir.

`execSQL` : Bu metod  verilen sql ifadesinin ilgili veritabanında çalıştırılmasını sağlamaktadır.

5.satırda veritabanı güncellemesi gerçekleştiğinde `DATABASE_DROP` sql cümlesi ile kelime tablosunun drop (silme) edilmesi  sağlanır.  Buradaki önemli nokta, eğer  tablodaki veriler web üzerinde saklanmıyorsa , tablo silindiğinde veriler kaybolacaktır.6. satırda `onCreate` metodu çağrılarak tablonun yeniden oluşması sağlanır.


Diğer activityleri anlatmadan önce database işlemlerinde datalar üzerinde gezinmeyi sağlayan bir işaretçi olan Cursor hakkında bilgi verilmiştir. Cursor insert(ekleme),update(güncelleme) ve delete (silme) işlemlerinde sıkça karşımıza çıkacaktır.

**Cursor**

Bir sorgulama yapıldığında size  bir sonuç kümesi döner. Kayıtların tamamını elde etmek yerine  kayıtlara işaret eden bir referans elde edilir. Bu işaretçi yardımıyla kayıtlar üzerinde tek tek gezinebilmekte, kayıtların başına ya da sonuna kolayca gidilebilmektedir. Sıkça kullanılan metodlar şunlardır:




- **moveToFirst() :**Cursor veri tabanındaki ilk konumu işaret eder.




- **moveToNext() :**Cursor bulunduğu konumdan bir sonraki konumu işaret eder.


- **isAfterLast() :**Son elemandan bir sonraki konumu işaret edip etmediğini gösterir.


- **getPosition():**Cursorun hangi pozisyonda olduğunu gösterir.


- **getCount():**Cursor’un işaret ettiği sonuç kümesinde kaç eleman olduğunu gösterir.




### 2.MAIN (LAUNCHER) ACTIVITY OLUŞTURULMASI

----------

Öncelikle uygulama başlatıldığında çalışacak tanımlamaların yapılıp metodların kullanılacağı MainActivity isimli bir activity oluşturulur.
Daha sonra bu activitye bağlı ve uygulamanın  kullanıcı arayüzlerinden birini (user interface) oluşturacak olan  main.xml dosyası içerisinde düzenlemeler yapılır.


<img src ="http://i.hizliresim.com/mPGBv0.png">


`main.xml` dosyası içersinde Türkçe ya da İngilizce kelime girilip çeviri sonucunun gösterilmesi için birer kontrol oluşturulur. İngilizce’den Türkçe’ye ve Türkçe’den  İngilizce’ye çeviri için birer adet buton kontrolü eklenir. Kelime hatırlatma işlemi ve `SecondActivity` ‘e geçiş için birer ImageButton kontrolü konulur. Bu  ImageButton iconlarına  ve daha fazlasına  [http://www.icons4android.com/](http://www.icons4android.com) adresinden ulaşabilirsiniz. Buton stillerini değiştirmek için ise res klasörü altındaki drawable dosyasına color_button adında bir xml sayfası eklenir. Bu xml sayfasının `<shape > </shape>` tagları  içerisinde aşağıda görüldüğü gibi buton  şekli ve renginde değişikler yapılabilir. 






<img src ="http://i.hizliresim.com/8gl49W.png">


Değişiklik yapılan buton için xml sayfası içerisine aşağıdaki kod eklenir.


    android:background="@drawable/color_button"

Uygulamada kullanılan çeşitli renk kodlarına  [http://www.color-hex.com/](http://www.color-hex.com) adresinden ulaşabilirsiniz.

    





`main.xml` dosyası içerindeki düzenlemeler tamalandıktan sonra  MainActivity sınıfı aşağıdaki  hale getirilir.

<img src ="http://i.hizliresim.com/YBvXNl.png">

1.bölümde görüldüğü üzere activity onCreate metodu çağrılarak başlatılır. Bu metod içerisinde activity’nin layout dosya ismi `setContentView` metodu ile belirtilir.Bu metod ile bu sınıf sayfanın olduğu xml dosyasına bağlanmış olur. 

2.bölümde öncelikle bir PendingIntent oluşturulur daha sonra  AlarmManager ile sistem servisi çağrılır. 

3.bölümde onCreate metodu içerisinde `aramaYap()` adında bir metod oluşturulur. Bu metod  sorgulama işlemi sırasında kullanılacaktır. Yine bu metod içerisinde 4 ve 6. kısımlarda görülen iki tane ImageButton  yaratılır. 

4.bölümde bulunan imageButton2 id’sine sahip butonun `onClick` metodu içerisinde bir `bilgileriGöster` adında bir metod çağrılır.Daha sonra 5.bölümde  bir Intent  ve PendingIntent nesnesi oluşturulur.PendingIntent farklı uygulamalara gönderilen sizin yerinize yapılması istenilen işlemleri içeren yapıdır. Burada PendingIntent  bir  Notification oluşturmayı sağlamıştır. Notification bildirim çubuğuna bildirim bırakmayı sağlayan yapıdır. Arkaplan servisi (background service) kullanılarak yapılır. 



- **getTicker:** Notification oluştuğu anda sistem barında görünecek olan başlıktır.


- **setContentTitle:** Sistem barı açıldığında text’i simgeleyen başlıktır.


- **setContentText:** Bildirim yazısıdır. Bu metoda bir değişken atayarak veritabanından bilgileriGöster() metodu ile üretilmiş random kelime görünecektir.


- **setSmallIcon**: Sistem barında Gnotification oluştuğunda başlık ile birlikte görünecek olan ikondur.	                       


- **setShowWhen:**Belirlenen bir zaman damgası olup olmadığını kontrol eder.


- **setContentIntent:** Bildirime tıklandığında gönderilecek olan intent burada belirtilir.
 Daha sonra tüm bu metodlar build edilir.

`FLAG_AUTO_CANCEL` flag kullanılarak açılan uyarının sistem barında otomatik olarak silinmesi sağlanır. Son olarak notify metodu ile notification gönderilir.  Gönderilen notification texti bilgileriGöster metodundan gelmektedir. Bu metoddan gelen hatirlatici değişkeni içerisinde Türkçe-İngilizce kelime çifti bulunmaktadır. Bu notification’ı tetiklemek için `imageButton2` id’li butona (zil ikonu) tıklanır.Her tıklamada eğer bir önceki notification silinmişse hatirlatici değişkeniyle random olarak kelime çiftleri gösterilir.

6.bölümde imageButton3  id’sine sahip butonun `onClick` metodu içerisinde bir intent  nesnesi oluşturulur. Intent parametreleri  içerisinde sırasıyla, bulunulan activity ile geçiş yapılacak olan activity belirtilir. `startActivity()` metoduna intent nesnesi referans verilerek  yeni activity başlatılmış olur.  Burada onClick metodunun  çalışmasıyla beraber SecondActivity’e geçilir. SecondActivity  sözlük veritabanına yeni kelime ekleme, kelime günceleleme ve silme işlemlerini barındırır.

7.satırda görüldüğü gibi `bilgileriGöster()` metodu yaratılmadan önce String veri tipinde sutunlar isimli bir dizi oluşturulur. Bu dizi elemanları database tablosundaki `COLUMN_AD` ve `COLUMN_ACIKLAMA` sutunlarından oluşmaktadır. `COLUMN_AD` alanı Türkçe kelimelerden oluşurken `COLUMN_ACIKLAMA` alanı İngilizce kelimelerden oluşmaktadır.

`bilgileriGöster()` metodu incelenirse:
8 numaralı satırda SQLiteDatabase sınıfından bir db nesnesi oluşturulur. Database’den okuma yapılacağı için   getReadableDatabase() metodu kullanılır. 

9.satırda ise bir Cursor nesnesi tanımlanır. Query metodu ile DatabaseHelper sınıfından tablo adı ,data çekilecek sütun (sutunlar  dizisi)ve datanın random olarak çekilmesi için gereken ifadeler kullanılmıştır. Veri çekme işlemi sırasında kullanılan bu ifadenin şablonu aşağıdaki gibi olmalıdır.

*`
SQLiteDatabase.query(String table, String[] columns, String selection, String[] selectionArgs, String groupBy, String having, String orderBy, String limit);`*

Daha sonra  Cursor nesnesi ile satır satır okuma yapılır. Bunun için 10. bölümde görüldüğü gibi  while içerisinde `moveToNext()` metodunu kullanılır. Okunan satırlar `COLUMN_AD`  ve `COLUNM_ACIKLAMA` Türkçe ve İngilizce olmak üzere iki değişkene atanır. Bu kelimeler value -keys  şeklinde  yazılacak şekilde string tipindeki ‘hatirlatma’ adındaki değişkenine atanır. Daha sonra bu ifade ekranda Toast mesajı olarak gösterilebilir. `imageButton2` id’li butona tıklandığında dizi uzunluğu kadar Toast mesajı random olarak üretilen Türkçe-İngilizce kelime çifti döndürerek veritabanında bulunan kelimeleri hatırlatmış olur.




<img src ="http://i.hizliresim.com/94VJ1o.png">



3.bölümde çağrılan `aramaYap()` metodu içerisinde Türkçe-ingilizce ve İngilizce-Türkçe çeviri yaparken kullanılacak metodlar tanımlanmıştır. Bu metod içerisinde arama yapılırken girilen sözcüğün yazılacağı ve sonucun yani çevirin görüleceği EditText nesneleri yaratılır. Daha sonra Türkçe ve İngilizce isminde iki tane buton nesnesi yaratılır. findViewById ile buton ve EditTextler’in xml dosyasındaki  id’leri belirtilerek bu nesnelerin layout dosyasında bağlanması sağlanır.

Türkçe ve İngilizce butonlarına yani searchButtonE ve searchButtonT nesnelerine tıklandığında bir listener çalışacaktır.

İngilizce butonun `onClick()` metodu içerisinde ilk satırda EditText’e girilen kelime search nesnesi ile ‘ad’ isminde string tipinde bir değişkene atanır. Daha sonra  getKelimeTür metodu bu değişkenreferans verilerek çağrılır.

Aynı işlemler Türkçe butonu için de yapılır.Farklı olarak `onCreate` metodu içerisinde getKelimeİng metodu ‘aciklama’ değişkeni referans veilerek çağrılır.
Veritabanı  tablosunda (kelime) `COLUMN_AD` kolonunda Türkçe kelimeler `COLUMN_ACIKLAMA` kolonunda İngilizce kelimeler bulunmaktadır.



Daha sonra gelen `getKelimeTür` metodu incelenirse:


<img src ="http://i.hizliresim.com/EYVajD.png">



Bu metod içerisinde  kullanıcı tarafından İngilizce bir kelime girildiğinde database’de bu kelimenin Türkçe karşılığı olup olmadığına bakılıp varsa karşılığının gösterilmesini sağlayan işlemler yapılmaktadır.

1.satırda Cursor nesnesi  ile `kelimeSorgula` metoduna daha önce bahsettiğimiz ad değişkeni referans verilerek çağrılır. Daha sonra if ile Cursor nesnesinin boş olup olmadığı kontrol edilerek boş ise bir Toast mesajı gönderilmesi sağlanır. Cursor nesnesi bir sonraki veriyi işaret eder.Daha sonra `COLUMN_ID` ile belirtilen kolon adı aracılığıyla ilgili kolonun index değeri döndürülür. Bir sonraki adımda  2. Sütunda bulunan bilgi alınarak sonuç gösterecek olan EditText’ e yazılır.


<img src ="http://i.hizliresim.com/a5oOkO.png">



Yukarıda verilen `kelimeSorgula`  metodu içerisinde 1.satırda belli bir kelime adına sahip satıra erişmek  için where clause kullanılır.Bu ifadede kullanılan “?”  buraya herhangi bir argümanın konulabileceği  anlamına gelmektedir. 

2.satırda bir üst satırda oluşturulan where cümlesi için sadece ad argümanını içeren string array(dizi) tanımlanır."where" clause içerisinde kaç tane parametre kullanılacaksa,bu array içinde her bir parametre için bir argüman tanımlanmalıdır.Parametrik yapı kullanmak yerine "whereArgs" olarak null verilerek,doğrudan where cümlesinin içine ad değeri  yazılabilir.Sorgulama yapıldıktan sonra girilen ad değerine sahip kelimeyi temsil eden bir cursor döndürülecektir.


`getKelimeİng` metodu incelenirse:


<img src ="http://i.hizliresim.com/62Z3RP.png">

Bu metod içerisinde  kullanıcı tarafından Türkçe bir kelime girildiğinde database’de bu kelimenin İngilizce karşılığı olup olmadığına bakılıp varsa karşılığının gösterilmesini sağlayan işlemler yapılmaktadır. `getKelimeTür` metodu ile aynı işlemler yapılır. Farklı olarak bu metod içerisinde `wordQuery` metodu çağrılır  ve 1. sütunda bulunan bilgi alınarak sonuç  gösterecek olan EditText’ e yazılır.


`getKelimeİng` metodu tarafından çağrılan `wordQuery` metodu aşağıdaki gibidir.

<img src ="http://i.hizliresim.com/j54lYm.png ">

Bu metod içerisinde 1. satırda belli bir kelime adına sahip satıra erişmek  için where clause kullanılır.Bu ifadede kullanılan “?”  buraya herhangi bir argümanın konulabileceği  anlamına gelmektedir.
 
2.satırda bir üst satırda oluşturulan "where" cümlesi için sadece aciklama argümanını içeren string array(dizi) tanımlanır."where" clause içerisinde kaç tane parametre kullanılacaksa,bu array içinde her bir parametre için bir argüman tanımlanmalıdır.Parametrik yapı kullanmak yerine "whereArgs" olarak null verilerek,doğrudan "where" cümlesinin içine aciklama değeri  yazılabilir.Sorgulama yapıldıktan sonra girilen aciklama değerine sahip kelimeyi temsil eden bir cursor döndürülecektir.





### 3. İKİNCİ ACTİVİTY (SECOND ACTIVITY) İÇERİSİNDE YAPILAN İŞLEMLER

----------

Daha önce anlattığımız gibi `MainActivity` sınıfı içerisindeki `imageButton3` id’li butonun onClick metodu içerisinden `SecondActivity` metodu çağrılmaktadır.  Öncelikle SecondActivity sınıfı oluşturulurken bu sınıfa  bağlı bir `second.xml` dosyası oluşturulur.


<img src ="http://i.hizliresim.com/QARbZv.png ">



`second.xml` dosyası içerisinde ad(Türkçe kelime) ve açıklamaların(İngilizce kelime) girilebileceği birer EditText kontrolü ve hemen altında kaydetme, güncelleme ve silme işlemlerinin yapılabileceği birer tane daha kontrol eklenir. Butonların hemen altında ise kelimelerin listeleneceği bir ListView oluşturulmuştur.




Daha sonra `list_item` adında  bir xml dosyası daha oluşturulur.ListView  içerisinde listeye eklenen kelimelerin görünümü bu xml dosyası içerisindeki ayarlar sayesinde gerçekleşir. Bu dosyanın içeriği aşağıdaki gibi düzenlenir.



<img src ="http://i.hizliresim.com/AmAB3r.png ">

Daha önceden oluşturulan `SecondActivity` sınıfı `MainAcivity `sınıfı içerisinde  `imageButton3` id’li butonun `onClick` metodu içerisinden çağrılmıştı. Bu sınıf içerinde gerçekleşecek işlemler aşağıda verilmiştir.

<img src ="http://i.hizliresim.com/YBvzND.png">

1.satırda yeni bir DatabaseHelper sınıfı oluşturulur. Veritabanı oluşturma ve güncelleme işlemlerini helper oluşturarak sisteme bırakılmış olunur.

2.bölümde kelime tablosundaki kolon adlarını içeren bir string array değişkeni tanımlanır. Bu sorgulama esnasında kullanılacaktır. İnt tipinde tanımlanan array değişkeni ise listeye ekleme sırasında kullanılacaktır.

3.satırda `onCreate` metodu içerisinden `ekranKontrolleriniOluştur()` metodu çağrılır.

<img src ="http://i.hizliresim.com/YBvzOa.png">


Bu kısımda `ekranKontrolleriniOlustur()` adında kendi  yarattığımız  bir constructor metod çağrılır. `ekranKontrolleriniOlustur()`  metodu içerisinde buton, listView ve editText’ler oluşturularak  veri girişinin yapılması  ve butona tıklama sonucunda çağrılacak metodlar ile  girilen verilerin listelenmesi için çağrılacak metodlar tanımlanır.


<img src ="http://i.hizliresim.com/kkjV6m.png">

Yukarıda görülen kodlar ile kelime listesini oluşturacak ListView tanımlanır. `onItemClickListener` metodu ile listede kelime seçme işlemlerinin işleyişi belirtilir.


<img src ="http://i.hizliresim.com/rQgP0M.png">


Daha sonraki kısımda `kelimeSorgula`  adında, String tipinde bir referansa sahip metod bulunmaktadır.
Bu metod içerisinde 1. satırda belli bir kelime adına sahip satıra erişmek  için where clause kullanılır.Bu ifadede kullanılan “?”  buraya herhangi bir argümanın konulabileceği  anlamına gelmektedir. 

2.satırda bir üst satırda oluşturulan where cümlesi için sadece ad argümanını içeren string array(dizi) tanımlanır."where" clause içerisinde kaç tane parametre kullanılacaksa,bu array içinde her bir parametre için bir argüman tanımlanmalıdır.Parametrik yapı kullanmak yerine "whereArgs" olarak null verilerek,doğrudan "where" cümlesinin içine ad değeri  yazılabilir.Sorgulama yapıldıktan sonra girilen ad değerine sahip kelimeyi temsil eden bir cursor döndürülecektir.



<img src ="http://i.hizliresim.com/vEgMNv.png">

1.satırda `kelimeSorgula` metodu kullanılarak Cursor nesnesi elde edilir.

2.satırda if kontolünden sonra Cursor’un kaç tane elemanı içeren bir kümeye sahip olduğu `getCount` metodu ile elde edilir.Sorgulama yapılan cursor işaretçisi listedeki  elemanlardan ilkinin bir adım gerisinde durur. Yani ilk elemanı işaret etmez. 

3.satırda `moveToNext` metodu çağrıldığında ilk elemana erişilmiş olur. Bütün kelime listesi okunduğunda cursor bir adım ilerletmeden kayıt okunamaz ve hata alınır.moveToNext metodu başarıyla işaretçiyi hareket ettirirse true döner.

4.satırda id kolonunun index değeri `getColoumnIndex `fonksiyonu yardımıyla okunur. Bunun yapılmasının sebebi  kolon içeriklerine adlarıyla değil index değerleriyle erişmek zorunda olmaktır.İlgili kolon index değeri bulunamazsa  -1 değeri  döner.

5.satırda getInt metoduna bir üst satırda elde edilen index değeri verilerek daha önceden `INTEGER `olarak tanımlanan `COLUNM_ID` kolonunun değeri elde edilir.


**Content Values:**

Query işlemi için Cursor sınıfına ihtiyaç duyduğumuz gibi,ekleme(insert) ve güncelleme(update) işlemleri için de ContentValues sınıfını kullanmamız gerekecektir. Content values nesnesinin tek bir  tablo satırını temsil ettiği düşünülürse  bu nesneye her bir kolon için bir anahtar/değer(key /value ) çifti  eklenerek tablodaki bir satır oluşturulur.Sonra  bu nesne kullanılarak ilgili satır tabloya eklenir.

> **INSERT (EKLEME):**


<img src ="http://i.hizliresim.com/vEgM24.png">

Insert etmek istenilen kelime için content values nesnesi üzerinden kelimenin ad ve açıklama değerleri girilip insert metodu çağrılır ve eklenen satır id ‘si  sonuç olarak döndürülür.


> **UPDATE (GÜNCELLEME):**


<img src ="http://i.hizliresim.com/DMlnjm.png">


`kelimeEkle` metoduna benzer bir yapıya sahiptir. Tek fark güncellenecek olan satırın hangisi olduğunu bilmememiz gerekmektedir.Önce content values nesnesi  yardımı ile güncel satır değerleri oluşturulur.Sonra  `getKelimeId` metodu kullanılarak güncellemek istenilen ad değerine sahip satırın `COLUMN_ID`  kolonunun değeri elde edilir."where" cümleciğinde  bu kez doğrudan id değeri verildi.Bir sonraki argüman yani "whereArgs" için null kullanıldı.update fonksiyonu çağrıldığında güncelleme işlemi tamamlanmış olacaktır.


> **DELETE (SİLME):**

<img src ="http://i.hizliresim.com/zAmdX9.png">

Silme işlemi için sadece silmek istenilen satırın id’sinin bilinmesi yeterlidir.`kelimeGüncelle` metodunda oluğu gibi silinmesi istenilen satırın id değeri okunur.Daha sonra "where" ifadesi oluşturulur ve delete fonksiyonuna argüman olarak verilir.


Daha sonra Cursor veri tipinde bir değer döndüren `butunKelimeleriSorgula ` adında bir metod yaratılır. İlk satırda okunabilir veritabanı referansıelde edilir. 2. satırda daha önce bahsedilen şablona da uyarak bir query(sorgulama) işlemi yapılır. Bu metod kelime tablosunun tamamını temsil eden bir Cursor işaretçisi döndürecektir.Bu nesne üzerinde gezinerek satırlar tek tek okunabilir.



<img src ="http://i.hizliresim.com/rQgPPz.png">


Daha sonra `editTextGuncelle` metodu oluşturulur.

<img src ="http://i.hizliresim.com/Nq5z6N.png">


`editTextGüncelle` metodunda Cursor nesnesinden faydalanarak ad ve açıklama alanlarının değişmesi durumununun sağlanması tanımlaır. `listeGüncelle` metodunda ise EditText’te güncelenen kelimenin listeye aktarım işlemi yapılır.



### 4. MAIN VE SECOND ACTIVIY’E BAĞLI DİĞER SINIFLARIN OLUŞTURULMASI

----------


Uygulama kodlarımızın devamında `SelectableSimpleCursor ` sınıfı oluşturulur. Bu sınıf SimpleCursorAdapter sınıfından extend edilmiştir.

SimpleCursorAdapter  sınıfı datayı Content Provider(İçerik Sağlayıcılar) yardımıyla doldurmayı sağlamaktadır.Content Provider veritabanı iletişim arayüzü olarak düşünülebilir.

Bu sınıf içerisinde `SecondActivity` sınıfındaki ListView listesine insert (ekleme) işlemi ile bu listeye yeni kelime ekleme,update(güncelleme) işlemi ile listedeki kelimenin güncellenmesi ve delete(silme) işlemi ile listeden kelime silinmesi işlemleri gerçekleşir.

<img src ="http://i.hizliresim.com/g5k7nR.jpg">


### 5.NOTIFICATION OLUŞTURULMASI

----------

`MyAlarmService` adında bir servis  sınıfı oluşturulur.

Bu sınıf içerisinde uygulamaya sistem barında görünecek bir notification (uyarı) gönderme işlemi yapılmaktadır. 

Servis, arka planda çalışan ve kullanıcıyla her hangi etkileşime girmeyen bir bileşendir. Servisler veri kontrolu,intenet indirmeleri, veri işleme,içerik sağlayıcıları güncelleme gibi uzun vadede çalışan tekrarlı ve sürekli çalışmaya ihtiyaç duyan işlemler için kullanılır.

Bir servise arkaplanda çalışan bir activity ile aynı önceliği atamak mümkündür.Bu gibi durumda,ilgili servis için görünür bir notification gereklidir. İletişim için yayın alıcılar(Broadcast Receivers) kullanılabilir.Servis bir broadcast yayınlar ve activity içerisinde tanımlanmış broadcast receiver bu broadcast’i alır, işler ve bir sonuç döndürür.

Notification mesajlarından biri  olan “Daha iyi bir çeviri için katkıda bulunun.” mesajına sistem barında görünür. Tıklandığında ise `SecondActivity`'i çalıştırır.


<img src ="http://i.hizliresim.com/b5ln2b.jpg">




`onCreate()`: Bir servis ilk oluşturulduğunda, sistem bu metodu çağırır. Eğer servis zaten çalışıyorsa, bu metod çağrılmaz.

`onDestroy()`:Bir servis kullanılmadığında ve yok edildiğinde, sistem bu metodu çağırır. Servis thread, kayıtlı dinleyici, alıcı vb. sistem kaynaklarını boşaltmak için bu metodu kullanır.
Bir uygulama bileşeni `startService()` metodunu kullanarak bir hizmeti başlattığında, servis başlatılmış olur. Bir kez başlatıldıktan sonra, servisi başlatan bileşen yok edilse bile, servisler arka planda sonsuza kadar çalışabilir. 


Daha sonra arka planda çalışacak olan  servisin başlatılacağı `MyReceiver` adında bir sınıf oluşturularak  sınıfı BroadcastReceiver sınıfından extend edilir. Daha sonra bu sınıf içerisine onReceive metodu implement edilir. onReceive metodu iki parametre almaktadır.

1. ***Context:*** Context nesnesi ekstra bilgiye veya bir servisi veya activity ‘ i başlatmak için kullanılabilir.
2. **Intent:**  Intent nesnesi receiver’ in register olduğu action ile ilişkilidir.

Intent nesnesi ile  `MyAlarmService` sınıfına gidilir ve servis başlatılır.

<img src ="http://i.hizliresim.com/n7AXMN.png">


### 6. UYGULAMA GÖRSELLERİ

----------


<img src ="http://i.hizliresim.com/XvBV2O.png">

<img src ="http://i.hizliresim.com/ME6Nl7.png">

<img src ="http://i.hizliresim.com/vEgkQD.png">

<img src ="http://i.hizliresim.com/b5lqjY.png">

<img src ="http://i.hizliresim.com/8glqlr.png">






### *PROJE KÜNYESİ*

----------

<img src ="http://i.hizliresim.com/vEgkJ4.png">


**Okul :** Ondokuzmayıs Üniversitesi , SAMSUN    

**Fakülte:** Mühendislik Fakültesi

**Bölüm:** Bilgisayar Mühendisliği Bölümü                          

**Proje Adı :** Mobil Programlama Dersi Vize Projesi

**Ders Danışmanı :** Öğr.Gör.Dr. İsmail İŞERİ


----------

**Hazırlayan :** Büşra Bahar KURT - Bilgisayar Mühendisliği /3.Sınıf

**e-posta  adresi :** bahar.kurt@bil.omu.edu.tr

**Tarih:** 28 / 04 / 2015
