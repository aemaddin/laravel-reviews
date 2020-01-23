# Laravel Reviews (Overview)
**Laravel Reviews** package is useful for adding reviewing and rating system to your laravel web application. It is
useful for eCommerce, Blogs... etc.

**Laravel Reviews** package is easy to use in the following ways:
* Customize the reviewable model.
* Add review title.
* Add review body.
* Add review rate.
* Get average rate of specific review.
* You can choose to add either (title, body, or rate) to your review.

## How to install
### First:
Use the following `composer` command to install the package:
```
composer require melogail/laravel-reviews
```

### Second:
Publish package assets and resources using `artisan` command:
 ```
php artisan vendor:publish --tag=data
```
This command will make two major things:
1. Adding migration file to your `/database/migrations` directory.
2. Adding `laravel-reviews.php` config file to your `/config` directory
### Third:
Update your autoload files
```
composer dump-autoload -o
```
### Forth
By default, the `reviewable_id` is pointing to the `App\User` model, you can update your `reviewer_id`
pointer to any other model.
```php
require [
    /*
        |--------------------------------------------------------------------------
        | Eloquent Models
        |--------------------------------------------------------------------------
        */
    
        'models' => [
    
            'reviewer' => [
    
                'class' => App\User::class,  // model reviewer
    
            ]
    
        ]
];
```

## Usage:
Add the `Reviewable` trait to the model you want to add reviews to it.
```php
use Melogail\LaravelReviews\Reviewable;
 
class Product extends Model
{
 
    use Reviewable;
    
    // model code follows...
}
```
Reviews package table `reviews` has fields `title`, `body`, `rate`, `approved`, and `reviewer_id`

> If you want to return the data of the reviewer who added the review you must make a relationship between the 
> `reviews` table and the reviewer model.
>
> ex:
> ```php
> /*
>  * Inside User model
>  */
>  
> public function user() {
>   return $this->hasMany(\Melogail\LaravelReviews\Models\Review::class, 'reviewer_id', 'id');
>  
> }
>```

### Return All Reviews:
To return all reviews for specific object (ex: product):

```php
 foreach($product->reviews() as $review) {
  
    $review->title;
     
 }
```
### Add Review:
You use method `addReview($data)` to add review for you object. The method accept array of data as a parameter.

To add review to specific product:
```php
    $product->addReview([
        'title' => 'Review title',
         'body' => 'Review body',
         'rate' => 5,
         'approved' => true
    ]);
```
By default `approved` is set to `true`.

### Update Review:
The `updateReview()` method accepts three parameters:
1. `$reviewer_id`: Accept reviewer (ex: `Auth::id()`).
2. `$model_type`: In some situations you will need to add reviews to other parts of your application, adding the model namespace
 is important to tell the package which model the review is set to, ex (`App\Product`).
3. `$data`: The new data to be updated.
```php
    // Updating the review of authorized user
     
    $product->updateReview(Auth::id(), 'App\Product', [
                                                        'title' => 'New review title',
                                                        'body' => 'New review body',
                                                        'rate' => 3
                                                    ]); 
```

### Get Average Rate
To get the average rate of specific object -ex: product, use the `avgRate()` method. 
```php
    $product->aveRate();
```