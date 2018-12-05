
## Hash#dig

 - How many times did you do something like this:

 ```ruby
  ... if params[:user] && params[:user][:address] && params[:user][:address][:somewhere_deep]

```

## Object#presence_in

```ruby
  sort_options = [:by_date, :by_title, :by_author]
  ...
  sort = sort_options.include?(params[:sort])
    ? params[:sort]
    : :by_date
  # Another option
  sort = (sort_options.include?(params[:sort]) && params[:sort]) || :by_date

```

<strong>Doesn’t this look better?</strong>

```ruby
  params[:sort].presence_in(sort_options) || :by_date
```

## Module#alias_attribute

It had a table with weird column names like SERNUM_0, ITMDES1_0 and other. We mapped this table to an ActiveRecord model and instead of writing queries and scopes on it like WeirdTable.where(SERNUM_0: ‘123’) , we came up to using alias_attribute since it doesn’t just generate getter and setter (as well as a predicate method), but works in queries as well:

```ruby
  alias_attribute :name, :ITMDES1_0
  ...
  scope :by_name, -> (name) { where(name: name) }
```

## Object#presence

This one is more popular than others. There’s pretty good explanation on  <a href="https://apidock.com/rails/Object/presence">ApiDock</a>. So, object.presence is equivalent to:

```ruby
  object.present? ? object : nil
```

## Module#delegate

```ruby
class Profile < ApplicationRecord
  belongs_to :user
  delegate :email, to: :user
end
...
profile.email # equivalent to profile.user.email
```
