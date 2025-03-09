# Basics of C-Sharp

## **1. Input & Output (I/O)**
**Penjelasan:**  
- **Input:** Menggunakan `Console.ReadLine()` untuk membaca input dari pengguna.  
- **Output:** Menggunakan `Console.WriteLine()` untuk menampilkan teks ke konsol.  

**Contoh Kode:**  
```csharp
Console.Write("Masukkan nama Anda: ");
string name = Console.ReadLine(); // Membaca input user

Console.WriteLine($"Halo, {name}! Selamat datang di training C#.");
```

**Best Practices:**  
✅ Gunakan **pesan yang jelas** saat meminta input.  
✅ Gunakan **interpolasi string** (`$""`) untuk meningkatkan **readability**.  

## **2. Syntax Dasar (Comments, Variabel, Casting)**

### **2.1 Comments** 
**Penjelasan:**  
Komentar digunakan untuk menambahkan penjelasan dalam kode agar lebih mudah dipahami. Ada dua jenis komentar:  
- **Single-line comments** → menggunakan `//`  
- **Multi-line comments** → menggunakan `/* */`  

**Contoh Kode:**  
```csharp
// Ini adalah komentar satu baris

/*
   Ini adalah komentar multi-baris
   Biasanya digunakan untuk dokumentasi atau menjelaskan kode yang kompleks.
*/
```

**Best Practices:**  
✅ Gunakan komentar **untuk menjelaskan alasan di balik kode**, bukan hanya mendeskripsikan apa yang kode lakukan.  
✅ Jangan berlebihan menggunakan komentar, pastikan kode cukup **self-explanatory**.  
✅ Untuk dokumentasi function/class, gunakan XML comments:  
```csharp
/// <summary>
/// Fungsi ini menghitung luas persegi panjang.
/// </summary>
/// <param name="panjang">Panjang dari persegi panjang</param>
/// <param name="lebar">Lebar dari persegi panjang</param>
/// <returns>Luas dalam bentuk integer</returns>
int HitungLuas(int panjang, int lebar) 
{
    return panjang * lebar;
}
```

---

### **2.2 Variabel**  
**Penjelasan:**  
Variabel digunakan untuk menyimpan data. Ada dua cara deklarasi:  
1. **Tipe eksplisit**: Menentukan tipe secara langsung (`int angka = 10;`)  
2. **Tipe inferensi (`var`)**: Tipe ditentukan otomatis berdasarkan nilai awal (`var angka = 10;`)  

**Contoh Kode:**  
```csharp
// Tipe eksplisit
int usia = 25;
double tinggi = 1.75;
string nama = "Budi";

// Tipe inferensi
var alamat = "Jakarta";  // Secara otomatis dianggap string
var suhu = 30.5;         // Secara otomatis dianggap double
```

**Best Practices:**  
✅ Gunakan **camelCase** untuk variabel (`usia`, `namaLengkap`).  
✅ Hindari penggunaan `var` kecuali tipe datanya jelas.  
✅ Beri nama variabel yang **deskriptif**, jangan pakai `x`, `y`, `data1`.  

🚫 **Buruk**:  
```csharp
var x = 10; // Tidak jelas apa maksud dari x
```
✅ **Baik**:  
```csharp
var jumlahPeserta = 10; // Lebih jelas
```

---

### **2.3 Tipe Data**  
**Penjelasan:**  
Tipe data menentukan jenis nilai yang bisa disimpan dalam variabel. Beberapa tipe data utama di C#:  
- **Numerik**: `int`, `double`, `float`, `decimal`  
- **Boolean**: `bool` (true/false)  
- **Karakter & String**: `char`, `string`  

**Contoh Kode:**  
```csharp
int jumlah = 100;         // Integer
double suhu = 36.5;       // Double (desimal)
bool isActive = true;     // Boolean
char inisial = 'A';       // Karakter
string nama = "Andi";     // String
```

**Best Practices:**  
✅ Gunakan **tipe data yang sesuai**, jangan pakai `double` kalau cukup pakai `int`.  
✅ Gunakan **string interpolation** untuk gabungan teks daripada concatenation (`+`).  

🚫 **Buruk**:  
```csharp
string pesan = "Halo " + nama + ", selamat datang!";
```
✅ **Baik**:  
```csharp
string pesan = $"Halo {nama}, selamat datang!";
```

---

### **2.4 Casting (Konversi Tipe Data)**  
**Penjelasan:**  
- **Implicit Casting**: Otomatis dilakukan saat konversi aman (`int → double`).  
- **Explicit Casting**: Harus dilakukan secara manual (`double → int`).  
- **Konversi dengan `Convert.ToX()`**: Untuk mengonversi antara tipe data yang berbeda.  

**Contoh Kode:**  
```csharp
// Implicit Casting (otomatis)
int angka = 10;
double angkaDouble = angka;  // Tidak perlu casting manual

// Explicit Casting (manual)
double pi = 3.14;
int piBulat = (int)pi;  // Memotong angka desimal

// Konversi dengan Convert
string angkaStr = "123";
int angkaDariString = Convert.ToInt32(angkaStr);
```

**Best Practices:**  
✅ Gunakan **explicit casting** saat ada kemungkinan kehilangan data.  
✅ Gunakan `Convert.ToX()` untuk konversi dari string ke tipe lain agar **menghindari error parsing**.  
✅ Selalu gunakan pengecekan validasi sebelum konversi string ke angka.  

🚫 **Buruk**:  
```csharp
int nilai = int.Parse("abc"); // Akan error jika input bukan angka
```
✅ **Baik**:  
```csharp
int nilai;
if (int.TryParse("abc", out nilai))
{
    Console.WriteLine($"Konversi berhasil: {nilai}");
}
else
{
    Console.WriteLine("Input tidak valid!");
}
```

## **3. Strings**  
**Penjelasan:**  
- **Concatenation (`+`)**: Menggabungkan string secara manual.  
- **Interpolation (`$""`)**: Menyisipkan variabel dalam string.  
- **Access String**: Mengakses karakter dengan indeks (`str[index]`).  
- **Escape Character**: `\n` (new line), `\t` (tab), `\\` (backslash).  

**Contoh Kode:**  
```csharp
string firstName = "John";
string lastName = "Doe";

// Concatenation
string fullName = firstName + " " + lastName;

// Interpolation
string greeting = $"Halo, {fullName}!";

// Mengakses karakter pertama
char firstLetter = fullName[0];

Console.WriteLine(greeting);
```

**Best Practices:**  
✅ Gunakan **interpolasi string** (`$""`) daripada concatenation untuk **readability**.  
✅ Hindari **string hardcoded**, gunakan **konstanta** jika perlu.  

## **4. Operator & Ekspresi**  
**Penjelasan:**  
- **Aritmatika (`+`, `-`, `*`, `/`, `%`)**  
- **Perbandingan (`==`, `!=`, `<`, `>`, `<=`, `>=`)**  
- **Logika (`&&`, `||`, `!`)**  

**Contoh Kode:**  
```csharp
int a = 10, b = 20;
bool isGreater = a > b;
bool isValid = (a < b) && (b != 0);

Console.WriteLine($"Apakah a lebih besar dari b? {isGreater}");
Console.WriteLine($"Apakah pernyataan valid? {isValid}");
```

**Best Practices:**  
✅ Gunakan **spasi di sekitar operator** untuk meningkatkan **readability**.  
✅ Hindari penggunaan **magic numbers**, gunakan **konstanta** jika memungkinkan.  

## **5. Control Flow (if, switch, loops)**  
**Penjelasan:**  
- **If-Else:** Percabangan logika.  
- **Switch:** Alternatif if-else untuk kondisi banyak.  
- **Loops:** `for`, `while`, `do-while` untuk perulangan.  

**Contoh Kode:**  
```csharp
int score = 85;

// If-Else
if (score >= 90)
{
    Console.WriteLine("Grade: A");
}
else if (score >= 80)
{
    Console.WriteLine("Grade: B");
}
else
{
    Console.WriteLine("Grade: C");
}

// Switch
switch (score / 10)
{
    case 9:
    case 10:
        Console.WriteLine("Excellent!");
        break;
    case 8:
        Console.WriteLine("Good Job!");
        break;
    default:
        Console.WriteLine("Keep Trying!");
        break;
}

// Loop
for (int i = 1; i <= 5; i++)
{
    Console.WriteLine($"Iteration ke-{i}");
}
```

**Best Practices:**  
✅ Gunakan **switch-case** untuk kondisi banyak.  
✅ Gunakan **break** untuk keluar dari loop saat kondisi sudah terpenuhi.  
✅ Jangan lupa menambahkan **default case** di switch-case.  

## **6. Functions & Parameter**  
**Penjelasan:**  
- **Function (Method):** Blok kode yang dapat dipanggil ulang.  
- **Parameter:** Input ke function.  
- **Return Value:** Mengembalikan hasil.  

**Contoh Kode:**  
```csharp
// Function dengan parameter dan return value
int Tambah(int a, int b)
{
    return a + b;
}

// Function tanpa return (void)
void SapaPengguna(string nama)
{
    Console.WriteLine($"Halo, {nama}!");
}

// Panggil function
int hasil = Tambah(10, 5);
Console.WriteLine($"Hasil penjumlahan: {hasil}");

SapaPengguna("Andi");
```

**Best Practices:**  
✅ Gunakan **PascalCase** untuk nama function (`Tambah`, `SapaPengguna`).  
✅ Buat function **hanya melakukan satu tugas** untuk maintainability.  
✅ Jika function tidak mengembalikan nilai, gunakan **void** dengan deskriptif.  