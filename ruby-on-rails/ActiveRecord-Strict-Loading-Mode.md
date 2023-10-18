N+1 queries probably are the most common performance issues. Since Rails 6.1, we can set a *strict_loading* configuration that,
when *true*, will throw an error when the code attempts to lazy load any association without explicitly including the association
in the ActiveRecord query.

The following snippet is an example of N+1 query:
```
class Order < ApplicationRecord
  has_many :order_items
end

class OrderItem < ApplicationRecord
  belongs_to :order
end

# In the controller
@order = Order.first

# In the partial
<% @order.order_items.each do |order_item| %>
  <p><%= order_item.price %></p>
<% end %>
```

There is a few ways to enable strict_loading, the first way is calling the method *strict_loading* while writing your query:

```
Order.string_loading.where(id: 1).each.order_item do |item|
  pp item
end
```
output:
```
`Order` is marked for strict_loading. The OrderItem association named `:order_items` cannot be lazily loaded. (ActiveRecord::StrictLoadingViolationError)
```

The second way is defining the flag for strict_loading per model as the following:

```
class Order < ApplicationRecord
  self.strict_loading_by_default = true

  has_many :order_items
end
```

The third way is setting the flag on the environment file, which sets the strict_loading to all ActiveRecord models:

```
config.active_record.strict_loading_by_default = true
```


