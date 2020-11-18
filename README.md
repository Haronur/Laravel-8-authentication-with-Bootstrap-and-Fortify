<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains over 1500 video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the Laravel [Patreon page](https://patreon.com/taylorotwell).

### Premium Partners

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Cubet Techno Labs](https://cubettech.com)**
- **[Cyber-Duck](https://cyber-duck.co.uk)**
- **[Many](https://www.many.co.uk)**
- **[Webdock, Fast VPS Hosting](https://www.webdock.io/en)**
- **[DevSquad](https://devsquad.com)**
- **[OP.GG](https://op.gg)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## -- Fortify-Integration --

#### Step 1: Installation: 
- To get started, install Fortify using Composer:

```
composer require laravel/fortify
```

#### Step 2-0: Next, publish Fortify's resources:

```php artisan vendor:publish --provider="Laravel\Fortify\FortifyServiceProvider"```
- This command will publish Fortify's actions to your `app/Actions` directory. This directory will be created if it does not exist. In addition, Fortify's configuration file and migrations will be published.
#### Step 2-1:The Fortify Service Provider

- The `vendor:publish` command discussed above will also publish the `app/Providers/FortifyServiceProvider` file. You should ensure this file is registered within the providers array of your app configuration file.

- This service provider registers the actions that Fortify published, instructing Fortify to use them when their respective tasks are executed by Fortify

#### Step 3: Create Database at phpMyAdmin named `laravel-8-authentication-with-bootstrap-and-fortify` and setup .env file in your root directory 

- Database
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel-8-authentication-with-bootstrap-and-fortify
DB_USERNAME=root
DB_PASSWORD=
```

#### Step 4: Customize at AppServiceProvider.php 
```
Customize at AppServiceProvider.php in project\app\Providers
```

#### Step 5: Migrate Database
- Now you need to run default migration of laravel by the following command:
```
php artisan migrate
```

## Follow This Instruction:  https://github.com/laravel/fortify


## -- Fortify-Integration 02 - Fortify views for Bootstrap 4
#### Install bootstrap

- We will be also be installing jquery and popper.js because bootstrap requires these packages
```
npm i
npm install --save bootstrap jquery popper.js cross-env
```
- Import packages in the resources/js/bootstrap.js file
```
try {
    window.Popper = require('popper.js').default;
    window.$ = window.jQuery = require('jquery');

    require('bootstrap');
} catch (e) {
}
```
- Delete resources/css folder and create app.scss file in resources/sass
```
rm -rf resources/css
mkdir resources/sass
touch resources/sass/app.scss
```
- Import packages in the `resources/sass/app.scss` file
```
// bootstrap
@import "~bootstrap/scss/bootstrap";
```
#### Next update webpack.mix.js
```
mix.js('resources/js/app.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css');
```
Compile assets

`npm run dev`

#### Add Auth Views

- In your `resources/views` folder create two folders auth and layouts

`mkdir resources/views/layouts resources/views/auth`

- Next create the following views
```
touch resources/views/layouts/app.blade.php
touch resources/views/auth/login.blade.php
touch resources/views/auth/register.blade.php
touch resources/views/auth/forgot-password.blade.php
touch resources/views/auth/reset-password.blade.php
touch resources/views/auth/verify-email.blade.php
touch resources/views/home.blade.php
```
#### Update Fortify Provider

- Next we will need to update boot method in `app/Providers/FortifyServiceProvider`. This is to tell fortify how to authenticate the user and where the views we created early are located.
```
public function boot()
{
    Fortify::loginView(function () {
        return view('auth.login');
    });

    Fortify::authenticateUsing(function (Request $request) {
        $user = User::where('email', $request->email)->first();

        if ($user &&
            Hash::check($request->password, $user->password)) {
            return $user;
        }
    });

    Fortify::registerView(function () {
        return view('auth.register');
    });

    Fortify::requestPasswordResetLinkView(function () {
        return view('auth.forgot-password');
    });

    Fortify::resetPasswordView(function ($request) {
        return view('auth.reset-password', ['request' => $request]);
    });

    Fortify::verifyEmailView(function () {
        return view('auth.verify-email');
    });

    // ...
}
```
- Next register FortifyServiceProvider by adding it to the providers array in `config\app.php` `App\Providers\FortifyServiceProvider::class,`.

- In your app/Models/User.php file ensure the class implements MustVerifyEmail
```
<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class User extends Authenticatable implements MustVerifyEmail
{
    use HasFactory, Notifiable;

    // ...
}
```
- We also need to tell fortify that we want to enable email verfication. In the `app/fortify.php` file uncomment the line that says `Features::emailVerification()`.

- If you want to test email verification you will need to update your email variables in `.env`. You can use a free mail server mailtrap.io
```
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=info@lara8auth.com
MAIL_FROM_NAME="${APP_NAME}"
```
- Finally we will add the home route to the routes/web.php file
```
Route::middleware(['auth', 'verified'])->group(function () {
    Route::view('home', 'home')->name('home');
});
```
- We have completed our setup. You should have a working authentication system using laravel fortify and bootstrap.

## Follow This Instruction: https://dev.to/jasminetracey/laravel-8-with-bootstrap-livewire-and-fortify-5d33
