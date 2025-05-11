# Müşteri Segmentasyonu ve Kredi Limiti Analizi 

Bu proje, bir bankanın müşteri verilerini kullanarak müşteri segmentasyonu gerçekleştirmeyi ve her bir segment için ortalama kredi limitlerini analiz etmeyi amaçlamaktadır. Müşteriler, gelir kategorisi, kart türü, eğitim seviyesi, yaş grubu ve cinsiyet gibi çeşitli demografik ve finansal özelliklerine göre gruplandırılmıştır.

## Projenin Amacı

- Müşteri davranışlarını ve özelliklerini daha iyi anlamak.
- Farklı müşteri segmentlerini tanımlamak.
- Her bir müşteri segmentinin ortalama kredi limitini belirlemek.
- Bu bilgileri kullanarak daha hedefli pazarlama stratejileri geliştirmek ve müşteri ilişkileri yönetimini iyileştirmek.

## Kullanılan Veri Seti

Bu analiz için `BankChurners.csv` adlı veri seti kullanılmıştır. Veri seti, müşteri demografik bilgilerini, hesap bilgilerini ve kredi kartı kullanım alışkanlıklarını içermektedir.

## Metodoloji

1.  **Veri Yükleme ve Hazırlık:**
    * Gerekli Python kütüphaneleri (Pandas, NumPy, Matplotlib, Seaborn) import edildi.
    * `BankChurners.csv` dosyası bir Pandas DataFrame'e yüklendi.
    * Gereksiz sütunlar (proje bağlamında `CLIENTNUM` ve son iki sütun gibi) veri setinden çıkarıldı.

2.  **Özellik Mühendisliği ve Müşteri Seviyesi Belirleme:**
    * Müşterileri daha detaylı segmentlere ayırmak için yeni bir `Müşteri_Seviye_Belirleme` özelliği oluşturuldu. Bu özellik, aşağıdaki mevcut özelliklerin birleştirilmesiyle elde edildi:
        * `Income_Category` (Gelir Kategorisi)
        * `Card_Category` (Kart Kategorisi)
        * `Education_Level` (Eğitim Seviyesi)
        * `Customer_Age` (Müşteri Yaşı - yaş aralıklarına bölündü: 18-30, 31-40, 41-50, 51-60, 60+)
        * `Gender` (Cinsiyet)
    * Örneğin, bir müşteri için `Müşteri_Seviye_Belirleme` değeri "$40K - $60K_Blue_Graduate_41-50_M" şeklinde olabilir.

3.  **Ortalama Kredi Limiti Hesaplaması:**
    * Her bir benzersiz `Müşteri_Seviye_Belirleme` grubu için ortalama `Credit_Limit` (Kredi Limiti) hesaplandı.

4.  **Müşteri Segmentasyonu:**
    * Hesaplanan ortalama kredi limitlerine göre müşteriler dört farklı segmente (A, B, C, D) ayrıldı. Bu segmentasyon, `pd.qcut` fonksiyonu kullanılarak kantillere (çeyrekliklere) dayalı olarak yapıldı:
        * **D Segmenti:** En düşük ortalama kredi limitine sahip %25'lik dilim.
        * **C Segmenti:** %25 ile %50 arasındaki ortalama kredi limitine sahip dilim.
        * **B Segmenti:** %50 ile %75 arasındaki ortalama kredi limitine sahip dilim.
        * **A Segmenti:** En yüksek ortalama kredi limitine sahip %25'lik dilim.
    * Bu segmentasyon, `segment` adlı yeni bir DataFrame'de `Müşteri_Seviye_Belirleme`, `Ortalama_Kredi_Limiti` ve `Müşteri_Segmenti` sütunlarını içerecek şekilde saklandı.

## Bulgular (Örnek)

Analiz sonucunda, belirli müşteri profillerinin hangi kredi limiti segmentine dahil olduğu görülebilir. Örneğin:

| Müşteri\_Seviye\_Belirleme             | Ortalama\_Kredi\_Limiti | Müşteri\_Segmenti |
| :------------------------------------- | :---------------------- | :---------------- |
| $80K - $120K\_Silver\_High School\_51-60\_M | 34516.0                 | A                 |
| ...                                    | ...                     | ...               |

Bu çıktı, farklı müşteri profillerinin banka için ne kadar değerli olabileceğine dair bir gösterge sunar ve bu segmentlere özel stratejiler geliştirilmesine olanak tanır.

## Kurulum ve Çalıştırma

1.  Bu repoyu klonlayın:
    ```bash
    git clone https://github.com/BlackRazor34/Bank_Customer_Segmentation.git
    cd Bank_Customer_Segmantation
    ```
2.  Gerekli kütüphanelerin kurulu olduğundan emin olun. Eğer kurulu değilse, yükleyebilirsiniz:
    ```bash
    pip install pandas numpy matplotlib seaborn jupyter
    ```
3.  `customer.ipynb` dosyasını Jupyter Notebook veya JupyterLab ile açın.
4.  Hücreleri sırayla çalıştırarak analizi tekrarlayabilirsiniz. `BankChurners.csv` dosyasının notebook ile aynı dizinde olduğundan emin olun.

## Kullanılan Teknolojiler

* Python 3.11.9
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Jupyter Notebook
