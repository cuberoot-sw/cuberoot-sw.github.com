---
layout: post
title: "Custom product search scope in spree"
date: 2013-09-04 10:36
comments: true
author: Deepali
redirect_from: 
  - /2013/09/04/custom-product-search-scope-in-spree/
  - /blog/2013/09/04/custom-product-search-scope-in-spree/

---

I am using Spree 1.3.3 and I want to modify the product search scope in
spree.

####To modify the product serach scope for Store.

  Override the `lib/spree/core/search/base.rb` file.
  In this file override the protected method `get_base_scope`. Apply whatever condition you want to apply on `base_scope` in this method.

  Suppose I want to display only approved products on my store side so
the condition will be :

```ruby
def get_base_scope
  base_scope = Spree::Product.active
  base_scope = base_scope.where({:status => "approved"})
  base_scope = add_search_scopes(base_scope)
  base_scope
end
```

####To modify the product search scope for Admin.

  Override the `app/controllers/spree/admin/products_controller.rb`
file.
  In this file override the method `collection`.
   In this method apply what ever condition you want to add on `@search.result`
  
  Suppose I want to display only unaproved products at my admin side
then my collection method will be :

```ruby
def collection
    return @collection if @collection.present?
    params[:q] ||= {}
    params[:q][:deleted_at_null] ||= "1"

    params[:q][:s] ||= "name asc"

    @search = super.ransack(params[:q])
    @collection = @search.result.
      where({:status => "unapproved"}).
      group_by_products_id.
      includes(product_includes).
      page(params[:page]).
      per(Spree::Config[:admin_products_per_page])

    if params[:q][:s].include?("master_default_price_amount")
      # PostgreSQL compatibility
      @collection = @collection.group("spree_prices.amount")
    end
    @collection
end
```

And finally just restart your server and you are done.
