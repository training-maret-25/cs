# Exceptions and File handling

## **1. Try-Catch-Finally**
**Penjelasan:**  
- `try`: Bagian kode yang berpotensi menimbulkan error.  
- `catch`: Menangkap dan menangani error yang terjadi.  
- `finally`: Kode yang dieksekusi selalu, baik terjadi exception maupun tidak.  

**Best Practices:**  
✅ Jangan gunakan `catch-all` (`catch (Exception ex)`) kecuali benar-benar diperlukan.  
✅ Tangani exception secara spesifik (`catch (IOException ex)`, `catch (FormatException ex)`, dll.).  
✅ Pastikan resource yang digunakan dibersihkan di `finally` atau dengan `using`.

**Contoh Kode:**
```csharp
using System;

class Program
{
    static void Main()
    {
        try
        {
            int number = int.Parse("abc"); // Akan menyebabkan FormatException
            Console.WriteLine($"Parsed number: {number}");
        }
        catch (FormatException ex)
        {
            Console.WriteLine($"Error: Input bukan angka. Detail: {ex.Message}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Unexpected error: {ex.Message}");
        }
        finally
        {
            Console.WriteLine("Proses selesai.");
        }
    }
}
```

**Output:**
```
Error: Input bukan angka. Detail: Input string was not in a correct format.
Proses selesai.
```

## **2. Custom Exception**
**Penjelasan:**  
- Custom exception digunakan untuk membuat jenis error yang lebih spesifik sesuai kebutuhan aplikasi.  
- Kelas exception harus diwarisi dari `Exception`.  
- Sebaiknya tambahkan constructor agar bisa membawa informasi tambahan.  

**Best Practices:**  
✅ Beri nama exception yang jelas, misalnya `InvalidAgeException`.  
✅ Gunakan `base` constructor untuk menyimpan pesan error.  
✅ Jangan buat custom exception jika exception bawaan sudah cukup mewakili.  

**Contoh Kode:**
```csharp
using System;

class InvalidAgeException : Exception
{
    public InvalidAgeException(string message) : base(message) { }
}

class Program
{
    static void ValidateAge(int age)
    {
        if (age < 18)
            throw new InvalidAgeException("Umur harus minimal 18 tahun.");
    }

    static void Main()
    {
        try
        {
            ValidateAge(16); // Ini akan melempar exception
        }
        catch (InvalidAgeException ex)
        {
            Console.WriteLine($"Custom Exception Caught: {ex.Message}");
        }
    }
}
```

**Output:**
```
Custom Exception Caught: Umur harus minimal 18 tahun.
```

## **3. Logging Exception**
**Penjelasan:**  
- Logging sangat penting untuk debugging dan monitoring aplikasi.  
- Bisa menggunakan `Console.WriteLine`, `File.WriteAllText`, atau library logging seperti **Serilog** atau **NLog**.  

**Best Practices:**  
✅ Log error dengan meaningful message.  
✅ Jangan hanya log `ex.Message`, tapi juga `ex.StackTrace` untuk debugging.  
✅ Gunakan library logging seperti **Serilog** atau **NLog** untuk produksi.  

**Contoh Kode Logging Sederhana:**
```csharp
using System;
using System.IO;

class Program
{
    static void Main()
    {
        try
        {
            int result = 10 / int.Parse("0"); // Akan menyebabkan DivideByZeroException
        }
        catch (DivideByZeroException ex)
        {
            string logMessage = $"[{DateTime.Now}] ERROR: {ex.Message}\nStackTrace: {ex.StackTrace}\n";
            File.AppendAllText("error.log", logMessage);
            Console.WriteLine("Terjadi error, cek file error.log");
        }
    }
}
```

**Output di `error.log`:**
```
[2025-03-08 12:34:56] ERROR: Attempted to divide by zero.
StackTrace: at Program.Main()
```

## **4. File Reading & Writing (StreamReader & StreamWriter)**
**Penjelasan:**  
- `StreamReader` digunakan untuk membaca file.  
- `StreamWriter` digunakan untuk menulis ke file.  
- Gunakan `using` statement agar resource dilepaskan otomatis.  

**Best Practices:**  
✅ Gunakan `using` untuk resource management.  
✅ Tangani `IOException` saat membaca/menulis file.  
✅ Pastikan file ada sebelum membacanya (`File.Exists`).  

### **Membaca File (`StreamReader`)**
```csharp
using System;
using System.IO;

class Program
{
    static void Main()
    {
        string filePath = "data.txt";

        if (File.Exists(filePath))
        {
            using (StreamReader reader = new StreamReader(filePath))
            {
                string content = reader.ReadToEnd();
                Console.WriteLine("Isi file:");
                Console.WriteLine(content);
            }
        }
        else
        {
            Console.WriteLine("File tidak ditemukan.");
        }
    }
}
```

### **Menulis ke File (`StreamWriter`)**
```csharp
using System;
using System.IO;

class Program
{
    static void Main()
    {
        string filePath = "output.txt";
        string text = "Hello, C#! Ini contoh file writing.";

        using (StreamWriter writer = new StreamWriter(filePath))
        {
            writer.WriteLine(text);
        }

        Console.WriteLine("Teks telah ditulis ke file output.txt");
    }
}
```

## **Kesimpulan**
- **Gunakan `try-catch-finally`** untuk menangani error dengan cara yang benar.  
- **Hindari `catch (Exception ex)`** kecuali benar-benar diperlukan.  
- **Gunakan `using` statement** untuk menghindari memory leak pada resource seperti file.  
- **Selalu log error dengan meaningful message** agar mudah didiagnosis.