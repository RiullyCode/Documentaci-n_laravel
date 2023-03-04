# Documentación Laravel

## Instalación
1. Lo primero que tendremos es instalar Laravel, para ello lo primero que deberemos hacer es instalar XAMPP => https://www.apachefriends.org/es/index.html

2. Lo siguiente que deberemos hacer es instalar composer con windows, para ello debemos instalar Composer con el instalador para Windows. DescargaráComposer y creará una variable de entorno PATH para poder invocar el comando composer desde cualquier directorio =>  https://getcomposer.org/

3. Después en el powershell se escribirá la siguiente sintaxis
```
composer global require laravel/installer
```
4. Crearemos un proyecto de Laravel con:
```
laravel new nombre_proyecto
```
5. Nos meteremos dentro de la carpeta del proyecto y escribiremos:
```
cd nombre_proyecto
```
6. Ejecutaremos el servidor con:
```
php artisan serve
```

## Dinámica básica de Laravel
Todo en la web consiste en request y response, solicitud y respuesta. Nosotros escribimos una URL y cuando le damos enter nosotros hacemos una solicitud desde nuestro navegador a un servidor para que este nos de una respuesta, como puede ser archivos de html, css, javascript. 

En laravel las rutas se escriben en un archivo llamado ***web.php*** el cual se encuentra en carpeta ***routes***.

Una ruta suele conectar con un controlador, un controlador suele ser una consulta a una tabla de una base de datos.


Es decir, el orden será el siguiente:

1. primero se escriba una ruta la cual se dirige a un controlador.

2. el controlador consulta con una tabla de la base de datos.

3. la base de datos le devuelve la respuesta de la consulta al controlador.

4. El controlador nos retorna una vista


Esta dinámica se puede hacer más compacta de tal forma que solo veamos, ruta -> vista. Si nosotros entramos directamente en laravel en el archivo que comentabamos antes llamado ***web.php*** nos encontramos con que viene de serie la siguiente ruta:
```php
Route::get('/', function () {
    return view('welcome');
});
```
Esto se podria traducir de la siguiente manera: la palabra ***Route*** indica que esto es una ruta, que ejecuta una respuesta de tipo ***get***, despues si analizamos lo que va entre parentesis el ***'/'*** significa que es el localhost y luego nos devuelve una vista ***view*** que se llama ***welcome***.

Como hemos visto antes hay una vista llamada ***welcome*** la cual se encuentra en carpeta ***resources*** después ***views***, el archivo se llama ``welcome.blade.php`` y ahí es donde crearemos todas las vistas. Si nos fijamos el nombre es el mismo pero en la ruta no hemos muesto .blade.php ya para que funcione hay que poner exclusivamente el nombre.

Teniendo en cuenta la ruta que teniamos en el controlador de antes:
```php
Route::get('/', function () {
    return view('welcome');
});
```

Si nosotros quisieramos crear otra vista tendriamos que modificar lo que en el ejemplo anterior seria ``'/'`` ya que eso significa que esa es la ruta por defecto. Es decir, si nosotros quisieramos crear otra vista, tendriamos que poner por ejemplo ``/todos``, al poner en el navegador al final de la url ``/todos`` se lanzará otra vista diferente. Vamos a suponer que creamos otra vista diferente que haga referencia a una vista llamada todos, lo primero en vistas tendriamos que crear una vista ``todos.blade.php`` y dentro de esta todo lo que queramos que aparezca en esa vista, pues en el ``web.php`` tendriamos que hacer lo siguiente:
```php
Route::get('/todos', function () {
    return view('todos');
});
```
Esto basicamente hará que al lanzar la aplicacion nos aparezca la vista que hemos creado en todos.

Bien, ahora supongamos que yo creo un menu en ``/todos`` y yo quiero que aparezca en otra vista, pues no es necesario copiar el codigo. Simplemente tendriamos que poner lo siguiente:

1. Al final del body del archivo el cual queremos copiar, justo antes de cerrar la etiqueta, escribiremos ``@yield('content')``, esto por decirlo de algun modo definirá una etiqueta la cual luego podremos nombrar en otro archivo .blade de una determinada manera lo cual hará que todo el codigo automaticamente se importe por asi decirlo.

2. Lo segundo que habria que hacer es al principio del archivo .blade en el cual queremos importar el codigo, escribiremos ``@extends('nombre_plantilla_padre')`` es decir, si nosotros el ``@yield`` lo hemos escrito en la platilla ``app.blade.php`` pues en la plantilla hija escribiremos ``@extends('app')``.

3. Por ultimo escriberemos debajo del extends ``@section('nombre_yield')``, es decir, escribiremos la palabra ``@section`` para hacer referencia al yield que hemos puesto anteriormente, es decir, como nosotros hemos llamado anteriormente al ``@yield('content')`` ahora tendremos que escribir ``@section('content')``. Despues de escribirlo, debemos cerrarlo con un ``@endsection``.

4. Hay que tener en cuenta que nosotros podemos incluir contenido a la copia escribiendo más codigo entre ``@section('content')`` y ``@endsection``.

Bien ahora lo que haremos será añadir un formulario copiado de bootstrap y ponerlo en este .blade, dentro de las etiquetas ``@section('content')`` y ``@endsection``, por ejemplo, algo así.
```html
    <div class="container w-25 border p-4 mt-4">
        <form>
            <div class="form-group">
                <label for="tarea">Título de la tarea</label>
                <input type="email" class="form-control" id="tarea" aria-describedby="emailHelp" placeholder="Enter email">
            </div>
            <button type="submit" class="btn btn-primary">Crear nueva tarea</button>
        </form>
    </div>
```
Ya que el objetivo que nos vamos a poner es hacer un formulario en el cual podamos añadir tareas a una base de datos.

## Creación de nuestro primer modelo.

### Migraciones

Para esto lo primero que hay que entender es son las migraciones, en laravel no vamos a tocar tal cual la base de datos directamente con consultas, sentencias o querys, si no que vamos a usar las migraciones, estas hacen referencia a un control de versiones para nuestra base de datos, es decir, git por ejemplo es un control de versiones para codigo. Nosotros podemos volver a ver el codigo en un punto anterior, pues esto es igual pero con las bases de datos. Nosotros al utilizar migraciones estamos versionando o guardando snap shots de nuestra base de datos y la estructura de las tablas.

### ORM (Object Relational Mapper)

Un ORM es una object relational Mapper o un mapeador relacional de objetos, como se explico anteriormente, no vamos a usar consultas o querys para modificar la base de datos, laravel tiene una capa de abstracción, la cual se llama ``elocuent``, esto nos va a permitir tener una relacion entre nuestras tablas con nuestros modelos, esto quiere decir que por ejemplo nosotros vamos a crear un modelo el cual llamaremos ``Todo`` el cual tenga un titulo, entonces ese parametro se va a transformar en una tabla que tenga una columna que se llame titulo que sea de tipo string, de esa forma es que vamos a hacer la relacion entre nuestros modelos y nuestras tablas en nuestra base de datos sin la necesidad de tocar una consulta con laravel.

### Aplicación:
Para aplicar todo lo explicado anteriormente lo primero que tenemos que hacer es crear un modelo, esto se hace escribiendo lo siguiente en la terminal, se hace con artisan por lo que la sintaxis será:
```
php artisan make:model Todo -m 
```
La sintaxis basicamente lo que es crear con artisan un modelo llamado Todo, el ``-m`` del final sirve para crear la respectiva migración.

Con esto hemos creado 2 archivos, el del modelo y el de la migración.

Para ver el del modelo tendremos que ir a `app` -> `models` -> `Todo.php`.

Para ver el de la migración debemos ir a `database` -> `migrations` -> `El archivo que corresponda`. Se sabe cual es el archivo ya que primero pone la fecha y luego el nombre del modelo, este caso el nombre es: `2023_02_25_222255_create_todos_table.php`

Como podemos ver en la migración viene una funcion llamada `up` y otra llamada `down`. El `up` es para ejecutar la actividad principal de la migracion y el `down` es si quieres eliminar lo que acabas de crear.

Si por ejemplo nosotros vemos uno de las migraciones que vienen de default (por ejemplo la primera), la cual en el up contiene esto.

```php
    public function up(): void
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }
```
Basicamente lo que está haciendo es:
Crea una nueva tabla llamada usuarios la cual contenga un id, una string de tipo nombre... etc. 

Ahora si nos vamos a la migración que hemos creado y por defecto viene lo siguiente:

```php
    public function up(): void
    {
        Schema::create('todos', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
```

y nosotros ahora vamos a definir para añadir el titulo de las tareas un nuevo campo de tipo string llamado `title` tal que así:

```php
        public function up(): void
    {
        Schema::create('todos', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->timestamps();
        });
    }
```

ahora lo que haremos es como correr esta migracion en nuestra base de datos. Lo primero es crear la base de datos. Para esto se puede hacer de diferente formas, yo usare la que utilizamos en clase, es decir, con heidiSQL:

1. Instalamos HeidiSQL, la página oficial es https://www.heidisql.com/download.php

2. Una vez abierto, nos aparecerá este menú, ponemos todos lo necesario y entramos dandole a `abrir`.

![Menu Principal Heidi](https://media.discordapp.net/attachments/914613587715182622/1079383884380250162/image.png)

3. Nos aparecerá el siguiente menú, debajo de localhost nos apareceran todas las bases de datos que tenemos, lo que tenemos que hacer es crear una nueva.

![Pantalla Heidi](https://media.discordapp.net/attachments/914613587715182622/1079384870343688192/image.png)

4. Para crear una nueva base de datos le daremos `click derecho encima de localhost`, luego a `crear nuevo` y luego a `base de datos`

![Crear base de datos](https://cdn.discordapp.com/attachments/914613587715182622/1079385334250471445/image.png)

5. Escribiremos en nombre, el nombre que queremos, en este caso nosotros escribiremos `todos` y le daremos a `aceptar`.

![Crear base de datos](https://media.discordapp.net/attachments/914613587715182622/1079386894783234138/image.png)

6. La base de datos ya nos deberia de aparecer en la parte izquierda debajo de localhost junto al resto de las bases de datos

![Crear base de datos](https://media.discordapp.net/attachments/914613587715182622/1079387708536934471/image.png)

### Conexion a la base de datos:

Para ello tendremos que ir en laravel al archivo llamado `.env`. Dentro de este archivo deberemos configurar exactamente el siguiente apartado:

```js
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=todos
DB_USERNAME=root
DB_PASSWORD=
```
En `DB_DATABASE` deberemos poner el nombre de la base de datos, en este caso como ya lo he modificado y puesto `todos` el cual es el nombre que usamos para crear nuestra base de datos.

Ahora en la consola deberemos escribir una sintaxis en concreto la cual es:
```
php artisan migrate
```
Esto lo que hará es ejecutar todas las migraciones que tenemos en laravel, es decir, que esa modificacion que hemos hecho antes de crear la string `title` se insertara en la base de datos que hemos especificado en `.env`.

Si nosotros antes de ejecutar el comando escribimos: 
```
php artisan migrate:status
```
Nos saldrá un mensaje en rojo el cual nos dirá `Migration table not found`. Después de ejecutar `php artisan migrate` no pasara esto.

Si nosotros vamos ahora a heidi a revisar la base de datos, nos encontraremos con que se han aplicado las migraciones correctamente:

![Base de datos con migraciones](https://media.discordapp.net/attachments/914613587715182622/1079392590442545212/image.png)


En caso de que nosotros nos equivoquemos al hacer la migracion, escribremos lo siguiente en la consola:

```
php artisan migrate:rollback
```
esto va a deshacer el ultimo realizado.

## Unir la vista con el modelo

Lo siguiente que tendremos que hacer para unir la vista con el modelo es a traves de un controlador, esto se hará escribiendo lo siguiente en la consola:

```
php artisan make:controller TodosController
```
Este controlador que acabamos de crear lo encontraremos en `app` -> `Http` -> `Controllers`->`TodosController.php`.

Esto nos servira para determinar la logica de nuestra aplicación.

Ahora vamos a hacer nuestor primer `insert`, laravel hace una convencion de nombres para poder nombrar algunos de nuestros metodos.

1. `index`: para mostrar todos los todos.
2. `store`: para guardar un todo
3. `update`: para actualizar un todo.
4. `destroy`: para eliminar un todo.
5. `edit`: para mostrar el formulario de edicion.

Esto es por convencion por lo que no es obligatorio usarlo pero si aconsejable para poder entenderlo mejor.

Para empezar a realizar nuestro primer insert, esto lo que hara es almacenar en nuevo todo en nuestros todos. Va a recibir un parametro el cual llamaremos `$request` para ello escribiremos:
```php
public function store(Request $request){
}
```
Nosotros tendriamos que validar los datos por post, pero con laravel lo haremos mas sencillo:
```php
public function store(Request $request){
    //esto valida los datos sin necesidad de ifs, elses... etc.
    $request->validate([
        'title' => 'required'|min:3
    ]);
}
```
Basicamente lo que le hemos dicho es que valide title de tal forma que sea obligatorio rellenarlo y que como minimo tenga una longitud de 3 caracteres de longitud.

Ahora que tenemos la validacion ya podemos crear un nuevo `todo`.
```php
class TodosController extends Controller
{
    public function store(Request $request){
        //esto valida los datos sin necesidad de ifs, elses... etc.
        $request->validate([
            'title' => 'required|min:3'
        ]);
        $todo = new Todo;
        $todo->title = $request->title;
        $todo->save();

        return redirect()->route('todos')->with('success', 'Tarea creada correctamente');
    }
}
```
la sintaxis seria la siguiente y con todo esto ya nos dejaria guardar un nuevo elemento. 

Para ver esto en accion ahora nos vemos a `web.php` y escribiremos lo siguiente:

```php
Route::post('/todos', [TodosController::class, 'store']);
```

En el .blade del formularioen el form deberemos poner:

```php
form action="{{ route('todos') }}" method="POST"
```
y debajo de esta linea hay que poner por seguridad y para que permita la inyeccion:
```
@csrf
``` 








