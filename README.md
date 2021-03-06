Acts As Immutable
=================

A Rails plugin that will ensure an ActiveRecord object is immutable once
saved. Optionally, you can specify attributes to be mutable if the object
is in a particular state (block evaluates to true).

Authored by: NuLayer Inc. / www.nulayer.com


Install
-------
```ruby
gem 'acts_as_immutable', github:'Dealermade/acts_as_immutable'
```

Usage examples
--------------

Ex. 1 All attributes will be immutable
```ruby
class PaymentTransaction < ActiveRecord::Base
    acts_as_immutable
end
```

Ex. 2
All attributes will be immutable except 'paid',
ie. if the invoice is still outstanding
```ruby
class Invoice < ActiveRecord::Base
    acts_as_immutable :paid do
        paid == false
    end
end
```

Ex. 3
Objects are immutable unless the subscription is
active or free in which case changes can be made
to: status, free and updated_at
```ruby
class Subscription < ActiveRecord::Base
    acts_as_immutable :status, :free, :updated_at do
        free? || active?
    end
        
    def active?
        status == "active"
    end
end
```

```bash
> sub = Subscription.new
> sub.status = "active"
> sub.free = false
> sub.start_date = Time.now
> sub.end_date = Time.now + 1.year
> sub.save
=> true
> sub.start_date = Time.now + 1.week
=> ActiveRecord::ActsAsImmutableError: start_date is an immutable attribute

> sub.update_attributes({ :start_date => Time.now + 1.week })
=> ActiveRecord::ActsAsImmutableError: start_date is an immutable attribute

> sub.status = "cancelled"
> sub.save
=> true
```

Testing
-------

Tested on Rails 4.2.0
Sorry no test suite


License (MIT)
-------------
Copyright (c) 2009 NuLayer Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
