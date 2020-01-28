<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>


# Laravel Eloquent
- Get data with pagination
```php
  $posts = Post::paginate(30);
  
  Use in blade/view, show the pagination
  {{ $posts->links() }}
```
- Count total row
```php
  $posts = Post::where('status', 1)->get();
  $totalPost = $posts->count();
  or 
  $totalPost = count($posts);
  or
  $totalPost = Post::where('status', 1)->count();
 ```
 
 - Get sum of a column
 ```php
  $totalPrice = Product::sum('price');
  or 
  $products = Product::all();
  $totalPrice = collect($products)->sum('price');
```
  
- Get data with many relationship
```php
  $relations = [''company', 'department', 'goods_requisition_details.item.item_stock'];
  $goods_requisitions = GoodsRequisition::with($relations)->orderByDesc('id')->get();
```

- Get key and value
```php
  # In controller
  $categories = Category::pluck('name', 'id');
  
  # In blade/view, where most use
  <select name="company_id" class="form-control chosen-select">
      <option selected value="">select</option>
      @foreach($categories as $id => $name)
          <option value="{{ $id }}">{{ $name }}</option>
      @endforeach
  </select>
```
- Get sum of a column from has many relationship


```php
  $purchases = Purchase::withCount([
'purchase_details AS detail' => function ($query) {
            $query->select(DB::raw("SUM(quantity) as totalQty"));
        }
    ]);
    
    it return "detail_count" => "320.00"
  or
  # In controller
  $purchases = Purchase::with(['purchase_details' => function($q){
                $q->select(DB::raw('sum(quantity) as totalQty, purchase_id'))->groupBy('purchase_id');
            }])->get();
 
```

```php
  # In blade/view
  {{ optional($purchase->purchase_details->first())->totalQty }}
```



