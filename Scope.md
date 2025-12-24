

## 🔹 Simple example

```rb
class Book < ApplicationRecord
  scope :out_of_print, -> { where(out_of_print: true) }
end
```

### Use it like this

```rb
Book.out_of_print
```

👉 Means: *“Give me all out-of-print books”*

---

## 🔹 Why scopes are useful

✅ Clean code
✅ Reusable queries
✅ Easy to read
✅ Chainable

---

# 1️⃣ Calling scopes

### On the model

```rb
Book.out_of_print
```

### On an association

```rb
author.books.out_of_print
```

👉 Same scope works everywhere

---

# 2️⃣ Chaining scopes

```rb
class Book < ApplicationRecord
  scope :out_of_print, -> { where(out_of_print: true) }
  scope :expensive, -> { where("price > 500") }
end
```

```rb
Book.out_of_print.expensive
```

👉 Combines conditions using **AND**

---

# 3️⃣ Scopes with arguments

```rb
class Book < ApplicationRecord
  scope :costs_more_than, ->(amount) { where("price > ?", amount) }
end
```

```rb
Book.costs_more_than(100)
```

👉 Same as a class method
👉 But **scopes return relations**, so they chain nicely

---

# 4️⃣ Conditional scopes

```rb
class Order < ApplicationRecord
  scope :created_before, ->(time) {
    where(created_at: ...time) if time.present?
  }
end
```

### Important rule

* Scope → **always returns a relation**
* Class method → may return `nil`

👉 Scopes are safer when chaining

---

# 5️⃣ `default_scope` (Applied everywhere)

```rb
class Book < ApplicationRecord
  default_scope { where(out_of_print: false) }
end
```

Now:

```rb
Book.all
```

Automatically becomes:

```sql
WHERE out_of_print = false
```

---

### Default scope affects `new`

```rb
Book.new.out_of_print
# => false
```

Remove it:

```rb
Book.unscoped.new
```

---

⚠️ Be careful:
**default_scope can cause confusion** if overused.

---

# 6️⃣ Merging scopes

Scopes combine using **AND**

```rb
Book.out_of_print.old
```

```sql
WHERE out_of_print = true AND year_published < 1969
```

---

### Mix scope + where

```rb
Book.in_print.where(price: ...100)
```

👉 Still AND

---

### Let last condition win (`merge`)

```rb
Book.in_print.merge(Book.out_of_print)
```

👉 Replaces earlier condition

---

# 7️⃣ `unscoped` (Remove all scopes)

```rb
Book.unscoped.all
```

👉 Ignores:

* `default_scope`
* other scopes

---

### Block form

```rb
Book.unscoped { Book.out_of_print }
```

👉 Only applies the scope inside the block

---
