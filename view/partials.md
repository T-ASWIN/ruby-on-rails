Good question 👍
I’ll show you **exactly how to use `options` inside a partial**, step-by-step, with **real examples**.

---

## 1️⃣ Where `options` comes from (quick recap)

In a partial:

```erb
<% local_assigns => { product:, **options } %>
```

This means:

* `product` → required data
* `options` → **all extra locals** passed to the partial

---

## 2️⃣ How to USE `options` (most common ways)

### ✅ 1. Pass `options` to HTML tags (MOST COMMON)

### `show.html.erb`

```erb
<%= render "product",
      product: @product,
      class: "card highlight",
      data: { role: "admin" }
%>
```

### `_product.html.erb`

```erb
<% local_assigns => { product:, **options } %>

<%= tag.div id: dom_id(product), **options do %>
  <h2><%= product.name %></h2>
<% end %>
```

### Output

```html
<div id="product_1" class="card highlight" data-role="admin">
  <h2>Pen</h2>
</div>
```

👉 `**options` spreads everything into HTML attributes.

---

## 3️⃣ Using `options` conditionally

```erb
<% local_assigns => { product:, **options } %>

<% if options[:highlight] %>
  <strong>Featured</strong>
<% end %>
```

Render:

```erb
<%= render "product", product: @product, highlight: true %>
```

---

## 4️⃣ Providing defaults for options (safe way)

```erb
<% local_assigns.with_defaults(class: "card") => { product:, **options } %>

<%= tag.div **options do %>
  <%= product.name %>
<% end %>
```

If `class` is not passed → `"card"` is used.

---

## 5️⃣ Using options for logic (not just HTML)

```erb
<% local_assigns => { product:, **options } %>

<% price = options[:show_price] == false ? nil : product.price %>

<%= product.name %>
<%= price %>
```

Render:

```erb
<%= render "product", product: @product, show_price: false %>
```

---

## 6️⃣ Real-world example (button partial)

### `index.html.erb`

```erb
<%= render "button",
      text: "Save",
      class: "btn btn-primary",
      disabled: true
%>
```

### `_button.html.erb`

```erb
<% local_assigns => { text:, **options } %>

<%= tag.button text, **options %>
```

✔ Reusable
✔ Clean
✔ Flexible

---

## 7️⃣ What NOT to do ❌

❌ Don’t access `local_assigns[:class]` everywhere
❌ Don’t mix required data into `options`
❌ Don’t use this for simple partials

---

## 8️⃣ Mental rule (easy to remember)

```
Required data  → named variables
Extra config  → options (**options)
```

---

## 🎯 Interview-ready explanation

> Options extracted from `local_assigns` are typically used to pass flexible configuration or HTML attributes into a partial and can be splatted directly into helper methods.

---

## ✅ You’re at the right level now

You understand:
✔ partials
✔ locals
✔ `local_assigns`
✔ pattern matching
✔ `options` usage

If you want next, I can show:

* strict locals vs options
* performance impact
* Rails core examples
* when NOT to use this pattern

Just tell me 🚀
