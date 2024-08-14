# OgrenciVeritabaniYonetimSistemi
Staj dersi kapsamında talep edilen projede, temel CRUD işlemlerinin olduğu sorgular yer almaktadır.

# Veritabanı Projesi Dokümantasyonu
## 1. Giriş
Bu doküman, bir öğrenci bilgi sistemi için geliştirilen SQL veritabanı yapısını ve temel sorguları detaylandırmaktadır. Proje, öğrencilerin ve ilgili fakülteler, dersler ve notlar ile ilgili verilerin yönetimi için gerekli olan veritabanı tablolarını ve sorgularını içerir. Doküman, veritabanı yapısını ve bu yapıya nasıl veri ekleneceğini açıklayan adımları içerir.
## 2. Veritabanı Şeması
Veritabanı dört ana tablo içermektedir: Fakülteler, Öğrenciler, Dersler ve Notlar. Bu tablolar, öğrenci yönetim sistemi için gerekli olan tüm temel bilgileri saklamaktadır.
### 2.1 Fakülteler Tablosu
Amaç: Üniversite bünyesindeki fakültelerin bilgilerini saklamak.
    • FakulteID (INT, PRIMARY KEY): Fakültenin benzersiz kimlik numarası.
    • FakulteAd (VARCHAR(100), NOT NULL): Fakültenin adı.

CREATE TABLE IF NOT EXISTS Fakülteler (
    FakulteID INT PRIMARY KEY,
    FakulteAd VARCHAR(100) NOT NULL
);
### 2.2 Öğrenciler Tablosu
Amaç: Öğrenci bilgilerini saklamak.
    • OgrenciNo (INT, PRIMARY KEY): Öğrencinin benzersiz numarası.
    • İsim (VARCHAR(50), NOT NULL): Öğrencinin adı.
    • Soyisim (VARCHAR(50), NOT NULL): Öğrencinin soyadı.
    • FakulteID (INT, FOREIGN KEY): Öğrencinin bağlı olduğu fakültenin kimlik numarası, Fakülteler tablosuna referans verir.

CREATE TABLE IF NOT EXISTS Öğrenciler (
    OgrenciNo INT PRIMARY KEY,
    İsim VARCHAR(50) NOT NULL,
    Soyisim VARCHAR(50) NOT NULL,
    FakulteID INT,
    FOREIGN KEY (FakulteID) REFERENCES Fakülteler(FakulteID)
);
### 2.3 Dersler Tablosu
Amaç: Üniversitedeki derslerin bilgilerini saklamak.
    • DersKodu (INT, PRIMARY KEY): Dersin benzersiz kodu.
    • Dersİsmi (VARCHAR(100), NOT NULL): Dersin adı.

CREATE TABLE IF NOT EXISTS Dersler (
    DersKodu INT PRIMARY KEY,
    Dersİsmi VARCHAR(100) NOT NULL
);
### 2.4 Notlar Tablosu
Amaç: Öğrencilerin derslerden aldığı notları saklamak.
    • OgrenciNo (INT, PRIMARY KEY, FOREIGN KEY): Notu alan öğrencinin numarası, Öğrenciler tablosuna referans verir.
    • DersKodu (INT, PRIMARY KEY, FOREIGN KEY): Notun ait olduğu dersin kodu, Dersler tablosuna referans verir.
    • Vize (INT): Vize sınavı notu.
    • Final (INT): Final sınavı notu.
    • İlişkiler: Öğrenci silindiğinde ona ait tüm notlar da otomatik olarak silinir (ON DELETE CASCADE).

CREATE TABLE IF NOT EXISTS Notlar (
    OgrenciNo INT,
    DersKodu INT,
    Vize INT,
    Final INT,
    PRIMARY KEY (OgrenciNo, DersKodu),
    FOREIGN KEY (OgrenciNo) REFERENCES Öğrenciler(OgrenciNo) ON DELETE CASCADE,
    FOREIGN KEY (DersKodu) REFERENCES Dersler(DersKodu)
);
## 3. Veri Eklemeleri
Bu bölüm, yukarıda tanımlanan tablolara örnek veri eklemelerini içerir.

-- Fakülteler Tablosuna Veri Eklemeleri
INSERT INTO Fakülteler (FakulteID, FakulteAd) VALUES (1, 'İşletme');
INSERT INTO Fakülteler (FakulteID, FakulteAd) VALUES (2, 'Hukuk');
INSERT INTO Fakülteler (FakulteID, FakulteAd) VALUES (3, 'Tıp');

-- Öğrenciler Tablosuna Veri Eklemeleri
INSERT INTO Öğrenciler (OgrenciNo, İsim, Soyisim, FakulteID) VALUES (201, 'Ece', 'Kaya', 1);
INSERT INTO Öğrenciler (OgrenciNo, İsim, Soyisim, FakulteID) VALUES (202, 'Burak', 'Yıldız', 2);
INSERT INTO Öğrenciler (OgrenciNo, İsim, Soyisim, FakulteID) VALUES (203, 'Zeynep', 'Demir', 3);

-- Dersler Tablosuna Veri Eklemeleri
INSERT INTO Dersler (DersKodu, Dersİsmi) VALUES (101, 'Mikro İktisat');
INSERT INTO Dersler (DersKodu, Dersİsmi) VALUES (102, 'Anayasa Hukuku');
INSERT INTO Dersler (DersKodu, Dersİsmi) VALUES (103, 'Anatomi');

-- Notlar Tablosuna Veri Eklemeleri
INSERT INTO Notlar (OgrenciNo, DersKodu, Vize, Final) VALUES (201, 101, 85, 90);
INSERT INTO Notlar (OgrenciNo, DersKodu, Vize, Final) VALUES (202, 102, 75, 80);
INSERT INTO Notlar (OgrenciNo, DersKodu, Vize, Final) VALUES (203, 103, 65, 70);
## 4. Örnek Sorgular
Bu bölümde, veritabanı üzerinde yapılabilecek çeşitli sorgular ve işlemler örneklerle gösterilmektedir.
### 4.1 Tüm Öğrencilerin İsim, Soyisim ve Fakülte Adını Listeleme
Bu sorgu, öğrencilerin isimlerini, soyisimlerini ve fakülte adlarını listeler.
SELECT Öğrenciler.İsim, Öğrenciler.Soyisim, Fakülteler.FakulteAd
FROM Öğrenciler
JOIN Fakülteler ON Öğrenciler.FakulteID = Fakülteler.FakulteID;
### 4.2 Belirli Bir Ders Koduna Sahip Öğrencileri Listeleme
Bu sorgu, belirli bir dersi alan öğrencilerin isimlerini ve soyisimlerini listeler.
SELECT Öğrenciler.İsim, Öğrenciler.Soyisim, Dersler.Dersİsmi
FROM Notlar
JOIN Öğrenciler ON Notlar.OgrenciNo = Öğrenciler.OgrenciNo
JOIN Dersler ON Notlar.DersKodu = Dersler.DersKodu
WHERE Dersler.DersKodu = 101;
### 4.3 Öğrencilerin Ortalama Notlarını Hesaplama
Bu sorgu, her öğrencinin ders notlarının ortalamasını hesaplar.
SELECT Öğrenciler.İsim, Öğrenciler.Soyisim, 
       AVG((Notlar.Vize * 0.4) + (Notlar.Final * 0.6)) AS OrtalamaNot
FROM Notlar
JOIN Öğrenciler ON Notlar.OgrenciNo = Öğrenciler.OgrenciNo
GROUP BY Öğrenciler.İsim, Öğrenciler.Soyisim;
### 4.4 En Yüksek Final Notuna Sahip Öğrenciyi Bulma
Bu sorgu, final notu en yüksek olan öğrenciyi listeler.
SELECT Öğrenciler.İsim, Öğrenciler.Soyisim, Notlar.Final
FROM Notlar
JOIN Öğrenciler ON Notlar.OgrenciNo = Öğrenciler.OgrenciNo
ORDER BY Notlar.Final DESC
LIMIT 1;
### 4.5 Belirli Bir Öğrencinin Aldığı Tüm Dersleri ve Notları Listeleme
Bu sorgu, belirli bir öğrencinin aldığı dersleri ve notlarını listeler.
SELECT Dersler.Dersİsmi, Notlar.Vize, Notlar.Final
FROM Notlar
JOIN Dersler ON Notlar.DersKodu = Dersler.DersKodu
WHERE Notlar.OgrenciNo = 201;
### 4.6 Bir Fakültedeki Tüm Öğrencilerin Ortalama Notlarını Hesaplama
Bu sorgu, belirli bir fakülteye bağlı tüm öğrencilerin ortalama notlarını fakülte bazında gruplar.
SELECT Fakülteler.FakulteAd, Öğrenciler.İsim, Öğrenciler.Soyisim, 
       AVG((Notlar.Vize * 0.4) + (Notlar.Final * 0.6)) AS OrtalamaNot
FROM Notlar
JOIN Öğrenciler ON Notlar.OgrenciNo = Öğrenciler.OgrenciNo
JOIN Fakülteler ON Öğrenciler.FakulteID = Fakülteler.FakulteID
WHERE Fakülteler.FakulteID = 1
GROUP BY Fakülteler.FakulteAd, Öğrenciler.İsim, Öğrenciler.Soyisim;
## 5. Veri Güncelleme ve Silme
Bu bölüm, mevcut verileri güncellemek veya silmek için kullanılan SQL komutlarını içerir.
### 5.1 Öğrenci Bilgilerini Güncelleme
Belirli bir öğrencinin soyadını güncellemek için kullanılan sorgu.
UPDATE Öğrenciler
SET Soyisim = 'Yılmaz'
WHERE OgrenciNo = 201;
### 5.2 Öğrenci Kayıtlarını Silme
Belirli bir öğrenciyi ve bu öğrenciye ait tüm notları silen sorgu.
DELETE FROM Öğrenciler
WHERE OgrenciNo = 202;
## 6. Sonuç
Bu doküman, öğrenci bilgi sistemi için gerekli olan veritabanı tasarımını ve temel SQL sorgularını kapsamaktadır. Bu yapı, sistemin genişletilebilir ve sürdürülebilir bir veri yönetim çözümü sunmasına olanak tanır. Veritabanı, gelecekteki ihtiyaçlara göre uyarlanabilir ve geliştirilmesi kolaydır.
