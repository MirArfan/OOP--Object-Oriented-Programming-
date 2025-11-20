# 🧩 Intrinsic vs Extrinsic State (Flyweight Pattern Core Concept)


### 🟦 Intrinsic State (Shared State)

  
Intrinsic state is the data that never changes and can be shared between multiple objects. It is stored inside the Flyweight object.


Intrinsic state হলো সেই ডেটা যা পরিবর্তন হয় না এবং অনেকগুলো অবজেক্টের মধ্যে শেয়ার করা যায়। এটিই Flyweight অবজেক্টের ভেতরে থাকে।

---

### 🟪 Extrinsic State (Unique / Context-Specific State)


Extrinsic state is the data that varies between objects and cannot be shared. It is stored outside the Flyweight and passed when needed.


Extrinsic state হলো সেই ডেটা যা অবজেক্টভেদে ভিন্ন হয় এবং শেয়ার করা যায় না। এটি Flyweight-এর বাইরে রাখা হয় এবং প্রয়োজন হলে পাঠানো হয়।

---

### 🧠 Why Distinguish Intrinsic & Extrinsic?

### English  
To reduce memory usage by storing shared (intrinsic) data once and keeping only the unique (extrinsic) data separately.


Memory কমানোর জন্য shared (intrinsic) data একবার রাখা হয় এবং unique (extrinsic) data আলাদা করে রাখা হয়।

---

### 🟩 Real-Life Examples (Intrinsic vs Extrinsic)

### Example 1: Fonts in Document Editor (MS Word)

**Intrinsic (Shared):** Font style, font family, character shape  
**Extrinsic (Unique):** Characters typed (A, B, C...), size, color, position

---

### **Example 2: Game Trees (Open World Game)**

**Intrinsic:**  
- Tree model  
- Texture  

**Extrinsic:**  
- Position (X, Y)  
- Height  
- Rotation  

---

### **Example 3: Chess Pieces**

**Intrinsic:** Shape of Bishop, Knight, Queen  
**Extrinsic:** Position on board (A1, B4, etc.)

---

### **Example 4: Emoji in Messenger**

**Intrinsic:** Emoji design (😀)  
**Extrinsic:** Sender, size, timestamp

---

### **Example 5: Car Models in Racing Game**

**Intrinsic:** Model 3D shape, texture  
**Extrinsic:** Current speed, position, damage level

---

### 🧩 How Intrinsic & Extrinsic Work Together


- Intrinsic data stays in the Flyweight object (stored once).  
- Extrinsic data is provided from outside whenever drawing or using the Flyweight.


- Intrinsic ডেটা Flyweight ক্লাসের ভিতরে একবারই থাকে।  
- Extrinsic ডেটা বাইরে থেকে পাঠানো হয় যখন অবজেক্ট ব্যবহার করা হয়।

---

### 🟦 Summary Table

| Concept    | Intrinsic | Extrinsic |
|------------|-----------|-----------|
| **Meaning** | Shared, constant data | Variable, unique data |
| **Stored where?** | Inside Flyweight object | Outside Flyweight |
| **Can be shared?** | ✔ Yes | ❌ No |
| **Purpose** | Reduce memory | Provide object uniqueness |

---

### 🟩 Super Simple Example

### 🍕 Pizza

- **Intrinsic:** Base size, shape, dough type  
- **Extrinsic:** Toppings, quantity, extra cheese  


