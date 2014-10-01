---
layout: post
title: "Ajax Pagination In Rails"
date: 2013-10-22 10:47
comments: true
author: Deepali
redirect_from: 
  - /2013/10/22/ajax-pagination-in-rails/
  - /blog/2013/10/22/ajax-pagination-in-rails/

---
In this blog post we will see how to apply rails pagination on Ajax.
Here, I am doing pagination using `kaminari`.

1.Add following gem in your `Gemfile`.

```ruby
gem kaminari
```

2.bundle install

3.Open your controller file in which you want to apply pagination.

```ruby
def index
  @products = Product.page(params[:page]).per(10)
		respond_to do |format|
		  format.js
		  format.html
	end
end
```

4.In view file index.html.haml.

```ruby
%div#products
	  = render :partial => '/products/product', :locals => {:products => @products}

	#paginate	     
	  = paginate @products, :remote => true
```

5.Add following code in `_product.html.haml` file.

```ruby
%table
  @products.each do |product|
  %tr
 	  %td= product.name
```

6.Create new file `_index.js.haml` and add following code in it.

```ruby
$('#products').html('<%= escape_javascript render(:partial => '/products/product', :locals => {:products => @products}) %>');
$('#paginator').html('<%= escape_javascript(paginate(@products, :remote => true).to_s) %>');     
```

And it's done! pagination works with ajax.
