# Collections and LINQ

## **1. Array, List, dan Dictionary di C#**  
C# punya beberapa struktur data untuk menyimpan koleksi data, tapi yang paling sering digunakan adalah **Array, List, dan Dictionary**.  

### **🔹 1.1. Array**  
Array adalah kumpulan elemen dengan ukuran tetap dan tipe data yang sama.  

📌 **Kelebihan:**  
✅ Cepat dalam akses indeks langsung.  
✅ Menggunakan memori lebih sedikit dibanding List atau Dictionary.  

📌 **Kekurangan:**  
❌ Ukurannya tetap, tidak bisa ditambah/dikurangi setelah dibuat.  
❌ Operasi seperti penambahan atau penghapusan elemen lebih sulit dilakukan.  

💻 **Contoh Kode:**  
```csharp
// Deklarasi array dengan ukuran tetap
int[] numbers = new int[5];  // {0, 0, 0, 0, 0}
numbers[0] = 10;
numbers[1] = 20;

// Deklarasi array langsung dengan nilai
string[] fruits = { "Apple", "Banana", "Cherry" };

// Looping array
foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```
  
🔍 **Best Practice untuk Array:**  
✅ Gunakan array jika jumlah elemen tetap dan tidak perlu diubah-ubah.  
✅ Hindari array jika sering menambah/menghapus data, gunakan **List<T>** sebagai gantinya.  

---  

### **🔹 1.2. List<T>**  
List<T> adalah koleksi dinamis yang mirip array, tapi ukurannya bisa bertambah dan berkurang.  

📌 **Kelebihan:**  
✅ Ukuran fleksibel, bisa bertambah atau berkurang.  
✅ Banyak metode bawaan seperti `Add()`, `Remove()`, `Contains()`, dll.  

📌 **Kekurangan:**  
❌ Akses indeks sedikit lebih lambat dibanding array karena ada overhead tambahan.  

💻 **Contoh Kode:**  
```csharp
// Deklarasi List
List<int> numbers = new List<int> { 1, 2, 3 };

// Menambahkan elemen
numbers.Add(4);
numbers.Add(5);

// Menghapus elemen
numbers.Remove(2);  // Menghapus angka 2
numbers.RemoveAt(0); // Menghapus elemen pertama (index 0)

// Looping List
foreach (var num in numbers)
{
    Console.WriteLine(num);
}
```
  
🔍 **Best Practice untuk List<T>:**  
✅ Gunakan **List<T>** jika data sering bertambah atau berkurang.  
✅ Gunakan **var** dengan bijak, misalnya untuk readability yang lebih baik:  
   ```csharp
   var names = new List<string> { "Alice", "Bob", "Charlie" };
   ```
✅ Hindari `.RemoveAt()` pada List besar karena operasinya O(n) (perlu shift elemen).  

---  

### **🔹 1.3. Dictionary<TKey, TValue>**  
Dictionary adalah koleksi pasangan **key-value**, berguna untuk pencarian cepat berdasarkan key.  

📌 **Kelebihan:**  
✅ Lookup cepat O(1) jika key unik.  
✅ Berguna untuk menyimpan data dengan key unik seperti ID, Nama, dsb.  

📌 **Kekurangan:**  
❌ Menggunakan lebih banyak memori dibanding List atau Array.  
❌ Key harus unik, tidak bisa ada duplikasi.  

💻 **Contoh Kode:**  
```csharp
// Deklarasi Dictionary
Dictionary<string, int> ages = new Dictionary<string, int>();

// Menambahkan elemen
ages["Alice"] = 25;
ages["Bob"] = 30;

// Mengakses elemen
Console.WriteLine($"Alice's age: {ages["Alice"]}");

// Mengecek apakah key ada sebelum akses
if (ages.ContainsKey("Charlie"))
{
    Console.WriteLine($"Charlie's age: {ages["Charlie"]}");
}
else
{
    Console.WriteLine("Charlie not found.");
}

// Looping Dictionary
foreach (var kvp in ages)
{
    Console.WriteLine($"{kvp.Key} is {kvp.Value} years old.");
}
```
  
🔍 **Best Practice untuk Dictionary<TKey, TValue>:**  
✅ Gunakan **Dictionary** jika butuh pencarian cepat berdasarkan key unik.  
✅ Selalu gunakan `ContainsKey()` sebelum mengakses key untuk menghindari `KeyNotFoundException`.  
✅ Gunakan **TryGetValue** jika sering mengakses key yang belum tentu ada:  
   ```csharp
   if (ages.TryGetValue("Charlie", out int age))
   {
       Console.WriteLine($"Charlie's age is {age}");
   }
   else
   {
       Console.WriteLine("Charlie not found.");
   }
   ```


### **🔹 Perbandingan Array, List, dan Dictionary**
| Struktur Data | Ukuran Dinamis? | Kecepatan Akses | Tambah/Hapus Mudah? | Keperluan |
|--------------|----------------|-----------------|-----------------|----------|
| **Array** | ❌ Tidak | ⚡ Cepat (O(1)) | ❌ Sulit | Data fix, tidak berubah |
| **List<T>** | ✅ Ya | 🔹 Sedikit lebih lambat dari array | ✅ Mudah | Data berubah-ubah dengan urutan |
| **Dictionary<TKey, TValue>** | ✅ Ya | ⚡ Cepat (O(1) untuk lookup) | 🔸 Key harus unik | Data dengan key unik untuk pencarian cepat |


## **🚀 Kesimpulan & Rekomendasi**  
- Gunakan **Array** jika data tetap dan perlu akses cepat.  
- Gunakan **List<T>** jika data berubah-ubah dan butuh fleksibilitas.  
- Gunakan **Dictionary<TKey, TValue>** jika perlu pencarian cepat berdasarkan key.  

## **2. Iterasi Koleksi dengan Loops**  

Ketika bekerja dengan **Array, List, dan Dictionary**, kita sering perlu melakukan iterasi atau looping untuk mengakses dan memproses elemen di dalamnya. Ada beberapa cara untuk melakukan iterasi di C#, yaitu:  

1️⃣ `for` loop  
2️⃣ `foreach` loop  
3️⃣ `while` loop  
4️⃣ `do-while` loop  
5️⃣ **LINQ** (akan dibahas di materi berikutnya)  

Kita bahas satu per satu dengan contoh kasusnya.  

---

### **🔹 2.1. for loop**  
Loop ini digunakan ketika kita butuh kontrol penuh atas indeks.  

📌 **Cocok digunakan untuk:**  
✅ Iterasi dengan indeks atau perlu akses ke elemen sebelum/sesudahnya.  
✅ Mengubah elemen berdasarkan indeks tertentu.  

💻 **Contoh Kasus: Cetak angka genap dari array**  
```csharp
int[] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

for (int i = 0; i < numbers.Length; i++)
{
    if (numbers[i] % 2 == 0)
    {
        Console.WriteLine($"Angka genap: {numbers[i]}");
    }
}
```

🔍 **Best Practice untuk `for` loop:**  
✅ Gunakan jika perlu akses indeks langsung.  
✅ Hindari menggunakan `for` jika hanya ingin membaca semua elemen—gunakan `foreach` agar lebih bersih.  

---

### **🔹 2.2. foreach loop**  
Loop ini digunakan untuk membaca elemen koleksi dengan lebih simpel tanpa perlu indeks.  

📌 **Cocok digunakan untuk:**  
✅ Membaca koleksi tanpa perlu mengubah elemen.  
✅ Iterasi di **List<T>** dan **Dictionary<TKey, TValue>**.  

💻 **Contoh Kasus: Looping List<T>**  
```csharp
List<string> names = new List<string> { "Alice", "Bob", "Charlie" };

foreach (var name in names)
{
    Console.WriteLine($"Hello, {name}!");
}
```

💻 **Contoh Kasus: Looping Dictionary**  
```csharp
Dictionary<string, int> ages = new Dictionary<string, int>
{
    { "Alice", 25 },
    { "Bob", 30 },
    { "Charlie", 22 }
};

foreach (var kvp in ages)
{
    Console.WriteLine($"{kvp.Key} berusia {kvp.Value} tahun.");
}
```

🔍 **Best Practice untuk `foreach` loop:**  
✅ Gunakan untuk **membaca** koleksi tanpa mengubahnya.  
✅ Jangan gunakan `foreach` jika perlu **mengubah** elemen dalam koleksi—gunakan `for` atau LINQ.  

---

### **🔹 2.3. while loop**  
Loop ini digunakan ketika jumlah iterasi **tidak diketahui sebelumnya** dan bergantung pada kondisi tertentu.  

📌 **Cocok digunakan untuk:**  
✅ Looping berdasarkan kondisi, bukan jumlah elemen.  
✅ Menunggu event atau input tertentu.  

💻 **Contoh Kasus: Input dari pengguna sampai valid**  
```csharp
string input;

do
{
    Console.Write("Masukkan nama (minimal 3 karakter): ");
    input = Console.ReadLine();
}
while (input.Length < 3);

Console.WriteLine($"Halo, {input}!");
```

🔍 **Best Practice untuk `while` loop:**  
✅ Gunakan jika iterasi bergantung pada kondisi yang bisa berubah runtime.  
✅ Pastikan ada kondisi berhenti agar tidak jadi **infinite loop**.  

---

### **🔹 2.4. do-while loop**  
Hampir sama dengan `while`, tapi **dijalankan minimal sekali**, bahkan jika kondisinya salah dari awal.  

📌 **Cocok digunakan untuk:**  
✅ Menjalankan perintah setidaknya **sekali**, lalu mengecek kondisi.  

💻 **Contoh Kasus: Konfirmasi keluar aplikasi**  
```csharp
string confirm;
do
{
    Console.Write("Apakah Anda ingin keluar? (y/n): ");
    confirm = Console.ReadLine().ToLower();
}
while (confirm != "y");

Console.WriteLine("Program selesai.");
```

🔍 **Best Practice untuk `do-while` loop:**  
✅ Gunakan jika perlu memastikan kode berjalan **minimal satu kali**.  
✅ Jangan gunakan jika kondisi mungkin false dari awal.  

## **🚀 Kesimpulan & Rekomendasi**  

| Loop | Kapan Digunakan? | Contoh Penggunaan |
|------|----------------|------------------|
| **for** | Jika butuh akses indeks | Mengubah elemen array |
| **foreach** | Jika hanya membaca data | Looping List atau Dictionary |
| **while** | Jika iterasi bergantung pada kondisi | Validasi input pengguna |
| **do-while** | Jika kode harus jalan minimal sekali | Konfirmasi keluar aplikasi |

## **3. LINQ Dasar (Where, Select, OrderBy)**  
LINQ (**Language Integrated Query**) adalah fitur di C# yang memungkinkan kita melakukan query terhadap koleksi data dengan cara yang lebih bersih dan efisien.  

Dengan LINQ, kita bisa:  
✅ Memfilter data dengan **Where**  
✅ Memilih atau memproyeksikan data dengan **Select**  
✅ Mengurutkan data dengan **OrderBy** dan **OrderByDescending**  

---

### **🔹 3.1. Where (Filter Data)**  
Digunakan untuk mengambil elemen yang memenuhi suatu kondisi tertentu.  

📌 **Cocok digunakan untuk:**  
✅ Menyaring data dari **List, Array, atau Database**.  
✅ Mengambil hanya data yang diperlukan tanpa perlu looping manual.  

💻 **Contoh Kasus: Ambil angka genap dari List**  
```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Filter angka genap
var evenNumbers = numbers.Where(n => n % 2 == 0);

foreach (var num in evenNumbers)
{
    Console.WriteLine(num);  // Output: 2, 4, 6, 8, 10
}
```

💻 **Contoh Kasus: Filter orang yang usianya di atas 25 tahun**  
```csharp
var people = new List<(string Name, int Age)>
{
    ("Alice", 25),
    ("Bob", 30),
    ("Charlie", 22)
};

// Ambil yang umurnya > 25
var filteredPeople = people.Where(p => p.Age > 25);

foreach (var person in filteredPeople)
{
    Console.WriteLine(person.Name);  // Output: Bob
}
```

🔍 **Best Practice untuk Where:**  
✅ Gunakan `Where()` untuk menyaring data secara langsung tanpa `if` dalam loop.  
✅ Jika hanya butuh **satu elemen**, gunakan `FirstOrDefault()` untuk efisiensi:  
   ```csharp
   var firstEven = numbers.FirstOrDefault(n => n % 2 == 0);
   ```

---

### **🔹 3.2. Select (Transformasi Data)**  
Digunakan untuk mengubah atau memproyeksikan elemen dalam koleksi menjadi bentuk lain.  

📌 **Cocok digunakan untuk:**  
✅ Mengambil hanya sebagian data dari objek.  
✅ Mengubah data ke format yang berbeda.  

💻 **Contoh Kasus: Ambil nama orang saja dari List**  
```csharp
var names = people.Select(p => p.Name);

foreach (var name in names)
{
    Console.WriteLine(name);  // Output: Alice, Bob, Charlie
}
```

💻 **Contoh Kasus: Ubah angka menjadi teks "Angka: X"**  
```csharp
var numberLabels = numbers.Select(n => $"Angka: {n}");

foreach (var label in numberLabels)
{
    Console.WriteLine(label);  
}
```

🔍 **Best Practice untuk Select:**  
✅ Gunakan `Select()` untuk mengambil hanya data yang diperlukan untuk efisiensi.  
✅ Hindari `Select()` jika tidak mengubah data—gunakan langsung tanpa transformasi.  

---

### **🔹 3.3. OrderBy dan OrderByDescending (Urutkan Data)**  
Digunakan untuk mengurutkan elemen berdasarkan nilai tertentu.  

📌 **Cocok digunakan untuk:**  
✅ Mengurutkan data berdasarkan angka, string, atau properti objek.  
✅ Membuat daftar lebih terstruktur dan mudah dibaca.  

💻 **Contoh Kasus: Urutkan angka dari kecil ke besar**  
```csharp
var sortedNumbers = numbers.OrderBy(n => n);

foreach (var num in sortedNumbers)
{
    Console.WriteLine(num);  // Output: 1, 2, 3, 4, 5, ..., 10
}
```

💻 **Contoh Kasus: Urutkan orang berdasarkan usia (Descending)**  
```csharp
var sortedPeople = people.OrderByDescending(p => p.Age);

foreach (var person in sortedPeople)
{
    Console.WriteLine($"{person.Name} - {person.Age}");
}
// Output:
// Bob - 30
// Alice - 25
// Charlie - 22
```

🔍 **Best Practice untuk OrderBy:**  
✅ Gunakan **OrderBy()** untuk urutan naik dan **OrderByDescending()** untuk urutan turun.  
✅ Jika perlu **lebih dari satu kriteria**, gunakan `ThenBy()`:  
   ```csharp
   var sortedPeople = people.OrderBy(p => p.Age).ThenBy(p => p.Name);
   ```


## **🚀 Kesimpulan & Rekomendasi**  

| LINQ Method | Kegunaan | Contoh |
|------------|---------|-------|
| **Where** | Filter data berdasarkan kondisi | `Where(n => n > 10)` |
| **Select** | Transformasi data | `Select(n => $"Angka: {n}")` |
| **OrderBy** | Urutkan dari kecil ke besar | `OrderBy(n => n)` |
| **OrderByDescending** | Urutkan dari besar ke kecil | `OrderByDescending(n => n)` |


## **4. Manipulasi Data dengan LINQ**  

Setelah memahami dasar LINQ seperti **Where**, **Select**, dan **OrderBy**, kita sekarang akan belajar bagaimana memanipulasi data menggunakan metode LINQ lainnya, seperti:  

✅ **GroupBy** → Mengelompokkan data  
✅ **Aggregate, Sum, Count, Average** → Perhitungan data  
✅ **Distinct** → Menghapus duplikasi  
✅ **Take & Skip** → Paging data  

---

### **🔹 4.1. GroupBy (Mengelompokkan Data)**  
Digunakan untuk mengelompokkan elemen dalam koleksi berdasarkan kriteria tertentu.  

📌 **Cocok digunakan untuk:**  
✅ Membuat laporan yang mengelompokkan data.  
✅ Menghitung jumlah elemen dalam setiap grup.  

💻 **Contoh Kasus: Kelompokkan orang berdasarkan usia**  
```csharp
var people = new List<(string Name, int Age)>
{
    ("Alice", 25),
    ("Bob", 30),
    ("Charlie", 22),
    ("David", 30),
    ("Eve", 25)
};

// Kelompokkan berdasarkan usia
var groupedByAge = people.GroupBy(p => p.Age);

foreach (var group in groupedByAge)
{
    Console.WriteLine($"Usia: {group.Key}");
    foreach (var person in group)
    {
        Console.WriteLine($" - {person.Name}");
    }
}
```
🔍 **Best Practice untuk GroupBy:**  
✅ Pastikan kunci grup (`Key`) unik dan relevan.  
✅ Gunakan `.Select()` setelah `GroupBy()` jika hanya butuh data tertentu.  

---

### **🔹 4.2. Aggregate, Sum, Count, Average (Perhitungan Data)**  
Digunakan untuk menghitung total, jumlah elemen, rata-rata, dan operasi agregasi lainnya.  

📌 **Cocok digunakan untuk:**  
✅ Menghitung total harga, jumlah item, atau rata-rata nilai.  

💻 **Contoh Kasus: Hitung total, jumlah, dan rata-rata harga produk**  
```csharp
var products = new List<(string Name, decimal Price)>
{
    ("Laptop", 15000),
    ("Mouse", 500),
    ("Keyboard", 1200),
    ("Monitor", 3000)
};

// Total harga semua produk
decimal totalPrice = products.Sum(p => p.Price);
Console.WriteLine($"Total Harga: {totalPrice}");

// Jumlah produk
int productCount = products.Count();
Console.WriteLine($"Jumlah Produk: {productCount}");

// Rata-rata harga produk
decimal averagePrice = products.Average(p => p.Price);
Console.WriteLine($"Rata-rata Harga: {averagePrice}");
```

💻 **Contoh Kasus: Gabungkan nama produk menjadi satu string**  
```csharp
string productList = products.Select(p => p.Name).Aggregate((a, b) => a + ", " + b);
Console.WriteLine($"Produk: {productList}");
// Output: Produk: Laptop, Mouse, Keyboard, Monitor
```

🔍 **Best Practice untuk Aggregate & Count:**  
✅ Gunakan `Sum()`, `Count()`, dan `Average()` jika hanya butuh nilai akhir.  
✅ Gunakan `Aggregate()` jika perlu memproses koleksi lebih kompleks.  

---

### **🔹 4.3. Distinct (Menghapus Duplikasi)**  
Digunakan untuk menghapus elemen duplikat dalam koleksi.  

📌 **Cocok digunakan untuk:**  
✅ Menghapus data yang berulang sebelum diproses lebih lanjut.  

💻 **Contoh Kasus: Menghilangkan angka duplikat dalam List**  
```csharp
var numbers = new List<int> { 1, 2, 2, 3, 4, 4, 5, 6, 6 };

// Hapus angka yang duplikat
var uniqueNumbers = numbers.Distinct();

Console.WriteLine(string.Join(", ", uniqueNumbers));
// Output: 1, 2, 3, 4, 5, 6
```

🔍 **Best Practice untuk Distinct:**  
✅ Gunakan `Distinct()` jika ingin menghapus elemen duplikat di **List** atau **Array**.  
✅ Jika ingin menghapus duplikat berdasarkan properti objek, gunakan **IEqualityComparer** atau `GroupBy()`.  

---

### **🔹 4.4. Take & Skip (Paging Data)**  
Digunakan untuk mengambil atau melewatkan sejumlah elemen dalam koleksi.  

📌 **Cocok digunakan untuk:**  
✅ Membuat sistem **paging** dalam UI atau API.  

💻 **Contoh Kasus: Ambil 3 produk pertama (Take)**  
```csharp
var top3Products = products.Take(3);

foreach (var product in top3Products)
{
    Console.WriteLine(product.Name);
}
```

💻 **Contoh Kasus: Lewati 2 produk pertama dan ambil sisanya (Skip)**  
```csharp
var remainingProducts = products.Skip(2);

foreach (var product in remainingProducts)
{
    Console.WriteLine(product.Name);
}
```

💻 **Contoh Kasus: Paging (Ambil halaman ke-2 dengan 2 item per halaman)**  
```csharp
int pageSize = 2;
int pageNumber = 2;

var pageData = products.Skip((pageNumber - 1) * pageSize).Take(pageSize);

foreach (var product in pageData)
{
    Console.WriteLine(product.Name);
}
```

🔍 **Best Practice untuk Take & Skip:**  
✅ Gunakan `Take()` untuk mengambil sejumlah elemen tertentu dari awal.  
✅ Gunakan `Skip()` untuk melewatkan sejumlah elemen sebelum mengambil sisanya.  

---

## **🚀 Kesimpulan & Rekomendasi**  

| LINQ Method | Kegunaan | Contoh |
|------------|---------|-------|
| **GroupBy** | Mengelompokkan data | `GroupBy(p => p.Category)` |
| **Sum, Count, Average** | Perhitungan data | `Sum(p => p.Price)` |
| **Distinct** | Hapus elemen duplikat | `Distinct()` |
| **Take** | Ambil sejumlah elemen | `Take(5)` |
| **Skip** | Lewati sejumlah elemen | `Skip(5)` |
