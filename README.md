SQL Proje Raporu

Bu rapor, SirketDB veritabanı üzerinde gerçekleştirilen bir SQL projesini detaylandırmaktadır. 
Proje, belirli gereksinimlere göre saklı prosedürler, görünümler ve fonksiyonlar oluşturmayı içermektedir. 
Aşağıda proje gereksinimleri ve bu gereksinimlere yönelik çözümler detaylı olarak açıklanmıştır.

Proje Gereksinimleri ve Çözümler
1. Pozisyonunda müdür kelimesi geçen ve herhangi bir departmanda çalışan personelin Ad, Soyad, Maaş, Pozisyon ve Departman bilgilerini sorgulayan saklı prosedür oluşturulması.

Çözüm:

``````sql
CREATE PROCEDURE GetManagerEmployees
AS 
BEGIN 
    SELECT Ad, Soyad, Maas, Pozisyon, Departman
    FROM Personel 
    WHERE Pozisyon LIKE '%müdür%' 
    AND Departman IS NOT NULL;
END
``````

2. Ortalamanın üstünde maaş alan personelin Ad, Soyad, Departman, Pozisyon ve Maaş bilgilerini sorgulayan saklı prosedür oluşturulması.

Çözüm:

``````sql
CREATE PROCEDURE GetAboveAverageSalaryEmployees 
AS 
BEGIN
    SELECT Ad, Soyad, Departman, Pozisyon, Maas 
    FROM Personel 
    WHERE Maas > (SELECT AVG(Maas) FROM Personel);
END
``````

3. Müşterilerin Ad, Soyad, Cep Numarası, Vergi Dairesi, Vergi No, IBAN ve Sektörlerini View olarak kaydedilmesi. Otomotiv sektöründe olanları sorgulama.

Çözüm:

``````sql
CREATE VIEW OtomotivCustomers
AS 
SELECT Ad, Soyad, CepNumarasi, VergiDairesi, VergiNo, IBAN, Sektor
FROM Musteriler 
WHERE Sektor = 'Otomotiv'
``````

4. tUrunStok tablosunda belirli bir fiyatın üstünde olan ürünleri sayan bir fonksiyon tanımlanması.

Çözüm:

``````sql
CREATE FUNCTION CountProductsAbovePrice(@Price FLOAT)
RETURNS INT 
AS
BEGIN
    RETURN (SELECT COUNT(*) 
            FROM UrunStok 
            WHERE Fiyat > @Price); 
END
``````

5. Çocuğu olan personel için zam alabilir, diğerleri için alamaz yazdıran bir sorgunun oluşturulması. Personelin Ad, Soyad, Çocuk Sayısı ve Maaş bilgileri ile sorgulama. Maaş kolonuna göre en yüksekten düşüğe sıralama.

Çözüm:

``````sql
SELECT Ad, Soyad, CocukSayisi, Maas, 
    CASE
        WHEN CocukSayisi > 0 THEN 'Zam Alabilir'
        ELSE 'Zam Alamaz'
    END AS ZamDurumu
FROM Personel 
ORDER BY Maas DESC
``````

Sonuç

Bu proje, SirketDB veritabanı üzerindeki belirli gereksinimlere yönelik çeşitli SQL çözümlerini içerir. Saklı prosedürler, görünümler ve fonksiyonlar kullanılarak veritabanı yönetimi ve sorgulama işlemleri gerçekleştirilmiştir. Bu çözümler, veritabanı kullanıcılarına istenen bilgileri etkin ve doğru bir şekilde sağlar.










