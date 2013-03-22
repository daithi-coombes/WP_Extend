WP_Extend
=========

A class wrapper to provide extending capabilities to awkward wordpress core class that throw errors when extended.

Blog url:
http://david-coombes.com/extending-wordprss-classes-with-wp_extend/

Usage
=====
```php
class MyClass extends WP_Extend(){
 
 	//required
    function __construct(){
        $this->extend_filter('wp_classname_filter', __CLASS__);
    }
 	
 	//required - abstract
    function __call(){
        if(!method_exists($this, $method))
	        return parent::__call($method, $args);
    }

    //example - overwrite a method as normal
    function my_overwrite_method(){

    	/**
    	 * Helper method. This will automatically trying calling the required parent class method 
    	 * of the same name, and automatically pass any arguments/paramters.
    	 */
    	return $this->parent();
    }
}

/**
 * In wp core example
 * The object gets constructed and used as normal
 */
$class_name = do_filter('wp_classname_filter');
$wp_obj = new $class_name();
$wp_obj->method_not_overwritten();	//methods not overwritten will get called and run as if it were extended

/**
 * My plugin/theme
 * you can now call your overwritten methods and access the required class as if it were extended
 */
global $wp_obj;

$wp_obj->method_not_overwritten();
$wp_obj->my_overwrite_method();
```