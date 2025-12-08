## 🔄 Iterator Design Pattern

### 📌 Defination
**Iterator Pattern** is a behavioral design pattern that allows you to **sequentially** access elements of **a collection** (like an array, list, or map) without exposing its underlying structure. 

It **separates the traversal logic from the collection** itself, allowing you to iterate over different types of collections uniformly.


>**Iterator Pattern** হলো একটি **behavioral design pattern** যা একটি collection (যেমন array, list, map) এর elements **sequentially access** করতে দেয় **without exposing the internal structure**।  

>**Basically:** তুমি collection traverse করতে পারবে, কিন্তু collection-এর **internal structure expose করতে হবে না**।

<br>

### ❗ 2. Why Needed



- To provide a standard way to traverse a collection without exposing its internal structure.  
- To support multiple traversal methods without modifying the collection.  
- To simplify code by removing complex loops from client code.  

<br>

### 🏢 3. Real World Example 


- **TV Remote:** You can go through channels one by one without knowing how channels are stored.  
- **Playlist in Music App:** You can iterate through songs without knowing the internal storage of songs.  

<br>

### 🎯 4. Goal and Purpose 


- To provide a consistent way to traverse different types of collections.  
- To decouple iteration from collection implementation.  
- To allow multiple simultaneous traversals of the same collection.  

<br>

### 📌 5. Use Cases 


- Traversing arrays, lists, sets, or maps.  
- Implementing custom traversal for complex data structures like trees or graphs.  
- Iterating over database result sets.  
- Supporting multiple ways to traverse a collection (forward, backward, filtered).  

<br>

### ⚙️ 6. How it Works 


- The collection implements an `Iterable` interface or provides a method to return an `Iterator`.  
- The Iterator has methods like `hasNext()` and `next()` to traverse elements.  
- The client code uses the iterator instead of accessing the collection directly.  

<br>

### ☑️ 7. Advantages 


- Encapsulates iteration logic.  
- Provides **multiple traversal methods**.  
- Decouples collection from client code.  
- Supports complex data structures.  
- SRP maintain


### ☑️ 8. Disadvantages 


- **Extra classes/interfaces** may increase complexity.  
- Slightly **more memory** usage for iterator objects.  
- May introduce overhead for simple collections. 
- hard to debug

<br>

### 🚨 Example  1 : Music Playlist
Scenario :
We want to play songs one by one in a playlist.

### Bad Example : 1 (Without Iterator)
```c#
using System;
using System.Collections.Generic;

class BadPlayList
{
    public List<string> Songs = new List<string>();
}

class Program
{
    static void Main(string[] args)
    {
        BadPlayList playList = new BadPlayList();
        playList.Songs.Add("Shape of You");
        playList.Songs.Add("Blinding Lights");
        playList.Songs.Add("Dance Monkey");

        Console.WriteLine("Playing songs:");

        // Client directly accesses the collection
        for (int i = 0; i < playList.Songs.Count; i++)
        {
            Console.WriteLine(playList.Songs[i]);
        }
    }
}
```
### Problems in This Approach

- Client accesses the internal `List<string>` directly → tight coupling.  
- If internal storage changes (e.g., array → LinkedList), client code must change.  
- Multiple types of traversal (backward, filtered) are hard to implement.  

### ☑️ Example : 1 (Using Iterator Pattern)

```c#
using System;
using System.Collections.Generic;

// Iterator Interface
public interface IIterator<T>
{
    bool HasNext();
    T Next();
}

// Collection Interface
public interface IPlayList<T>
{
    IIterator<T> CreateIterator();
}

// Concrete Iterator
public class PlayListIterator<T> : IIterator<T>
{
    private List<T> _items;
    private int _position = 0;

    public PlayListIterator(List<T> items)
    {
        _items = items;
    }

    public bool HasNext()
    {
        return _position < _items.Count;
    }

    public T Next()
    {
        if (!HasNext()) throw new InvalidOperationException();
        return _items[_position++];
    }
}

// Concrete Collection
public class MyPlayList : IPlayList<string>
{
    private List<string> _songs = new List<string>();

    public void AddSong(string song)
    {
        _songs.Add(song);
    }

    public IIterator<string> CreateIterator()
    {
        return new PlayListIterator<string>(_songs);
    }
}

// Client
class Program
{
    static void Main(string[] args)
    {
        // Create playlist and add songs
        MyPlayList playList = new MyPlayList();
        playList.AddSong("Shape of You");
        playList.AddSong("Blinding Lights");
        playList.AddSong("Dance Monkey");

        // Use iterator to traverse
        IIterator<string> iterator = playList.CreateIterator();

        Console.WriteLine("Playing songs:");
        while (iterator.HasNext())
        {
            string song = iterator.Next();
            Console.WriteLine(song);
        }
    }
}
```
### Advantages of the Good Example

- Client doesn’t know how songs are stored → decoupled.  
- Changing internal storage doesn’t affect client code.  
- Supports multiple iterators simultaneously or complex traversal.  
- Cleaner and more maintainable code.  

Output :
```yaml
Playing songs:
Shape of You
Blinding Lights
Dance Monkey
```

>Notice: Output is the same, but internal structure and maintainability are **very different**.

<br>
<br>

### 📚 Example 2 : Book Library — Iterator Pattern Example
Scenario : 
একটা লাইব্রেরিতে অনেক বই আছে, এবং আমরা চাই সব বই একটার পর একটা print করতে — **internal data structure না জেনে**।


### ❌ Bad Example : 2 (Without Iterator — Wrong Approach)

### 🔴 Problem:
Client সরাসরি `List` access করছে → **tight coupling**।

```c#
using System;
using System.Collections.Generic;

class BadLibrary
{
    public List<string> Books = new List<string>();
}

class Program
{
    static void Main()
    {
        BadLibrary library = new BadLibrary();
        library.Books.Add("Clean Code");
        library.Books.Add("Design Patterns");
        library.Books.Add("C# in Depth");

        Console.WriteLine("Listing all books:");

        // Direct access → BAD
        for (int i = 0; i < library.Books.Count; i++)
        {
            Console.WriteLine(library.Books[i]);
        }
    }
}
```


### ❌ Problems:
- Client knows the internal structure (`List`)  
- Storage structure change করলে (`List → Array`) → **client code break করবে**  
- Custom traversal (reverse, skip, filter) → everywhere loop rewrite করতে হয়  

<br>

### ✅ Good Example (Using Iterator Pattern — Correct Approach)

### ✔ Step-by-step Components

| Component | Responsibility |
|----------|----------------|
| **IBookIterator** | `HasNext()`, `Next()` method define করে |
| **IBookCollection** | Iterator তৈরি করার responsibility |
| **BookIterator** | Actual traversal logic |
| **Library** | Collection of books |
| **Client** | শুধুমাত্র iterator ব্যবহার করে (কখনো list access করে না) |

### ✅ Code
```c#
using System;
using System.Collections.Generic;

// ===============================
//  ITERATOR INTERFACE
// ===============================
public interface IIterator<T>
{
    bool HasNext();
    T Next();
}

// ===============================
//  COLLECTION INTERFACE
// ===============================
public interface IBookCollection<T>
{
    IIterator<T> CreateIterator();
}

// ===============================
//  CONCRETE ITERATOR
// ===============================
public class BookIterator<T> : IIterator<T>
{
    private readonly List<T> _books;
    private int _index = 0;

    public BookIterator(List<T> books)
    {
        _books = books;
    }

    public bool HasNext()
    {
        return _index < _books.Count;
    }

    public T Next()
    {
        if (!HasNext()) 
            throw new InvalidOperationException("No more books!");

        return _books[_index++];
    }
}

// ===============================
//  CONCRETE COLLECTION
// ===============================
public class Library : IBookCollection<string>
{
    private readonly List<string> _books = new List<string>();

    public void AddBook(string book)
    {
        _books.Add(book);
    }

    public IIterator<string> CreateIterator()
    {
        return new BookIterator<string>(_books);
    }
}

// ===============================
//  CLIENT
// ===============================
class Program
{
    static void Main()
    {
        Library library = new Library();
        library.AddBook("Clean Code");
        library.AddBook("Design Patterns");
        library.AddBook("C# in Depth");

        IIterator<string> iterator = library.CreateIterator();

        Console.WriteLine("Listing all books:");

        while (iterator.HasNext())
        {
            Console.WriteLine(iterator.Next());
        }
    }
}
```


### ✅ OUTPUT
```yaml
Listing all books:
Clean Code
Design Patterns
C# in Depth
```

### ⭐ Why This Example Is Better

### ✔ 1. Client doesn’t know the data structure  
Client only uses iterator — not list/array.  
Library internally চাইলে যেকোনো সময়:

- `List`
- `Array`
- `LinkedList`
- `Tree`

এগুলোতে change করলেও client code break করবে না।


### ✔ 2. Supports Multiple Iterators  
Same Library object এর জন্য multiple iterator তৈরি করা যায়:

- Forward iterator  
- Backward iterator  
- Filter iterator  
(যেমন — শুধু “Programming” category এর books)

Iterator Pattern এ এগুলো খুব সহজে add করা যায়।



### ✔ 3. Clean, Maintainable & Extensible  
- Traversal logic iterator এ থাকে  
- Storage logic collection এ থাকে  
- Client শুধু iterator call করে  

Thus → **Separation of Concerns + Loose Coupling**



