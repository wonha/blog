---
title: "Common Library and Common Beans"
date: 2019-02-14T13:47:48+09:00
draft: true
---

I'd like to separate 'common beans' and 'common library(common logic)' in this post.
Because using common beans and common logic has different purpose.

# Common Beans

Creating common bean classes across the services will cause tight coupling among the services.
The beans could be silently updated without notifying, and it will cause integration problem to all the transitive dependency services
And especially with some languages like Java, defining common beans will restrict implementing behavior of the Value Object unless we inherit the class.
Since each service has it's own business logic and expect different behavior on the value object, using common beans doesn't make any sense

# Common Library

Services are going to be coupled more tightly
    Changes can not made easily
Versioning will be required at some point, and it is not just simple to keep track of versions especially when common library has business logic
Technical heterogenous