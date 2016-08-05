# Bigcommerce-Private-App-Php
Connect PHP applications with the Bigcommerce Platform https://developer.bigcommerce.com
Bigcommerce API Client
======================

PHP client for connecting to the Bigcommerce V2 REST API.

Requirements for this connection
------------

- PHP 5.3 or greater
- cUrl extension enabled

**To connect to the API with basic auth you need the following:**

- Secure URL pointing to a Bigcommerce store
- Username of an authorized admin user of the store
- API key for the user

To generate an API key, go to Control Panel > Users > Edit User and make sure
the 'Enable the XML API?' is ticked.

**To connect to the API with OAuth you will need the following:**

- client_id
- auth_token
- store_hash

Installation
------------

Use the following Composer command to install the
API client from [the Bigcommerce vendor on Packagist](https://packagist.org/packages/bigcommerce/api):

~~~shell
 $ composer require bigcommerce/api
 $ composer update
~~~

You can also install composer for your specific project by running the following in the library folder.

~~~shell
 $ curl -sS https://getcomposer.org/installer | php
 $ php composer.phar install
 $ composer install
~~~
***********************************************************************************
In this file i have upload the vendor file so no need to install  composer
***********************************************************************************
Namespace
---------

All the examples below assume the `Bigcommerce\Api\Client` class is imported
into the scope with the following namespace declaration:

~~~php
use Bigcommerce\Api\Client as Bigcommerce;
~~~

Configuration
-------------

To use the API client in your PHP code, ensure that you can access `Bigcommerce\Api`
in your autoload path (using Composer’s `vendor/autoload.php` hook is recommended).

Provide your credentials to the static configuration hook to prepare the API client
for connecting to a store on the Bigcommerce platform:

### Basic Auth
~~~php
Bigcommerce::configure(array(
	'store_url' => 'https://store.mybigcommerce.com',
	'username'	=> 'admin',
	'api_key'	=> 'd81aada4xc34xx3e18f0xxxx7f36ca'
));
~~~

### OAuth
~~~php
Bigcommerce::configure(array(
    'client_id' => 'xxxxxxxxxxxxx',
    'auth_token' => 'xxxxxxxxxxxx',
    'store_hash' => 'xxxxxxxxxxx'
));
~~~

Connecting to the store
-----------------------

To test that your configuration was correct and you can successfully connect to
the store, ping the getTime method which will return a DateTime object
representing the current timestamp of the store if successful or false if
unsuccessful:

Accessing collections and resources (GET)
-----------------------------------------

To list all the resources in a collection:

~~~php
$products = Bigcommerce::getProducts();

foreach ($products as $product) {
	echo $product->name;
	echo $product->price;
}
~~~

To view the total count of resources in a collection:

~~~php
$count = Bigcommerce::getProductsCount();

echo $count;
~~~
Paging and Filtering
--------------------

~~~php
$filter = array("page" => 3, "limit" => 30);

$products = Bigcommerce::getProducts($filter);
~~~

To filter a collection, you can also pass parameters to filter by as key-value
pairs:

~~~php
$filter = array("is_featured" => true);

$featured = Bigcommerce::getProducts($filter);
~~~
See the API documentation for each resource for a list of supported filter
parameters.

Updating existing resources (PUT)
---------------------------------

To update a single resource:

~~~php
$product = Bigcommerce::getProduct(11);

$product->name = "MacBook Air";
$product->price = 99.95;
$product->update();
~~~

You can also update a resource by passing an array or stdClass object of fields
you want to change to the global update method:

~~~php
$fields = array(
	"name"  => "MacBook Air",
	"price" => 999.95
);

Bigcommerce::updateProduct(11, $fields);
~~~

Creating new resources (POST)
-----------------------------

Some resources support creation of new items by posting to the collection. This
can be done by passing an array or stdClass object representing the new
resource to the global create method:

~~~php
$fields = array(
	"name" => "Apple"
);

Bigcommerce::createBrand($fields);
~~~

You can also create a resource by making a new instance of the resource class
and calling the create method once you have set the fields you want to save:

~~~php
$brand = new Bigcommerce\Api\Resources\Brand();

$brand->name = "Apple";
$brand->create();
~~~

Deleting resources and collections (DELETE)
-------------------------------------------

To delete a single resource you can call the delete method on the resource object:

~~~php
$category = Bigcommerce::getCategory(22);
$category->delete();
~~~

You can also delete resources by calling the global wrapper method:

~~~php
Bigcommerce::deleteCategory(22);
~~~

Some resources support deletion of the entire collection. You can use the
deleteAll methods to do this:

~~~php
Bigcommerce::deleteAllOptionSets();
~~~
