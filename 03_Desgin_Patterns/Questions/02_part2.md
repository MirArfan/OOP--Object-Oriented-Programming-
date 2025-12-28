# Design Pattern Comparisons

### 1️⃣ Strategy vs Template Method
| Feature | Strategy | Template Method |
|---------|---------|----------------|
| Purpose | Choose algorithm at runtime | Define algorithm skeleton & let subclasses override steps |
| Flexibility | High, behavior can change at runtime | Steps fixed, only certain steps can be overridden |
| Class dependency | Usually interface + context | Base class + subclass |
| Use Case | Payment method selection | Data processing pipeline |

---

### 2️⃣ Factory vs Abstract Factory
| Feature | Factory | Abstract Factory |
|---------|---------|----------------|
| Purpose | Create single type of object | Create family of related objects |
| Structure | One factory class | Multiple factories (Factory of factories) |
| Use Case | Payment: Bkash/Nagad/Card | GUI: Button + Checkbox for same theme |
| Flexibility | Limited | High, scalable |

---

### 3️⃣ Decorator vs Adapter
| Feature | Decorator | Adapter |
|---------|-----------|---------|
| Purpose | Add behavior at runtime | Make interfaces compatible (wrap) |
| Flexibility | High, multiple combinations | One-to-one interface mapping |
| Example | Coffee + Milk + Sugar | Power plug / API integration |

---

### 4️⃣ Observer vs Pub-Sub
| Feature | Observer | Pub-Sub |
|---------|----------|---------|
| Purpose | Auto-update objects when subject changes | Decoupled publish/subscribe mechanism |
| Coupling | Tightly coupled (subject knows observers) | Loosely coupled (via broker) |
| Example | MVC, GUI | Event bus, Message queue |

---

### 5️⃣ Command vs Strategy
| Feature | Command | Strategy |
|---------|---------|---------|
| Purpose | Encapsulate request as object | Choose algorithm dynamically |
| Focus | Action / request | Behavior / algorithm |
| Use Case | GUI button actions, Undo/Redo | Payment method selection, Sorting |

---

### 6️⃣ State vs Strategy
| Feature | State | Strategy |
|---------|-------|---------|
| Purpose | Behavior depends on internal state | Behavior chosen externally at runtime |
| Context change | Internal | External |
| Example | TCP connection: Open/Close | Payment method: Bkash/Nagad/Card |

---

### 7️⃣ Facade vs Proxy
| Feature | Facade | Proxy |
|---------|--------|-------|
| Purpose | Simplify complex subsystem | Control access to object |
| Use Case | Payment gateway facade | Remote object, Security, Lazy loading |
| Example | PaymentFacade.pay() | Image proxy, Virtual proxy |

---

### 8️⃣ Strategy vs State
| Feature | Strategy | State |
|---------|---------|------|
| Purpose | Algorithm selection | Behavior depends on state |
| Client vs Internal | Client sets strategy | Internal state changes behavior |
| Example | Sorting algorithm | TCP connection state |

---

### 9️⃣ Decorator vs Inheritance
| Feature | Decorator | Inheritance |
|---------|-----------|------------|
| Purpose | Add behavior at runtime | Extend behavior at compile-time |
| Flexibility | High, multiple combinations | Fixed hierarchy |
| Example | Coffee + Milk + Sugar | Coffee subclass CoffeeWithMilk |

---

### 🔟 Adapter vs Facade
| Feature | Adapter | Facade |
|---------|---------|-------|
| Purpose | Convert interface | Simplify subsystem |
| Example | API adapter | Payment gateway facade |

---

### 1️⃣1️⃣ Observer vs Event Listener
| Feature | Observer | Event Listener |
|---------|---------|----------------|
| Coupling | Subject knows observers | Usually via event dispatcher, loosely coupled |
| Usage | MVC, GUI update | JS DOM events, GUI events |
| Focus | State change notify | Event triggered notify |
