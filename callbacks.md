
---

## ✅ MOST USED RAILS CALLBACKS (YOU SHOULD KNOW THESE)

### 1️⃣ `before_validation` ⭐⭐⭐⭐

**Very common**

👉 Used to **prepare or clean data before validations run**

```ruby
before_validation :normalize_email
```

Use cases:

* Strip spaces
* Set default values
* Auto-fill fields

---

### 2️⃣ `after_validation` ⭐⭐

Used to **log or inspect validation errors**

```ruby
after_validation :log_errors
```

Not used as often, but good for debugging.

---

### 3️⃣ `before_save` ⭐⭐⭐⭐⭐ (MOST USED)

👉 Runs **before create & update**

```ruby
before_save :capitalize_name
```

Use cases:

* Formatting data
* Hashing passwords
* Final cleanup before save

---

### 4️⃣ `after_save` ⭐⭐⭐

👉 Runs **after create & update**

```ruby
after_save :clear_cache
```

Use cases:

* Cache updates
* Notifications
* Syncing data

---

### 5️⃣ `before_create` ⭐⭐⭐⭐

👉 Runs **only when record is created first time**

```ruby
before_create :set_default_role
```

Use cases:

* Generate tokens
* Set default values

---

### 6️⃣ `after_create` ⭐⭐⭐⭐

👉 Runs **only after successful create**

```ruby
after_create :send_welcome_email
```

Use cases:

* Emails
* Audit logs
* Analytics

---

### 7️⃣ `before_update` ⭐⭐

👉 Runs **only on update**

```ruby
before_update :check_status_change
```

Use cases:

* Track changes
* Prevent invalid updates

---

### 8️⃣ `after_update` ⭐⭐

👉 Runs **after update**

```ruby
after_update :notify_admin
```

---

### 9️⃣ `before_destroy` ⭐⭐⭐

👉 Runs **before deleting a record**

```ruby
before_destroy :ensure_not_admin
```

Use cases:

* Prevent delete
* Cleanup checks

---

### 🔟 `after_destroy` ⭐⭐

👉 Runs **after deletion**

```ruby
after_destroy :log_deletion
```

---

## 🔥 MOST IMPORTANT 5 (MEMORIZE THESE)

If you remember only these, you’re good:

1. `before_validation`
2. `before_save`
3. `before_create`
4. `after_create`
5. `before_destroy`

---
---
They trigger on:

```ruby
person.things << thing
person.things.delete(thing)
```

---

## 2️⃣ Simple use case for `Person`

Let’s say:

* A **Person can have many Tasks**
* A **Person can have at most 3 tasks**

---

## 3️⃣ Create the association

### Models

### `app/models/person.rb`

```ruby
class Person < ApplicationRecord
  has_many :tasks, before_add: :check_task_limit

  private

  def check_task_limit(_task)
    if tasks.count >= 3
      errors.add(:base, "Cannot add more than 3 tasks")
      throw(:abort)
    end
  end
end
```

---

### `app/models/task.rb`

```ruby
class Task < ApplicationRecord
  belongs_to :person
end
```

---

## 4️⃣ Migration (tables)

### Person table (already exists)

```ruby
# people
# id, name
```

### Task table

```bash
bin/rails generate model Task title:string person:references
bin/rails db:migrate
```

---

## 5️⃣ Try it in Rails console (IMPORTANT)

```bash
bin/rails console
```

```ruby
p = Person.create(name: "Aswin")

p.tasks.create(title: "Task 1")
p.tasks.create(title: "Task 2")
p.tasks.create(title: "Task 3")
```
