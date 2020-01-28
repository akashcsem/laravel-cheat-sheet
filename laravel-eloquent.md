<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>


# Laravel Eloquent

- Insert data
```php
    $flight = App\Flight::create(['name' => 'Flight 10']);
    or 
    $flight->fill(['name' => 'Flight 22']);
    
    // Retrieve flight by name, or create it if it doesn't exist...
    $flight = App\Flight::firstOrCreate(['name' => 'Flight 10']);
    
    // Retrieve flight by name, or create it with the name, delayed, and arrival_time attributes...
    $flight = App\Flight::firstOrCreate(
        ['name' => 'Flight 10'],
        ['delayed' => 1, 'arrival_time' => '11:30']
    );
    
    // Retrieve by name, or instantiate...
    $flight = App\Flight::firstOrNew(['name' => 'Flight 10']);
    
    // Retrieve by name, or instantiate with the name, delayed, and arrival_time attributes...
    $flight = App\Flight::firstOrNew(
        ['name' => 'Flight 10'],
        ['delayed' => 1, 'arrival_time' => '11:30']
    );
```


- Select specific columns
```php
  $products = Product::select('name', 'id', 'code')->get();
```
- Select first item/Select only one item
```php
  $products = Product::where('price', '=', 100)->first();
```
- Find item by id
```php
  $products = Product::find(1);
```

- Use where clause
```php
  $products = Product::where('price', '=', 100)->get();
  $products = Product::where('price', '<', 100)->get();
  $products = Product::where('price', '>', 100)->get();
  $products = Product::where('price', '>=', 100)->get();
  $products = Product::where('price', '<=', 100)->get();
  $products = Product::where('price', 'like', 100)->get();
  $products = Product::whereDate('created_at', '<=', $request->to_date);
```

- use whereIn
  <p> search multiple value in a column </p>
  ```php
      $purchases Purchase::whereIn('company_id', [1,2,4,8,12,20]);
  ```
  
  - use wherebetween
  <p> Compare value between two value </p>
  ```php
      $outWork = OutsideWork::whereBetween('date', [$from,$to])->get();
  ```

- Use multiple where clause
  <p>Multiple where clause is act as and operator</p>
```php
  $products = Product::where('price', '<', 100)->where('category_id', '=', 2)->get();
  or
  $products = Product::where([
    ['column_1', '=', 'value_1'],
    ['column_2', '<>', 'value_2'],
    [COLUMN, OPERATOR, VALUE]
  ]);
```
- Where and orWhere
  <p>(gender = 'Male' and age >= 18) or (gender = 'Female' and age >= 65)</p>

```php 
  $q->where(function ($query) {
      $query->where('gender', 'Male')
          ->where('age', '>=', 18);
  })->orWhere(function($query) {
      $query->where('gender', 'Female')
          ->where('age', '>=', 65);	
  })
```
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
  GoodsRequisitionDetails::groupBy('item_id')
            ->selectRaw('sum(quantity) as sum_quantity, item_id')
            ->get();
            
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

- Sum of two multiplied column
```php
    $data = Stock::whereDate('date', '<', Carbon::parse($from_date)->format('Y-m-d'))->get();
    $credit_amount = $data->sum(function($t){
        return $t->credit_qty * $t->credit_rate;
    });
 ```



