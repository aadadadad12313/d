
```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

## <font color="#0070c0">Array List</font>

```csharp
using System;
using System.Collections;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            ArrayList people = new ArrayList();

            // Person nesnelerini ekleyelim
            people.Add(new Person("Serkan", 30));
            people.Add(new Person("Ahmet", 25));
            people.Add(new Person("Mehmet", 28));

            // Object olarak foreach kullanımı (unboxing yapmalıyız)
            foreach (object item in people)
            {
                Person person = (Person)item;  // Unboxing
                Console.WriteLine($"{person.Name}, {person.Age} yaşında.");
            }

            Console.ReadKey();
        }
    }
}
```


## <font color="#00b0f0">List </font>

```csharp
using System;
using System.Collections.Generic;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            List<Person> people = new List<Person>();

            // Person nesnelerini ekleyelim
            people.Add(new Person("Serkan", 30));
            people.Add(new Person("Ahmet", 25));
            people.Add(new Person("Mehmet", 28));

            // Directly using the class type in foreach (No unboxing needed)
            foreach (Person person in people)
            {
                Console.WriteLine($"{person.Name}, {person.Age} yaşında.");
            }

            Console.ReadKey();
        }
    }
}
```


```csharp
List<Book> books = new List<Book>()
{//bu işaret object Object Initializer (Nesne Başlatıcı)'dır.
    new Book { Name = "Kitap1", Description = "Açıklama1" },
    /*Buradaki mantık ise listenin içine book nesnelerini oluşturup listeye atıyorsun*/
    new Book { Name = "Kitap2", Description = "Açıklama2" }
};

Console.WriteLine(books[0].Name+" " + books[0].Description);

foreach(Book item in books)
{
    Console.WriteLine(item.Name+" "+item.Description);
}
```
