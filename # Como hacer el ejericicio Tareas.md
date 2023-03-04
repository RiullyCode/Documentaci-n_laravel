# Como hacer el ejericicio Tareas

## 1. Crear un proyecto
Para crear el proyecto debemos entrar en la terminal y escribir el siguiente comando:
```
laravel new Tareas
```
## 2. Crear la base de datos y vincular al proyecto.
Crearemos la base de datos desde Heidi pero podemos hacerlo de muchas otras formas, lo primero sera iniciar sesion en el heidi desde el localhost
![menu heidi](https://cdn.discordapp.com/attachments/914613587715182622/1079383884380250162/image.png)


Despues le daremos click derecho al localhost, nuevo y base de datos
![crear bbdd heidi](https://cdn.discordapp.com/attachments/914613587715182622/1079383884380250162/image.png)


Por ultimo escribimos el nombre que queramos poner a la base de datos
![crear bbdd heidi](https://media.discordapp.net/attachments/914613587715182622/1081566325589164112/image.png)
le daremos a aceptar y la base de datos ya se habra creado correctamente:


Lo siguiente que tendremos que hacer será modificar un archivo de nuestro proyecto el cual se llama .env, concretamente un apartado en concreto el cual es el siguiente:
```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=tareas
DB_USERNAME=root
DB_PASSWORD=
```
Deberemos comprobar que esta todo configurado correctamente para que se conecte a la base de datos pero lo mas importante y seguramente lo unico que tendremos que modificar sera el `DB_DATABASE` al cual le tenemos que poner el nombre de la base de datos.

## Creación de la vista:
Lo siguiente sera crear una vista en la cual ver tanto el formulario como los resultados, lo que he hecho en mi caso es coger un formulario de bootstrap y modificarlo a mi gusto, el cual tiene el siguiente codigo:

### Formulario CSS + HTML
```css
    .form-border {
      border: 1px solid black;
      padding: 10px;
    }
```
```html
        <div div class="container align-items-start mt-5">
        <form class="form-border">
            <div class="form-group">
                <label for="tareas">Añadir Tareas</label>
                <input type="text" name="tarea" class="form-control" id="tareas" aria-describedby="emailHelp"
                    placeholder="Escribe aquí">
                <small id="emailHelp" class="form-text text-muted">Escribe en el cuadro superior la actividad que quieres añadir</small>
            </div>
            <button type="submit" class="btn btn-primary">Añadir</button>
        </form>
    </div>
```
Después he codigo de bootstrap una lista la cual he ajustado a mi gusto justo debajo del formulario, ha quedado de la siguiente manera:
### Lista HTML + CSS
```css
    .list-group {
      border: none;
    }
```
```html
    <ul class="list-group list-group-flush container mt-3 form-border justify-content-end">
        <li class="list-group-item">
            <form class="float-end">
                <button type="submit" class="btn btn-success">Realizado</button>
            </form>
        </li>
    </ul>
```
## Creación de Factorie, Migration, Seeder, Model and Controller
Para poder hacer todos los cambios en la base de datos, necesitamos crear todos los archivos que se mencionan con el titulo. Para crearlos necesitamos abrir la terminal y escribir el siguiente comando:
```
php artisan make:model Tarea -msfc
```
Esto creará todos los archivos en cuestión.
# Modificación migración 
Al primero al que tenemos que dirijirnos es en database, a migrations, y ahí entrar en la migracion la cual empieza por la fecha del dia en el que creaste la migracion y el nombre, es decir, algo de este estilo `2023_03_03_160204_create_tareas_table`, una vez dentro vendrá ya escrito una función llamada `up()`la cual servirá para añadir una tabla a nuestra base de datos. Vendrá lo siguiente:
```php
        Schema::create('tareas', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
```
nosotros como queremos añadir tareas crearemos un campo en la tabla el cual se llamará tarea, al ser de tipo string, nos quedaria de la siguiente forma:
```php
        Schema::create('tareas', function (Blueprint $table) {
            $table->id();
            $table->string('tarea');
            $table->timestamps();
        });
```
ahora en la consola escribiremos el siguiente comando para hacer la migracion:
```
php artisan migrate
```
## Modificación Controlador
En el controlador crearemos todas las funciones las cuales van a hacer todo lo que nosotros necesitemos.

Para listar todas las tareas la función que necesitamos crear será la siguiente:
```php
    public function listarTareas(){
        $tareas = Tarea::all();
        return view('tareas', ["misTareas" => $tareas]);
    }
```
 Su objetivo es recuperar todas las tareas registradas en la base de datos y mostrarlas en una vista llamada "tareas". Deveeulve a la vista un array asociativo que se llama `$misTareas` y contiene todas las tareas recuperadas de la base de datos. Nosotros al crear los factories y todo eso, por decirlo de algun modo, eso automaticamente enlaza el factories con el modelo y a su vez con el controlador, por lo que todos esos archivos estan enlazados de por si.

 en la función lo primero que nos encontramos es que `$tareas` es a igual coger todo del modelo Tarea (el cual se le llama arriba del todo con la url correspondiente). Esto se hace con eloquent y tiene varios metodos más como los siguientes:

1. `all()`: devuelve todas las filas de una tabla.
2. `find()`: devuelve una fila por su identificador.
3. `where()`: agrega una cláusula where a la consulta.
4. `orderBy()`: ordena los resultados por una columna específica.
5. `count()`: cuenta el número de filas en la tabla que coinciden con la consulta.
6. `save()`: guarda un modelo en la base de datos.
7. `delete()`: elimina un modelo de la base de datos.
8. `update()`: actualiza una o varias filas en la base de datos.

En la función listarTareas(), la línea $tareas = Tarea::all(); se utiliza para obtener todas las tareas almacenadas en la base de datos y se almacenan en la variable $tareas. Luego, la siguiente línea de código:

return view('tareas', ["misTareas" => $tareas]);

se utiliza para devolver una vista llamada tareas.blade.php y pasarle los datos de las tareas almacenadas en la variable $tareas. En particular, los datos se pasan como un arreglo asociativo con la clave "misTareas" y el valor de $tareas.

En la vista tareas.blade.php, se puede acceder a las tareas almacenadas en $tareas utilizando la variable $misTareas. Por ejemplo, se puede iterar a través de todas las tareas en $misTareas y mostrarlas en la página web.

En resumen, `$tareas = Tarea::all()` se utiliza para `obtener todas las tareas almacenadas en la base de datos y pasarlas a la vista tareas.blade.php como una variable llamada $misTareas.`

Después hay que definir la URL en web.php la cual sería de la siguiente manera:
```
Route::get('/tareas', [TareaController::class, 'listarTareas'])->name('listarTareas');
```
Esta ruta especifica que cuando un usuario solicite la URL /tareas en su navegador, Laravel debe ejecutar la acción listarTareas() en el controlador TareaController.

En términos generales, una ruta en Laravel es una forma de asociar una URL con una acción en un controlador. Esto se hace mediante el uso de métodos en la clase Route.

En la línea de código anterior, Route::get() es el método que se utiliza para definir una ruta que responde a solicitudes GET HTTP. Luego, se especifica la URL /tareas, que es la URL que activará la ruta.

El segundo argumento [TareaController::class, 'listarTareas'] especifica qué controlador y qué acción debe ejecutarse cuando se solicita la ruta. En este caso, se especifica que el controlador TareaController debe utilizarse y que la acción listarTareas() en ese controlador debe ejecutarse.

Finalmente, ->name('listarTareas') se utiliza para asignar un nombre a la ruta. Esto puede ser útil para hacer referencia a la ruta en otras partes del código, como en enlaces a otras páginas.

En resumen, una ruta en Laravel es una forma de asociar una URL con una acción en un controlador. En este caso, la ruta se define para responder a solicitudes GET HTTP en la URL /tareas y ejecuta la acción listarTareas() en el controlador TareaController.E

### En la vista
```html
   <ul class="list-group list-group-flush container mt-3 form-border justify-content-end">
        @foreach ($misTareas as $t)
        <li class="list-group-item">
            {{$t->tarea}}
            <form class="float-end">
                <button type="submit" class="btn btn-success">Realizado</button>
            </form>
        </li>
```
`<ul class="list-group list-group-flush container mt-3 form-border justify-content-end">`: Este código genera una lista sin estilos (list-group-flush) que se muestra en un contenedor. El estilo justify-content-end alinea los elementos a la derecha.

`@foreach ($misTareas as $t)`: Este código es una estructura de control que se utiliza para recorrer la variable $misTareas, que contiene un array de tareas recuperadas del controlador. En cada iteración, la variable $t contiene una tarea.

`<li class="list-group-item">`: Este código genera un elemento de la lista.

`{{$t->tarea}}`: Este código muestra el nombre de la tarea en el elemento de la lista. La sintaxis {{$t->tarea}} se utiliza para mostrar una variable en la plantilla Blade.

`<form class="float-end">`: Este código genera un formulario que se muestra a la derecha de la tarea. En este caso, el formulario no tiene acción, por lo que no se enviará a ningún lado.

`<button type="submit" class="btn btn-success">Realizado</button>`: Este código genera un botón que se muestra dentro del formulario. El botón se muestra con el estilo btn-success. En este caso, el botón no tiene una acción definida, por lo que no hará nada si se presiona.

## Funcion añadirTareas

```php
    public function añadirTareas(Request $req){
        $tarea = new Tarea;
        $tarea->tarea = $req->input('tarea');
        $tarea->save();
    
        $tareas = Tarea::all();
        return view('tareas', ["misTareas" => $tareas]);
    }
```
La función acepta un objeto Request como parámetro, el cual contiene los datos enviados por el usuario en la solicitud HTTP. A continuación, se crea una nueva instancia del modelo Tarea y se asigna el valor del campo 'tarea' del objeto Request a la propiedad 'tarea' del modelo. Luego, se guarda el modelo en la base de datos utilizando el método save().

Después de guardar la nueva tarea, se obtienen todas las tareas de la base de datos mediante el modelo Tarea y se almacenan en una variable $tareas. Finalmente, se devuelve una vista llamada 'tareas', pasando las tareas recuperadas como un arreglo asociativo en la variable $misTareas.

En resumen, esta función añade una nueva tarea a una lista de tareas en la base de datos y luego muestra todas las tareas, incluyendo la nueva, en una vista llamada 'tareas'.

La ruta que se define es la siguiente:
```php
Route::post('/tareas', [TareaController::class, 'añadirTareas'])->name('añadirTareas');
```
1. `Route::post()` define una ruta que maneja solicitudes HTTP POST.
2. `/tareas` es la URL a la que se responde.
3. `[TareaController::class, 'añadirTareas']` indica que el método añadirTareas() del controlador TareaController manejará esta ruta.
4. `->name('añadirTareas')` da un nombre a la ruta para poder referirse a ella fácilmente en otras partes de la aplicación.

Por lo tanto, cuando se recibe una solicitud POST a la URL /tareas, Laravel llamará a la funcion añadirTareas() del controlador TareaController y procesará los datos que se enviaron en la solicitud. El método añadirTareas() es responsable de agregar una nueva tarea a una lista de tareas y luego mostrar todas las tareas en una vista. La ruta se puede referir fácilmente en otras partes de la aplicación utilizando el nombre "añadirTareas".

La vista sera la siguiente:
```html
    <div div class="container align-items-start mt-5">
        <form class="form-border" action="{{ route('añadirTareas') }}" method="POST">
            @csrf
            <div class="form-group">
                <label for="tareas">Añadir Tareas</label>
                <input type="text" name="tarea" class="form-control" id="tareas" aria-describedby="emailHelp"
                    placeholder="Escribe aquí">
                <small id="emailHelp" class="form-text text-muted">Escribe en el cuadro superior la actividad que quieres añadir</small>
            </div>
            <button type="submit" class="btn btn-primary">Añadir</button>
        </form>
    </div>
```
1. `div class="container align-items-start mt-5"` es un contenedor que establece la alineación vertical y el margen superior del formulario.

2. `form class="form-border" action="{{ route('añadirTareas') }}" method="POST"` es el formulario en sí. La acción del formulario está establecida en la ruta que maneja las solicitudes POST para añadir tareas y el método HTTP utilizado es POST.

3. `@csrf` es una directiva de Laravel que genera un campo de token CSRF para proteger la solicitud contra ataques CSRF.

4. `div class="form-group"` es un contenedor para el campo de entrada.

5. `label for="tareas"` es una etiqueta para el campo de entrada.

6. `input type="text" name="tarea" class="form-control" id="tareas" aria-describedby="emailHelp" placeholder="Escribe aquí"` es el campo de entrada donde el usuario puede escribir el nombre de la tarea. El nombre de entrada es "tarea".
small id="emailHelp" class="form-text text-muted" es un texto de ayuda que se muestra debajo del campo de entrada.

7. `button type="submit" class="btn btn-primary"` es un botón de envío que el usuario puede hacer clic para enviar el formulario y añadir una nueva tarea a la lista.

En resumen, esta vista es un formulario HTML que permite a los usuarios añadir nuevas tareas a una lista de tareas mediante la entrada de texto en un campo de entrada y haciendo clic en un botón de envío. Cuando se envía el formulario, la solicitud es manejada por la ruta definida en la acción del formulario, lo que permite añadir la tarea a la lista de tareas.

## Función Eliminar Tareas.
```php
    public function eliminarTarea($id) {
        $tarea = Tarea::find($id);
        $tarea->delete();   
        return redirect()->route('listarTareas');
    }
```
La función toma un parámetro $id, que se refiere al identificador único de la tarea que se va a eliminar de la base de datos.

Dentro de la función, se utiliza el método find() del modelo Tarea para buscar la tarea con el identificador $id en la base de datos y se asigna el resultado a la variable $tarea. Si la tarea es encontrada, se llama al método delete() en la instancia de $tarea para eliminar la tarea de la base de datos.

Finalmente, la función redirige al usuario a la ruta con nombre listarTareas utilizando el método redirect()->route(). Este método construye una URL a partir del nombre de la ruta y redirige al usuario a esa URL. En este caso, la ruta listarTareas probablemente mostrará la lista actualizada de tareas sin la tarea que acaba de ser eliminada.

En resumen, la función eliminarTarea es un método de controlador que maneja la eliminación de una tarea específica de una lista de tareas en Laravel.

la url del web.php será la siguiente:
```php
Route::delete('/tareas/{id}', [TareaController::class, 'eliminarTarea'])->name('eliminarTarea');
```
1. ```Route::delete``` especifica que la ruta maneja solicitudes DELETE.

2. ```'/tareas/{id}'``` es la URL de la ruta. El segmento tareas es el nombre de la entidad que estamos manipulando, y {id} es un parámetro dinámico que se refiere al identificador único de la tarea que se va a eliminar. El parámetro {id} se pasará como un parámetro a la función de controlador que maneja esta ruta.

3. `[TareaController::class, 'eliminarTarea']` especifica el controlador y el método de controlador que manejan la ruta. En este caso, el método eliminarTarea del controlador TareaController se encargará de la solicitud DELETE para eliminar una tarea específica.

4. `->name('eliminarTarea')` especifica el nombre de la ruta. Este nombre se utiliza para generar URLs en otras partes de la aplicación. En este caso, el nombre de la ruta es 'eliminarTarea'.

En resumen, esta ruta maneja solicitudes DELETE para eliminar una tarea específica de una lista de tareas, utilizando el identificador único de la tarea como un parámetro dinámico en la URL. La ruta está vinculada a una función de controlador que se encarga de la eliminación de la tarea y se nombra como 'eliminarTarea'.

```html
    <ul class="list-group list-group-flush container mt-3 form-border justify-content-end">
        @foreach ($misTareas as $t)
        <li class="list-group-item">
            {{$t->tarea}}
            <form action="{{ route('eliminarTarea', $t->id) }}" method="POST" class="float-end">
                @csrf
                @method('DELETE')
                <button type="submit" class="btn btn-success">Realizado</button>
            </form>
        </li>
        @endforeach
```
El formulario  incluye los atributos @csrf y @method('DELETE'). @csrf es una directiva de Blade que agrega un campo de token CSRF para proteger contra ataques CSRF. @method('DELETE') es una directiva que especifica que la solicitud utiliza el método DELETE en lugar del método POST predeterminado.

Muestra un botón "Realizado" asociado para cada tarea, que permite eliminar la tarea de la lista cuando se ha completado. 