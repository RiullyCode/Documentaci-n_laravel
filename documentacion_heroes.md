# Documentación Api Heroes

## Creación del proyecto:

Lo primero que deberemos hacer es crear un nuevo proyecto de Laravel el cual llamaremos `api_heroes`. Para ello, nos situaremos en la terminal en la ruta en la que queremos crear el archivo y procedemos a escribir lo siguiente:
```php
laravel new api_heroes
```
## Creación Base de datos:
Lo siguiente será en HeidiSQL, crear la base de datos la cual llamaremos por ejemplo `api_heroes`. 

## Modificación archivo .env:
Ahora lo que deberemos hacer será modificar el archivo .env, concretamente el siguiente apartado:
```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=api_heroes
DB_USERNAME=root
DB_PASSWORD=
```
en `DB_DATABASE` pondremos el nombre de la base de datos la cual hemos  al crearla, en este caso `api_heroes`.

## Creación de Controladores, Factories, Migraciones y Seeders:
Ahora deberemos crear para cada tabla de la base de datos sus correspondientes controladores, factories, migraciones y seeders. Para ello, escribiremos la siguiente sintaxis en la consola:

1. para Hero:
```php
php artisan make:model Hero -mfsc
```
2. para Ability:
```php
php artisan make:model Ability -mfsc
```
3. para Origin:
```php
php artisan make:model Origin -mfsc
```
4. para Identity:
```php
php artisan make:model Identity -mfsc
```

## Modificación de las migraciones:
Esto servirá para inyectar las tablas correspondientes en nuestra base de datos con sus respectivos campos:

1. En la migración de Heroes se crea una tabla con un `id` el cual se autoincrementa, un campo de tipo texto llamado `name`, un campo de tipo entero llamado `height`, otro de tipo float llamado `weight` y por ultimo el `timestamps` que viene de default.  
```php
    public function up(): void
    {
        Schema::create('heroes', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->integer('height');
            $table->float('weight');
            $table->timestamps();
        });
    }
```

2. En la migración de Abilities se crea una tabla con un `id` el cual se autoincrementa, un campo de tipo texto llamado `name`, un campo de tipo entero llamado `energy` y por ultimo el `timestamps` que viene de default.  
```php
    public function up(): void
    {
        Schema::create('abilities', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->integer('energy');
            $table->timestamps();
        });
    }
```

3. En la migración de Origins se crea una tabla con un `id` el cual se autoincrementa, un campo de tipo texto llamado `place`, un campo de tipo texto llamado `solar_system`, un campo de tipo texto llamado `dimension` y por ultimo el `timestamps` que viene de default.  
```php
    public function up(): void
    {
        Schema::create('origins', function (Blueprint $table) {
            $table->id();
            $table->string('place');
            $table->string('solar_system');
            $table->string('dimension');
            $table->timestamps();
        });
    }
```

4. En la migración de Identities se crea una tabla con un `id` el cual se autoincrementa, un campo de tipo texto llamado `name`, un campo de tipo texto llamado `surname`, un campo de tipo texto llamado `gender`, un campo de tipo texto llamado `city`, y por ultimo el `timestamps` que viene de default.  
```php
    public function up(): void
    {
        Schema::create('identities', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('surname');
            $table->string('gender');
            $table->string('city');
            $table->timestamps();
        });
    }
```

Para inyectar las migraciones en la base de datos deberemos escribir en la terminal:
```php
php artisan migrate
```

## Modificación de los factories:
Los factories nos sirven para insertar datos en nuestra base de datos, en este caso lo que necesitamos es insertar de forma aleatoria 10 heroes ficticios, para ello, deberemos escribir en `HeroFactory.php` la siguiente sintaxis dentro de `return[];`:
```php
        return [
            'name' => fake()->name(),
            'height' => fake()->numberBetween(120, 220),
            'weight' => fake()->randomFloat(1, 45, 200)
        ];
```

para terminar de inyectar los factories en la consola deberemos escribir:
```php
php artisan db:seed
```

## Modificación Controlador Hero + Configuración Api.php:

En el controlador Hero crearemos varias funciones las cuales nos devolveran x datos de la base de datos dependiendo de la respuesta que necesitemos, esta respuesta la "invocaremos" escribiendo las urls que especifiquemos en api.php.

Empezaremos creando una funcion la cual nos `devuelve todos los Heroes` en `JSON`:

```php
    public function getAllHeroes()
    {
        $heroes = Hero::all();
        return $heroes;
    }
```
Para llamar a esta funcion, en `Api.php` escribiremos:

```php
Route::get('/heroes', [HeroController::class, 'getAllHeroes']);
```
Después nos encontramos con una funcion la cual te muestra el heroe que tiene la id la cual llamamos:
```php
    public function getHeroById($id)
    {
        $hero = Hero::where('id', $id)->get();
        return $hero;
    }
```
Para llamar a esta funcion, en `Api.php` escribiremos:

```php
Route::get('/heroes/{id}', [HeroController::class, 'getHeroById']);
```

Por ultimo nos encontramos con una funcion la cual hace una request a la base de datos y devuelve nombre peso y altura:
```php
    public function newHero(Request $req)
    {
        return [
            "name" => $req->input('name'),
            "height" => $req->input('height'),
            "weight" => $req->input('weight'),
        ];
    }
```
Para llamar a esta funcion, en `Api.php` escribiremos:

```php
Route::post('/heroes', [HeroController::class, 'newHero']);
```


