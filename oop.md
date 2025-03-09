
## **1. Class & Object**  
### **Penjelasan**  
Dalam C#, **class** adalah blueprint atau cetakan untuk membuat objek. Class mendefinisikan struktur data dan perilaku dengan **fields (variabel)**, **methods (fungsi)**, **properties (getter/setter)**, dan **constructors (inisialisasi awal objek)**.  
Objek adalah instance dari class yang memiliki nilai konkret.

---

### **1.1 Fields, Methods, Property, Constructor**
**Fields**: Variabel dalam class yang menyimpan data.  
**Methods**: Fungsi dalam class untuk melakukan operasi.  
**Properties**: Kombinasi getter & setter untuk mengontrol akses ke field.  
**Constructor**: Method khusus yang dieksekusi saat objek dibuat.  

### **1.2 Access Modifiers di C#**  
Access Modifier menentukan sejauh mana suatu **field, property, method, atau class** bisa diakses dari luar class-nya.  

| Modifier   | Dapat Diakses Oleh | Contoh Penggunaan |
|------------|-------------------|------------------|
| **public** | Semua class dalam project | Method yang harus bisa dipanggil dari luar |
| **private** | Hanya dalam class yang sama | Field yang tidak boleh diakses langsung dari luar |
| **protected** | Class yang sama & turunan (inheritance) | Method yang hanya bisa digunakan oleh subclass |
| **internal** | Semua class dalam **assembly** yang sama | Class yang hanya dipakai dalam project yang sama |
| **protected internal** | Kombinasi `protected` + `internal` | Class yang bisa diakses oleh subclass dalam assembly |
| **private protected** | Kombinasi `private` + `protected` | Hanya bisa diakses oleh subclass di dalam class yang sama |

#### **Contoh Kasus Masing-Masing Access Modifier**
Kita buat contoh `Person` dengan berbagai access modifier:

```csharp
public class Person
{
    // PRIVATE: Hanya bisa diakses dalam class ini
    private string _name;

    // PUBLIC: Bisa diakses dari mana saja
    public int Age { get; set; }

    // PROTECTED: Bisa diakses oleh class ini & turunannya
    protected string Address { get; set; }

    // INTERNAL: Hanya bisa diakses dalam assembly (project) yang sama
    internal string NationalID { get; set; }

    // PROTECTED INTERNAL: Bisa diakses oleh subclass dalam assembly yang sama
    protected internal string PhoneNumber { get; set; }

    // PRIVATE PROTECTED: Hanya subclass dalam class yang sama yang bisa mengakses
    private protected string SecretCode { get; set; }

    // Constructor
    public Person(string name, int age)
    {
        _name = name;
        Age = age;
    }

    // PUBLIC METHOD: Bisa dipanggil dari luar
    public void Introduce()
    {
        Console.WriteLine($"Hi, my name is {_name} and I'm {Age} years old.");
    }
}
```

#### **Contoh Inheritance dengan Protected**
Kita buat class `Employee` yang mewarisi `Person` dan coba akses beberapa modifier:

```csharp
public class Employee : Person
{
    public string JobTitle { get; set; }

    public Employee(string name, int age, string jobTitle) 
        : base(name, age)
    {
        JobTitle = jobTitle;
        Address = "Confidential"; // Bisa diakses karena 'protected'
    }

    public void ShowDetails()
    {
        Console.WriteLine($"I'm an {JobTitle} living at {Address}.");
    }
}
```

#### **Implementasi & Penggunaan**
```csharp
class Program
{
    static void Main()
    {
        // Membuat objek Person
        Person person = new Person("Alice", 30);
        person.Introduce();
        
        // Membuat objek Employee
        Employee employee = new Employee("Bob", 35, "Engineer");
        employee.ShowDetails();

        // Akses yang diizinkan
        Console.WriteLine($"Age: {employee.Age}"); // ✅ public -> bisa diakses
        // Console.WriteLine(employee._name); // ❌ private -> tidak bisa diakses
        // Console.WriteLine(employee.Address); // ❌ protected -> hanya bisa di subclass
        // Console.WriteLine(employee.NationalID); // ❌ internal -> hanya dalam assembly yang sama
    }
}
```

💡 **Output:**  
```
Hi, my name is Alice and I'm 30 years old.  
I'm an Engineer living at Confidential.  
Age: 35  
```

#### **Best Practices**
✅ **Gunakan `private` untuk field yang tidak boleh diakses langsung.**  
✅ **Gunakan `protected` hanya jika butuh diwarisi oleh subclass.**  
✅ **Gunakan `internal` untuk membatasi akses dalam project.**  
✅ **Hindari `public` jika tidak perlu, karena terlalu terbuka.**  

---

### **1.3 Contoh Kasus**
Misalnya, kita punya class `Car` yang merepresentasikan sebuah mobil.  
```csharp
public class Car
{
    // Field (Private)
    private string _brand;

    // Property (Encapsulation dengan getter & setter)
    public string Brand
    {
        get { return _brand; }
        set { _brand = value; }
    }

    // Property Auto-Implemented (Lebih ringkas)
    public string Model { get; set; }

    // Readonly Property (hanya bisa di-set dalam konstruktor)
    public int Year { get; }

    // Constructor (dipanggil saat objek dibuat)
    public Car(string brand, string model, int year)
    {
        _brand = brand;
        Model = model;
        Year = year;
    }

    // Method
    public void Drive()
    {
        Console.WriteLine($"{Brand} {Model} is driving...");
    }
}
```

**Implementasi & Penggunaan**
```csharp
class Program
{
    static void Main()
    {
        // Membuat objek Car
        Car myCar = new Car("Toyota", "Corolla", 2023);

        // Mengakses property
        Console.WriteLine($"Car: {myCar.Brand} {myCar.Model} ({myCar.Year})");

        // Memanggil method
        myCar.Drive();
    }
}
```
💡 **Output:**  
```
Car: Toyota Corolla (2023)  
Toyota Corolla is driving...
```

---

### **1.4 Best Practices**
1. **Gunakan Encapsulation** → Hindari exposing field secara langsung (`private` field + `public` property).  
2. **Gunakan Auto-Implemented Property** jika hanya ingin menyimpan nilai tanpa logika tambahan.  
3. **Readonly Property** untuk nilai yang tidak boleh diubah setelah objek dibuat.  
4. **Buat Constructor yang Jelas** → Pastikan parameter yang diperlukan diinisialisasi dalam constructor.  
5. **Gunakan Access Modifier dengan Benar** → Jangan buat semuanya `public` tanpa alasan.


## **2. Encapsulation, Inheritance, Polymorphism** 

### **2.1 Encapsulation (Enkapsulasi)**
#### **Penjelasan**  
Encapsulation adalah konsep dalam OOP yang membatasi akses langsung ke data di dalam suatu objek, biasanya dengan cara:  
- Menyimpan data di **private field**  
- Menyediakan akses melalui **public properties** (getter/setter)  
- Mencegah modifikasi data langsung tanpa kontrol  

🔹 **Tujuan utama encapsulation:**  
✅ Melindungi data dari perubahan yang tidak diinginkan  
✅ Menjaga konsistensi dan validitas data  
✅ Menyembunyikan implementasi internal dari luar  

---

#### **Contoh Kasus Tanpa Encapsulation (BAD PRACTICE)**<br>
Misalnya kita punya class `BankAccount`, tapi tanpa encapsulation:  

```csharp
public class BankAccount
{
    public string AccountNumber;
    public double Balance;

    public void Withdraw(double amount)
    {
        Balance -= amount; // Tidak ada validasi!
    }
}
```
🚨 **Masalah:**  
❌ `Balance` bisa diubah langsung dari luar → Bisa dibuat negatif!  
❌ Tidak ada validasi saat withdraw → Bisa menarik saldo lebih dari yang ada  

---

#### **Contoh Kasus dengan Encapsulation (BEST PRACTICE)**
Sekarang kita perbaiki dengan encapsulation:  

```csharp
public class BankAccount
{
    // Private field (data tidak bisa diakses langsung)
    private double _balance;

    // Public Property (getter hanya untuk membaca saldo)
    public double Balance
    {
        get { return _balance; }
        private set { _balance = value; } // Hanya bisa diubah dalam class ini
    }

    public string AccountNumber { get; } // Read-only (tidak bisa diubah setelah dibuat)

    // Constructor untuk inisialisasi awal
    public BankAccount(string accountNumber, double initialBalance)
    {
        AccountNumber = accountNumber;
        _balance = initialBalance;
    }

    // Method untuk deposit uang
    public void Deposit(double amount)
    {
        if (amount > 0)
        {
            _balance += amount;
            Console.WriteLine($"Deposited {amount}. New balance: {_balance}");
        }
        else
        {
            Console.WriteLine("Deposit amount must be positive.");
        }
    }

    // Method untuk withdraw uang (dengan validasi!)
    public void Withdraw(double amount)
    {
        if (amount > 0 && amount <= _balance)
        {
            _balance -= amount;
            Console.WriteLine($"Withdrew {amount}. New balance: {_balance}");
        }
        else
        {
            Console.WriteLine("Invalid withdrawal amount.");
        }
    }
}
```

**Implementasi & Penggunaan**
```csharp
class Program
{
    static void Main()
    {
        BankAccount account = new BankAccount("123456", 1000);

        // Akses saldo hanya bisa lewat getter
        Console.WriteLine($"Account {account.AccountNumber} Balance: {account.Balance}");

        // Deposit & Withdraw dengan validasi
        account.Deposit(500);   // ✅ Bisa
        account.Withdraw(200);  // ✅ Bisa
        account.Withdraw(5000); // ❌ Tidak bisa, saldo kurang

        // Tidak bisa mengubah saldo langsung
        // account.Balance = 5000; // ❌ Error, karena setter private
    }
}
```
💡 **Output:**  
```
Account 123456 Balance: 1000  
Deposited 500. New balance: 1500  
Withdrew 200. New balance: 1300  
Invalid withdrawal amount.  
```

---

#### **Best Practices dalam Encapsulation**
✅ **Gunakan `private` untuk field yang tidak boleh diakses langsung.**  
✅ **Gunakan property (`public get`, `private set`) agar data hanya bisa diubah dalam class.**  
✅ **Sediakan method yang mengontrol bagaimana data diubah (`Deposit()`, `Withdraw()`).**  
✅ **Gunakan validasi sebelum mengubah data untuk mencegah input yang salah.**  

---

#### **Kesimpulan**
🔹 **Encapsulation melindungi data dari akses langsung & modifikasi yang tidak valid.**  
🔹 **Menggunakan getter/setter memungkinkan kita mengontrol bagaimana data diubah.**  
🔹 **Ini adalah prinsip pertama dari SOLID: Single Responsibility (SRP), karena data hanya bisa diubah lewat method tertentu!**

### **2.2 Inheritance (Pewarisan)**
#### **Penjelasan**  
Inheritance adalah konsep dalam OOP yang memungkinkan **suatu class mewarisi properti dan method dari class lain**.  

🔹 **Tujuan utama inheritance:**  
✅ Menghindari kode duplikasi → Reusable Code  
✅ Meningkatkan keterbacaan dan perawatan kode  
✅ Memungkinkan penggunaan **Polymorphism**  

---

#### **Contoh Kasus Tanpa Inheritance (BAD PRACTICE)**
Misalkan kita punya class `Employee` dan `Manager`, tapi kita tidak menggunakan inheritance:  

```csharp
public class Employee
{
    public string Name;
    public double Salary;

    public void Work()
    {
        Console.WriteLine($"{Name} is working.");
    }
}

public class Manager
{
    public string Name;
    public double Salary;
    public string Department;

    public void Work()
    {
        Console.WriteLine($"{Name} is managing the {Department} department.");
    }
}
```
🚨 **Masalah:**  
❌ **Duplikasi kode** pada `Name`, `Salary`, dan `Work()`.  
❌ Jika ada perubahan di `Employee`, kita harus mengubah `Manager` juga.  

---

#### **Contoh Kasus dengan Inheritance (BEST PRACTICE)**
Sekarang kita buat **base class** `Employee` dan turunkan `Manager` darinya:  

```csharp
// Base Class (Superclass)
public class Employee
{
    public string Name { get; set; }
    public double Salary { get; set; }

    public Employee(string name, double salary)
    {
        Name = name;
        Salary = salary;
    }

    public void Work()
    {
        Console.WriteLine($"{Name} is working.");
    }
}

// Derived Class (Subclass)
public class Manager : Employee
{
    public string Department { get; set; }

    // Konstruktor Manager harus memanggil konstruktor Employee (base)
    public Manager(string name, double salary, string department)
        : base(name, salary)
    {
        Department = department;
    }

    // Override method Work untuk Manager
    public void Work()
    {
        Console.WriteLine($"{Name} is managing the {Department} department.");
    }
}
```

**Implementasi & Penggunaan**
```csharp
class Program
{
    static void Main()
    {
        // Membuat objek Employee
        Employee emp = new Employee("Alice", 5000);
        emp.Work();  // Output: Alice is working.

        // Membuat objek Manager (yang merupakan Employee)
        Manager mgr = new Manager("Bob", 8000, "IT");
        mgr.Work();  // Output: Bob is managing the IT department.

        // Akses properti yang diwarisi
        Console.WriteLine($"{mgr.Name} earns {mgr.Salary}");  // Bob earns 8000
    }
}
```

💡 **Output:**  
```
Alice is working.  
Bob is managing the IT department.  
Bob earns 8000  
```

---

#### **`base` Keyword untuk Memanggil Base Class**
Kadang kita butuh memanggil method dari superclass. Kita bisa gunakan **`base`**:  

```csharp
public class Manager : Employee
{
    public string Department { get; set; }

    public Manager(string name, double salary, string department)
        : base(name, salary)
    {
        Department = department;
    }

    public void Work()
    {
        base.Work();  // Memanggil versi Employee
        Console.WriteLine($"{Name} is also managing the {Department} department.");
    }
}
```

Output:
```
Bob is working.  
Bob is also managing the IT department.  
```

---

#### **Best Practices dalam Inheritance**
✅ **Gunakan inheritance hanya jika ada hubungan "IS-A".**  
✅ **Gunakan `protected` jika subclass perlu mengakses data parent.**  
✅ **Gunakan `base` untuk mengakses method dari parent class.**  
✅ **Hindari deep inheritance (terlalu banyak level pewarisan).**  
✅ **Jika hanya butuh shared behavior, lebih baik gunakan Interface atau Composition.**

---

#### **Kesimpulan**
🔹 **Inheritance membuat kode lebih reusable & mengurangi duplikasi.**  
🔹 **Gunakan inheritance jika ada hubungan yang jelas antara superclass & subclass.**  
🔹 **Inheritance adalah bagian dari SOLID Principle → Open/Closed Principle (OCP).**

### **2.3 Polymorphism (Polimorfisme)**
#### **Penjelasan**
Polymorphism adalah kemampuan suatu method untuk memiliki **banyak bentuk**.

🔹 **Tujuan utama Polymorphism:**  
✅ Memudahkan pemanggilan method yang sama dari class berbeda  
✅ Meningkatkan fleksibilitas dan scalability kode  
✅ Memanfaatkan konsep inheritance agar lebih dinamis  

#### **Jenis-jenis Polymorphism di C#:**  
1️⃣ **Compile-time (Static) Polymorphism** → Overloading  
2️⃣ **Run-time (Dynamic) Polymorphism** → Overriding  

---

#### **Compile-time Polymorphism (Method Overloading)**
🔹 **Method Overloading** = Banyak method dengan nama sama, tapi beda parameter.  

💡 **Contoh:**
```csharp
public class Calculator
{
    // Overloading dengan parameter yang berbeda
    public int Add(int a, int b)
    {
        return a + b;
    }

    public double Add(double a, double b)
    {
        return a + b;
    }

    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
}

class Program
{
    static void Main()
    {
        Calculator calc = new Calculator();

        Console.WriteLine(calc.Add(2, 3));       // Output: 5
        Console.WriteLine(calc.Add(2.5, 3.5));   // Output: 6
        Console.WriteLine(calc.Add(2, 3, 4));    // Output: 9
    }
}
```
✅ **Keuntungan Overloading:**  
- Bisa memanggil method yang sama dengan parameter berbeda  
- Tidak perlu membuat method dengan nama berbeda  

---

#### **Run-time Polymorphism (Method Overriding)**
🔹 **Method Overriding** = Mengubah implementasi method yang diwarisi dari parent class.  

💡 **Contoh:**
```csharp
public class Employee
{
    public virtual void Work()
    {
        Console.WriteLine("Employee is working.");
    }
}

public class Manager : Employee
{
    // Override method Work
    public override void Work()
    {
        Console.WriteLine("Manager is leading a team.");
    }
}

public class Developer : Employee
{
    // Override method Work
    public override void Work()
    {
        Console.WriteLine("Developer is writing code.");
    }
}

class Program
{
    static void Main()
    {
        Employee emp1 = new Manager();
        Employee emp2 = new Developer();

        emp1.Work();  // Output: Manager is leading a team.
        emp2.Work();  // Output: Developer is writing code.
    }
}
```
✅ **Keuntungan Overriding:**  
- Memungkinkan method yang sama memiliki implementasi berbeda di subclass  
- Memanfaatkan **Late Binding** (pemilihan method dilakukan saat runtime)  

---

#### **`abstract` vs `virtual` dalam Overriding**
🔹 **`virtual`** → Method bisa di-override, tapi ada default implementation  
🔹 **`abstract`** → Method harus di-override, tidak punya default implementation  

💡 **Contoh `abstract`:**
```csharp
public abstract class Animal
{
    public abstract void MakeSound(); // Harus diimplementasikan di subclass
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof! Woof!");
    }
}

public class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Meow! Meow!");
    }
}

class Program
{
    static void Main()
    {
        Animal myDog = new Dog();
        Animal myCat = new Cat();

        myDog.MakeSound();  // Output: Woof! Woof!
        myCat.MakeSound();  // Output: Meow! Meow!
    }
}
```

---

#### **Best Practices dalam Polymorphism**
✅ **Gunakan Overloading jika method melakukan tugas yang sama dengan parameter berbeda.**  
✅ **Gunakan Overriding untuk mengganti behavior method dari parent class.**  
✅ **Gunakan `abstract` jika method harus diimplementasikan oleh subclass.**  
✅ **Gunakan `virtual` jika method bisa di-override tapi tidak wajib.**  
✅ **Gunakan `base` jika ingin tetap memanggil method dari parent class.**  

---

#### **Kesimpulan**
🔹 **Polymorphism membuat kode lebih fleksibel & reusable.**
🔹 **Compile-time Polymorphism (Overloading) terjadi saat compile-time.**
🔹 **Run-time Polymorphism (Overriding) terjadi saat program berjalan.**
🔹 **Polymorphism mendukung SOLID Principles → Open/Closed Principle (OCP).**

## **3. Abstract Class vs Interface**
### **Penjelasan**  
🔹 **Abstract Class** dan **Interface** digunakan untuk mencapai **abstraction**, yaitu menyembunyikan detail implementasi dan hanya menampilkan fitur pentingnya.  
🔹 **Tapi, kapan harus pakai abstract class dan kapan harus pakai interface?**  

| **Fitur**            | **Abstract Class** | **Interface** |
|----------------------|------------------|--------------|
| **Tujuan**           | Mewarisi properti & method dasar | Menyediakan kontrak (tanpa implementasi) |
| **Field/Properties** | Bisa memiliki field & properties | Hanya deklarasi properties, tanpa field |
| **Method**          | Bisa memiliki method dengan body (`virtual` atau `abstract`) | Hanya deklarasi method (C# 8+ bisa ada default method) |
| **Constructor**     | Bisa punya constructor | Tidak bisa punya constructor |
| **Multiple Inheritance** | Tidak bisa (hanya satu superclass) | Bisa (banyak interface bisa diimplementasikan) |

---

### **Contoh Abstract Class**
🔹 **Gunakan Abstract Class jika ada behavior default yang diwarisi oleh subclass**  

```csharp
public abstract class Animal
{
    public string Name { get; set; }

    public Animal(string name)
    {
        Name = name;
    }

    public void Sleep()  // Method dengan implementasi
    {
        Console.WriteLine($"{Name} is sleeping.");
    }

    public abstract void MakeSound(); // Harus diimplementasikan oleh subclass
}

public class Dog : Animal
{
    public Dog(string name) : base(name) { }

    public override void MakeSound()
    {
        Console.WriteLine("Woof! Woof!");
    }
}

class Program
{
    static void Main()
    {
        Dog dog = new Dog("Buddy");
        dog.Sleep();      // Output: Buddy is sleeping.
        dog.MakeSound();  // Output: Woof! Woof!
    }
}
```
✅ **Gunakan Abstract Class jika:**  
- Ada **default behavior** yang bisa diwarisi oleh subclass  
- Mengandung field atau constructor  
- **Hierarki class yang erat**  

---

### **Contoh Interface**
🔹 **Gunakan Interface jika hanya ingin mendefinisikan kontrak (tanpa implementasi default).**  

```csharp
public interface IMovable
{
    void Move();
}

public interface IAttackable
{
    void Attack();
}

public class Robot : IMovable, IAttackable
{
    public void Move()
    {
        Console.WriteLine("Robot is moving...");
    }

    public void Attack()
    {
        Console.WriteLine("Robot is attacking...");
    }
}

class Program
{
    static void Main()
    {
        Robot robot = new Robot();
        robot.Move();    // Output: Robot is moving...
        robot.Attack();  // Output: Robot is attacking...
    }
}
```
✅ **Gunakan Interface jika:**  
- Ingin membuat **kontrak** yang bisa diterapkan ke banyak class  
- Perlu **multiple inheritance**  
- Tidak butuh field atau constructor  

---

### **Best Practices Abstract Class vs Interface**
✅ **Gunakan abstract class jika:**  
- Butuh default behavior  
- Butuh field atau constructor  
- Struktur classnya erat (inheritance hierarchy)  

✅ **Gunakan interface jika:**  
- Hanya butuh kontrak tanpa implementasi  
- Perlu **multiple inheritance**  
- Implementasi bisa bervariasi di banyak class  

---

### **Kesimpulan**
🔹 **Abstract Class = Struktur class dengan beberapa implementasi bawaan.**  
🔹 **Interface = Kontrak tanpa implementasi (lebih fleksibel).**  
🔹 **SOLID Principle:**  
   - **Liskov Substitution Principle (LSP)** → Bisa mengganti superclass dengan subclass  
   - **Interface Segregation Principle (ISP)** → Interface harus spesifik, tidak boleh besar seperti "God Object"

## **6. Dependency Injection (DI)**
### **Penjelasan**  
🔹 **Dependency Injection (DI)** adalah teknik untuk **mengelola dependensi antar objek** dengan cara **memberikan dependensi dari luar** alih-alih membuatnya langsung di dalam class.  
🔹 DI membantu mencapai **loose coupling**, meningkatkan testability, dan mempermudah pengelolaan kode.  

---

#### **Masalah Tanpa Dependency Injection**
🔴 **Tanpa DI, class memiliki ketergantungan langsung (tight coupling), sulit diuji & sulit diganti.**  

💡 **Contoh tanpa DI (Tight Coupling):**
```csharp
public class EmailService
{
    public void SendEmail(string message)
    {
        Console.WriteLine($"Sending Email: {message}");
    }
}

public class Notification
{
    private EmailService _emailService = new EmailService(); // Tight coupling

    public void Notify(string message)
    {
        _emailService.SendEmail(message);
    }
}
```
🔴 **Masalah:**  
❌ Class `Notification` langsung membuat instance `EmailService`, sulit diuji & tidak fleksibel.  
❌ Kalau ingin mengganti `EmailService` dengan `SMSService`, harus mengubah kode di `Notification`.  

---

#### **Menggunakan Dependency Injection**
✅ **Dengan DI, dependensi diberikan dari luar (loose coupling).**  

💡 **Contoh DI dengan Constructor Injection:**
```csharp
public interface IMessageService
{
    void SendMessage(string message);
}

public class EmailService : IMessageService
{
    public void SendMessage(string message)
    {
        Console.WriteLine($"Sending Email: {message}");
    }
}

public class Notification
{
    private readonly IMessageService _messageService;

    // Inject dependency melalui constructor
    public Notification(IMessageService messageService)
    {
        _messageService = messageService;
    }

    public void Notify(string message)
    {
        _messageService.SendMessage(message);
    }
}

class Program
{
    static void Main()
    {
        IMessageService emailService = new EmailService();
        Notification notification = new Notification(emailService); // Dependency diberikan dari luar

        notification.Notify("Hello DI!"); // Output: Sending Email: Hello DI!
    }
}
```
✅ **Keuntungan:**  
- **Loose Coupling** → `Notification` tidak bergantung langsung pada `EmailService`.  
- **Mudah diuji** → Bisa mengganti `EmailService` dengan `SMSService` atau `MockService`.  

---

#### **Jenis Dependency Injection di C#**
| **Jenis DI**          | **Penjelasan** | **Kelebihan** | **Kekurangan** |
|----------------------|---------------|--------------|--------------|
| **Constructor Injection** | Dependency dikirim melalui constructor | Paling umum & aman | Harus inisialisasi saat objek dibuat |
| **Property Injection** | Dependency diberikan melalui property | Bisa diubah kapan saja | Dependency bisa null |
| **Method Injection** | Dependency dikirim melalui method parameter | Fleksibel, tidak harus global | Harus disertakan di setiap pemanggilan |

---

💡 **Contoh Property Injection:**
```csharp
public class Notification
{
    public IMessageService MessageService { get; set; } // Dependency diberikan dari luar

    public void Notify(string message)
    {
        MessageService?.SendMessage(message);
    }
}

class Program
{
    static void Main()
    {
        var notification = new Notification();
        notification.MessageService = new EmailService(); // Inject dependency

        notification.Notify("Hello Property Injection!");
    }
}
```

#### **Implementasi DI di .NET Core (Built-in DI Container)**
.NET Core memiliki **built-in Dependency Injection Container**.  

💡 **Contoh Implementasi DI di .NET Core:**
```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddScoped<IMessageService, EmailService>(); // Register DI container

var app = builder.Build();
var service = app.Services.GetRequiredService<IMessageService>();
service.SendMessage("Hello from .NET Core DI!");
```
✅ **Keuntungan:**  
- **Mudah digunakan di ASP.NET Core**  
- **Mendukung Singleton, Scoped, dan Transient**  

---

### **Best Practices dalam Dependency Injection**
✅ **Gunakan Interface (`IService`) untuk loose coupling**  
✅ **Gunakan Constructor Injection untuk dependensi wajib**  
✅ **Gunakan Property Injection untuk dependensi opsional**  
✅ **Gunakan DI Container di .NET Core untuk pengelolaan otomatis**  
✅ **Hindari Service Locator (anti-pattern, mirip singleton)**  

---

### **Kesimpulan**
🔹 **Dependency Injection membuat kode lebih fleksibel, testable, dan scalable.**  
🔹 **Gunakan DI untuk menghindari tight coupling dan meningkatkan maintainability.**  
🔹 **C# menyediakan DI container bawaan yang bisa digunakan di aplikasi ASP.NET Core.**  
🔹 **SOLID Principle:**  
   - **Dependency Inversion Principle (DIP)** → "High-level modules should not depend on low-level modules."  