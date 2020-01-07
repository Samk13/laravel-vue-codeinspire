# Laravel vue App


# Quick notes 

## exemple on how to create a validation for the request in userontroller:
```php
    public function store(Request $request)
    {
        // addng validation to the request 
        $this->validate($request, [
            'name' => 'required|string|max:255',
            'email'=>'required|string|email|max:255|unique:users',
            'password'=> 'required|string|min:3'
        ] );
        // create a user from the form request
        return User::create([
            'name' => $request['name'],
            'email'=> $request['email'],
            'type'=> $request['type'],
            'bio'=> $request['bio'],
            'photo'=> $request['photo'],
            'password'=> Hash::make($request['password'])
        ]);
    }
```

## when using vueRouter you should change the route in laravel "web.php" to this 
this will fix the problem when you hard reload the page and you get page not found 

```php
Route::get('/{any}', 'HomeController@index')->where('any', '.*');
```




