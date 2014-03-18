[![Build Status](https://secure.travis-ci.org/pyromaniac/mongoid_orderable.png)](http://travis-ci.org/pyromaniac/mongoid_orderable)

# What?

Mongoid::Orderable is a ordered list implementation for your mongoid models.

# Why?

* It uses native mongo batch increment feature
* It supports mutators api
* It correctly assigns the position while moving document between scopes
* It supports mongoid 2, 3 and 4

# How?

```
gem 'mongoid_orderable'
```

Gem has the same api as others. Just include Mongoid::Orderable into your model and call `orderable` method.

```
class Item
  include Mongoid::Document
  include Mongoid::Orderable

  # belongs_to :group
  # belongs_to :drawer, class_name: "Office::Drawer",
  #            foreign_key: "legacy_drawer_key_id"

  # orderable
  # orderable scope: :group, column: :pos
  # orderable scope: :drawer, column: :pos # resolves scope foreign key from relation
  # orderable scope: 'drawer', column: :pos # but if you pass a string - it will use it as is, as the column name for scope
  # orderable scope: lambda { |document| where(group_id: document.group_id) }
  # orderable index: false # this one if you want specify indexes manually
  # orderable base: 0 # count position from zero as the top-most value (1 is the default value)
end
```

# Usage

```
item.move_to 2 # just change position
item.move_to! 2 # and save
item.move_to = 2 # assignable method

# symbol position
item.move_to :top
item.move_to :bottom
item.move_to :higher
item.move_to :lower

# generated methods
item.move_to_top
item.move_to_bottom
item.move_higher
item.move_lower

item.lower_items # return a collection of items lower on the list
item.higher_items # return a collection of items higher on the list

item.next_item # returns the next item in the list
item.previous_item # returns the previous item in the list
```

# Contributing

Fork && Patch && Spec && Push && Pull request.

# License

Mongoid::Orderable is released under the MIT license.
