# CSRF token with htmx in Django templates


## In a specific element

```html
<button
  class="btn btn-primary"
  type="button"
  hx-post="{{ object.delete_item_url }}"
  hx-target="#item-{{ object.item.id }}"
  hx-swap="outerHTML"
  hx-trigger="click"
  hx-headers='{"X-CSRFToken": "{{ csrf_token }}"}'
>
  Delete item
</button>
```

## in the body tag

```html
<body hx-headers='{"X-CSRFToken": "{{csrf_token }}"}'>
  <!-- here comes content -->
</body>
```
