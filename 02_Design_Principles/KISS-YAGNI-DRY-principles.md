## 🔹 KISS Principle

👉 KISS = Keep It Simple, Stupid (কখনও বলা হয় Keep It Short and Simple)

### 📝 Definition 

The **KISS principle** means that software design should be kept **as simple as possible**. Unnecessary complexity should be avoided, because simple code is easier to read, test, debug, maintain, and extend.

>KISS principle এর মূল কথা হলো — কোডকে যতটা সম্ভব সোজা-সাপ্টা রাখা। বাড়তি জটিলতা আনলে কোড maintain, debug আর extend করা কঠিন হয়ে যায়। সহজ কোড লেখা মানে future এ problem কম হবে।

### ✅ Why KISS is Important?

- সহজ কোড বোঝা সহজ – নতুন developer সহজেই বুঝতে পারবে।
- Bug কম হয় – জটিল logic এ bug বেশি হয়।

- Maintain করা সহজ – future এ পরিবর্তন আনা সহজ।

- Readability বাড়ে – টিমে কাজ করলে সবাই দ্রুত বুঝতে পারে।

<br>

### 🏗️ Example of KISS ( Signup Form Validation )

ধরা যাক তুমি একটা user signup system বানাচ্ছ।

❌ Without KISS
```c#
public bool ValidateUser(User user)
{
    if (user != null)
    {
        if (!string.IsNullOrEmpty(user.Username))
        {
            if (!string.IsNullOrEmpty(user.Password) && user.Password.Length >= 8)
            {
                if (!string.IsNullOrEmpty(user.Email) && user.Email.Contains("@"))
                {
                    return true;
                }
                else{
                    return false;
                }
            }
            else{
                return false;
            }
        }
        else {
            return false;
        }
    }
    else{
        return false;
    }
}
```
👉 এখানে nested if-else এ কোডটা বোঝা, maintain করা কঠিন।

✅ With KISS (Simple & Clean)
```c#
public bool ValidateUser(User user)
{
    if (string.IsNullOrEmpty(user?.Username)) return false;
    if (string.IsNullOrEmpty(user?.Password) || user.Password.Length < 8) return false;
    if (string.IsNullOrEmpty(user?.Email) || !user.Email.Contains("@")) return false;

    return true;
}
```

👉 প্রতিটা condition আলাদা, readable, maintainable — KISS principle follow করেছে।


### 📌 Conclusion:
KISS principle মানে হলো – কাজটা যতটা সম্ভব ছোট, সরল আর পরিষ্কারভাবে করা।

<br>
<br>

## 🔹 YAGNI Principle

👉 YAGNI = You Aren’t Gonna Need It

### 📝 Definition 

The **YAGNI principle** means: **“Don’t add functionality until it is actually necessary.”**
Developers often over-engineer by adding extra features “just in case” — but most of those features never get used. This increases complexity, bugs, and maintenance cost.


>YAGNI principle এর মানে হলো:
“যতক্ষণ পর্যন্ত দরকার না হচ্ছে, ততক্ষণ পর্যন্ত অপ্রয়োজনীয় feature বা code লিখবে না।”
কারণ অতিরিক্ত feature = অতিরিক্ত জটিলতা + bug + maintain করার ঝামেলা।

### ✅ Why YAGNI is Important?

- Saves Time – Focus only on the required features.

- Reduces Complexity – Keeps the code simple.

- Easier Maintenance – No need to maintain unused features.

- Keeps Focused – Stays aligned with the real business requirements.

<br>

### 🏗️  Example ( Blog Website )
### ❌ Without YAGNI (Over-Engineering)
```c#
public class BlogPost
{
    public string Title { get; set; }
    public string Content { get; set; }

    // Future-এর কথা ভেবে অপ্রয়োজনীয় property যোগ করা হয়েছে
    public int Likes { get; set; }
    public int Shares { get; set; }
    public bool AllowDonations { get; set; }
    public List<string> Tags { get; set; }
    public List<string> RelatedArticles { get; set; }

    public void TranslateToSpanish()
    {
        // Maybe useful later, not now
    }

    public void GenerateAudioVersion()
    {
        // Not needed yet
    }
}
```

👉 এখানে অনেক property/method এখনই দরকার নেই, শুধু ভবিষ্যতের জন্য ভেবে যোগ করা হয়েছে। এতে complexity বাড়ে।

### ✅ With YAGNI (Keep Only What’s Needed Now)
```c#
public class BlogPost
{
    public string Title { get; set; }
    public string Content { get; set; }
}
```

👉 কেবল blog লিখতে এবং publish করতে যতটুকু দরকার শুধু সেটাই রাখা হলো।
লাইক, শেয়ার, translate — এগুলো future এ দরকার হলে তখন যোগ করা যাবে।

### 📌 Summary:
YAGNI principle এ বলে — “Don’t write extra features today for tomorrow’s assumptions. Build only what is required right now.”

<br>
<br>


## 🔹 DRY Principle

👉 DRY = Don’t Repeat Yourself

### 📝 Definition

The **DRY principle** means: **“Every piece of knowledge should have a single, unambiguous representation in a system.”**
In simple words: Avoid duplicating code. If the same logic is repeated in multiple places, put it in one place (like a function, method, or class) and reuse it.


DRY principle এর মানে হলো:
“একই কোড বা লজিক বারবার লিখবে না। একবার লেখো, তারপর যেখানেই দরকার reuse করো।”
কারণ duplicate code মানে — bug fix, update, maintain সব জায়গায় বারবার পরিবর্তন করতে হয়।

### ✅ Why DRY is Important?

- Less Code – No need to repeat the same logic.

- Fewer Bugs – Change in one place reflects everywhere.

- Easier Maintenance – Future updates are easier.

- Reusability – Functions and classes can be reused.

### 🏗️  Example 3: Login Validation (DRY)
❌ Without DRY (Repeated Code)
```c#
public class LoginService
{
    public bool ValidateUsername(string username)
    {
        if (string.IsNullOrEmpty(username)) return false;
        return true;
    }

    public bool ValidatePassword(string password)
    {
        if (string.IsNullOrEmpty(password)) return false;
        return true;
    }
}
```


Null check বারবার লেখা হয়েছে → duplicate logic → DRY ভঙ্গ।

✅ With DRY (Reusable Method)
```c#
public class LoginService
{
    private bool IsNotEmpty(string input) => !string.IsNullOrEmpty(input);

    public bool ValidateUsername(string username) => IsNotEmpty(username);
    public bool ValidatePassword(string password) => IsNotEmpty(password);
}
```


IsNotEmpty() একবার লেখা → username এবং password validation এ reuse।

কোড ছোট, readable এবং maintainable।

### 💡 Takeaway:
**DRY principle** মানে হলো: “একবার লিখো, বারবার ব্যবহার করো”।