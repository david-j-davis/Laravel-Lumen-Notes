# Laravel Lumen Notes
---
*The following tutorial is loosly adopted from [http://wern-ancheta.com/blog/2015/05/09/getting-started-with-lumen/](http://wern-ancheta.com/blog/2015/05/09/getting-started-with-lumen/)*

### Installing & Serving your Lumen App
1. Download Lumen using composer ```composer global require "laravel/lumen-installer```
2. Place ```~/.composer/vendor/bin``` in your Bash Profile like: ```export PATH="~/.composer/vendor/bin:$PATH"```
3. If the above now works navigate to a directory and do an installation of lumen with ```lumen new name_your_project```
4. Serve up your application in that directory with ```php -S localhost:8000 -t public```

###Configuring your Lumen App
1. Configure your app settings in ```bootstrap/app.php```
2. Enable sessions by removing the comment in the 'Register Middleware' Section, and binding the session first like:

```
$app->bind(Illuminate\Session\SessionManager::class, function ($app) {
    return new Illuminate\Session\SessionManager($app);
});

$app->middleware([
    Illuminate\Session\Middleware\StartSession::class,
]);
```
3. If you need to use Eloquent or Facades uncomment the following:
```
$app->withFacades();
$app->withEloquent();
```

###Configuring the Environment File ```.env```
```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomKey_32_character_key!
APP_TIMEZONE=UTC

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lumen_boilerplate
DB_USERNAME=root
DB_PASSWORD=secret

CACHE_DRIVER=array
SESSION_DRIVER=cookie
QUEUE_DRIVER=array
```

###App structure (As of 1/27/17):
```
app
    - Console/
    - Events/
    - Exceptions/
    - Http/
    - Jobs/
    - Listeners/
    - Providers/
    - User.php
bootstrap
    - app.php
database
    - factories/
    - migrations/
    - seeds/
public
resources
    - views/
routes
    - web.php
storage
    - app/
    - framework/
    - logs/
tests
vendor
- artisan
- composer.json
- phpunit.xml
- readme.md
```
* ```app``` directory is where you controllers, middlewares, etc. are stored.
* ```bootstrap``` directory is where app.php lives and is where you configure functionality with Lumen
* ```database``` directory is where the database migrations and seeders are stored.
* ```public``` directory is where your public assets are stored
* ```resources``` directory is where you store the views, blade templates and such
* ```routes``` directory is where routes for the app are declared
* ```storage``` directory is where logs sessions and cache files are stored
* ```tests``` directory is where you put your test files
* ```vendor``` directory is where compuser dependencies for the app are stored
* ```artisan``` file is used for command line tasks for your project


###Routing
* Routing is declared in the ```routes``` folder and you can define a route directly in the web.php file like:
```
$app->get('/', functionn(){
    return 'Hello World!';
});
```
* Or you can define a controller to handle the route like: ```$app->get('/', array( 'as' => 'index', 'uses' => 'HomeController@index' ));```
* Make sure you create a file called HomeController.php or whatever you declare before the @ symbol. Place this file in ```app/Http/Controllers/```
* As an example, here's my HomeController connecting to the database and returning users data:
```
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\DB;
use Laravel\Lumen\Routing\Controller as BaseController;

class HomeController extends BaseController
{
    public function index() {

        $users = DB::table('users')->get();
        //
        // echo '<pre>';
        // print_r($users);
        // echo '</pre>';

        return view('index', compact('users')); // use '' instead of the $
    }

}
```
* Notice the classes declared at the top of the controller class, these classes are located in the di
