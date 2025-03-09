# Asynchronous Programming

## **1. Task vs Thread**
### **Penjelasan**
- **Thread**: Unit eksekusi independen dalam sistem operasi yang berjalan secara bersamaan (concurrent).
- **Task**: Wrapper tingkat lebih tinggi yang menggunakan **ThreadPool** untuk manajemen thread yang lebih efisien.

### **Perbedaan Utama**
| Feature | Thread | Task |
|---------|--------|------|
| Level | Low-level | High-level |
| Manajemen | Manual | Otomatis (ThreadPool) |
| Overhead | Lebih berat | Lebih ringan |
| Support Async | Tidak | Ya (async/await) |

### **Contoh 1: Thread vs Task**
```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
    static void RunThread()
    {
        Thread thread = new Thread(() =>
        {
            Console.WriteLine($"Thread: {Thread.CurrentThread.ManagedThreadId} running.");
            Thread.Sleep(2000);
            Console.WriteLine("Thread finished.");
        });
        thread.Start();
    }

    static async Task RunTask()
    {
        await Task.Run(() =>
        {
            Console.WriteLine($"Task: {Thread.CurrentThread.ManagedThreadId} running.");
            Thread.Sleep(2000);
            Console.WriteLine("Task finished.");
        });
    }

    static async Task Main()
    {
        RunThread();
        await RunTask();
    }
}
```
### **Best Practice**
✅ **Gunakan Task** kecuali benar-benar perlu kontrol manual atas thread.

## **2. async & await**
### **Penjelasan**
- `async` digunakan untuk mendefinisikan metode yang mendukung operasi asinkron.
- `await` digunakan untuk menunggu penyelesaian `Task` tanpa memblokir thread utama.

### **Contoh 2: async & await**
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task<int> FetchDataAsync()
    {
        await Task.Delay(2000); // Simulasi delay
        return 42;
    }

    static async Task Main()
    {
        Console.WriteLine("Fetching data...");
        int result = await FetchDataAsync();
        Console.WriteLine($"Data received: {result}");
    }
}
```
### **Best Practice**
✅ **Jangan gunakan `.Result` atau `.Wait()` pada Task** karena bisa menyebabkan deadlock.

## **3. Handling Deadlocks & Race Conditions**
### **3.1 Deadlock**
Deadlock terjadi ketika dua atau lebih thread saling menunggu resource yang terkunci.

### **Contoh 3: Deadlock (BAD CODE)**
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task<string> GetDataAsync()
    {
        await Task.Delay(1000);
        return "Data loaded";
    }

    static void Main()
    {
        // BAD CODE: Menyebabkan deadlock
        string result = GetDataAsync().Result;
        Console.WriteLine(result);
    }
}
```
🔴 **Masalah**: `.Result` memblokir thread utama dan menyebabkan deadlock.  

### **Solusi 3: Hindari Deadlock dengan async all the way**
```csharp
static async Task Main()
{
    string result = await GetDataAsync();
    Console.WriteLine(result);
}
```
✅ **Jangan gunakan `.Result` atau `.Wait()`, gunakan `await` secara menyeluruh.**

---

### **3.2 Race Condition**
Terjadi saat beberapa thread mengakses dan memodifikasi resource bersama tanpa sinkronisasi.

### **Contoh 4: Race Condition (BAD CODE)**
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    private static int counter = 0;

    static async Task IncrementAsync()
    {
        for (int i = 0; i < 1000; i++)
        {
            counter++;
        }
    }

    static async Task Main()
    {
        Task task1 = IncrementAsync();
        Task task2 = IncrementAsync();
        await Task.WhenAll(task1, task2);
        Console.WriteLine($"Counter: {counter}"); // Seharusnya 2000, tapi bisa kurang
    }
}
```
🔴 **Masalah**: Dua task mengakses `counter` secara bersamaan tanpa sinkronisasi.

### **Solusi 4: Gunakan Lock atau SemaphoreSlim**
```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
    private static int counter = 0;
    private static readonly object lockObj = new object();

    static async Task IncrementAsync()
    {
        for (int i = 0; i < 1000; i++)
        {
            lock (lockObj) // Mencegah race condition
            {
                counter++;
            }
        }
    }

    static async Task Main()
    {
        Task task1 = IncrementAsync();
        Task task2 = IncrementAsync();
        await Task.WhenAll(task1, task2);
        Console.WriteLine($"Counter: {counter}");
    }
}
```
✅ **Gunakan `lock` atau `SemaphoreSlim` untuk menghindari race condition.**

## **4. Performance Optimization dengan Asynchronous Execution**
**Kenapa Async?**
- Menghindari blocking thread utama.
- Meningkatkan throughput dengan **concurrent execution**.

### **Contoh 5: Blocking vs Non-Blocking**
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static void BlockingMethod()
    {
        Task.Delay(2000).Wait(); // Blocking
        Console.WriteLine("Blocking completed");
    }

    static async Task NonBlockingMethod()
    {
        await Task.Delay(2000); // Non-blocking
        Console.WriteLine("Non-blocking completed");
    }

    static async Task Main()
    {
        Console.WriteLine("Start");
        //BlockingMethod(); // Uncomment untuk melihat perbedaan
        await NonBlockingMethod();
        Console.WriteLine("End");
    }
}
```
✅ **Gunakan `await` daripada `.Wait()` untuk menghindari blocking.**

---

### **Best Practices untuk Performance Optimization**
1. **Gunakan `ConfigureAwait(false)` saat tidak perlu konteks UI.**
   ```csharp
   await Task.Delay(1000).ConfigureAwait(false);
   ```
2. **Gunakan `Parallel.ForEachAsync` untuk parallel execution.**
   ```csharp
   await Parallel.ForEachAsync(items, async (item, token) =>
   {
       await ProcessItemAsync(item);
   });
   ```
3. **Gunakan `CancellationToken` untuk membatalkan task jika tidak diperlukan.**
   ```csharp
   using System.Threading;

   CancellationTokenSource cts = new CancellationTokenSource();
   CancellationToken token = cts.Token;

   Task.Run(async () =>
   {
       await Task.Delay(5000, token);
       Console.WriteLine("Task selesai");
   });

   cts.Cancel(); // Membatalkan task
   ```

## **Kesimpulan**
| **Topik** | **Best Practice** |
|-----------|------------------|
| **Task vs Thread** | Gunakan `Task` untuk eksekusi async yang lebih efisien |
| **async & await** | Jangan gunakan `.Result` atau `.Wait()` |
| **Deadlocks** | Gunakan `await` secara penuh (async all the way) |
| **Race Conditions** | Gunakan `lock` atau `SemaphoreSlim` |
| **Performance Optimization** | Gunakan `ConfigureAwait(false)`, `Parallel.ForEachAsync`, dan `CancellationToken` |