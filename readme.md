## Laravel multilingual site example

In this repository you will find a little example on how to do multilingual sites that can be accessed by changing the language segment in the url, defining supported languages and a fallback when one is not available. 

This is just based on the url you use to access your site, any other type of language detection is left out of this example. Knowing this, if you want to access the French version of your site the url goes from http://site.com/ to http://site.com/fr.

The base for this example was taken from [Marcin Nabiałek](http://stackoverflow.com/users/3593996/marcin-nabia%C5%82ek) in [this](http://stackoverflow.com/questions/25082154/how-to-create-multilingual-translated-routes-in-laravel) Stack Overflow question.

### Dealing with routes

In order to have different locales in the URL you need to group the routes following these steps:

- Open **app/config/app.php** and add an array of the available languages.

```php
/**
 * List of alternative languages (not including the one specified as 'locale')
 */
'alt_langs' => array ('en', 'fr', 'es'),


/**
 *  Prefix of selected locale  - leave empty (set in runtime)
 */
'locale_prefix' => '',

``` 

- On **app/routes.php** define locale and group routes.

```php

/*
 *  Set up locale and locale_prefix if other language is selected
 */
if (in_array(Request::segment(1), Config::get('app.alt_langs'))) {

    App::setLocale(Request::segment(1));
    Config::set('app.locale_prefix', Request::segment(1));
}

// group routes according to locale
Route::group(array('prefix' => Config::get('app.locale_prefix')), function()
{
    Route::get(
        '/',
        function () {
            return "main page - ".App::getLocale();
        }
    );


    Route::get(
        '/laravel/',
        function () {
            return "contact page ".App::getLocale();
        }
    );

});

``` 

