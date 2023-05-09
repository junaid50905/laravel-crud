## laravel-crud

## crud for teachers

### first of all we need a migration file for teachers
```
php artisan make:migration create_teachers_table
```
now we have got a teachers migration file in database/migrations file

### write the columns on teachers migration file that we want

2023_05_08_022927_create_teachers_table.php
```
public function up(): void
    {
        Schema::create('teachers', function (Blueprint $table) {
            $table->id();
            $table->string('teacher_name');
            $table->double('teacher_salary');
            $table->timestamps();
        });
    }
```

### then migrate the teacher migration file
```
php artisan migrate
```
now we get a teachers table in our database

### we need a model that will communicate with our teachers table
```
php artisan make:model Teacher 
```
now we have got a teacher model file in app/models. <b>Note: model name will be singular and same of table name.</b>

### Now write the table name that will communicate with our TeacherController and fill the column name

models/Teacher.php
```
protected $table = 'teacher';
protected $fillable = ['teacher_name', 'teacher_salary'];
```

### now makes the view file for create. we can do it at first

views/backend/teacher/create.blade.php
```
@extends('backend.layouts.master')

@section('main_content')

    {{-- error message --}}
    @include('backend.layouts.includes.messages')
    {{-- end error message --}}

    <form action="" method="POST">
      @csrf
        <div class="form-group">
          <label for="" class="form-label">Teacher Name</label>
          <input type="text" name="teacher_name" id="" class="form-control" placeholder="" aria-describedby="helpId">
          @error('teacher_name')
            <p class="text-danger">{{ $message; }}</p>
          @enderror
        </div>
        

        <div class="form-group">
          <label for="" class="form-label">Teacher Salary</label>
          <input type="text" name="teacher_salary" id="" class="form-control" placeholder="" aria-describedby="helpId">
          @error('teacher_salary')
            <p class="text-danger">{{ $message; }}</p>
          @enderror
        </div>
                

        <button class="btn btn-primary mt-2" type="submit">Save</button>
        <button class="btn btn-danger mt-2" type="reset">Cancel</button>
    </form>
@endsection

```

### now makes a controler for teacher named TeacherControler
```
php artisan make:controller TeacherController
```
now we will get a file named TeacherController in app/Http/Controllers

### now makes route for create

routes/web.php
```
Route::get('/admin/teacher/create', [TeacherController::class, 'create'])->name('teacher.create');
```

### link the view files with route
``
<li class="nav-item">
        <a class="nav-link collapsed" data-bs-target="#teacher-nav" data-bs-toggle="collapse" href="#">
          <i class="bi bi-menu-button-wide"></i><span>Teachers</span><i class="bi bi-chevron-down ms-auto"></i>
        </a>
        <ul id="teacher-nav" class="nav-content collapse " data-bs-parent="#sidebar-nav">
          <li>
            <a href="{{ route('teacher.create') }}">
              <i class="bi bi-circle"></i><span>Create</span>
            </a>
          </li>
          <li>
            <a href="">
              <i class="bi bi-circle"></i><span>List</span>
            </a>
          </li>
        </ul>
      </li><!-- End Teachers Nav -->
  ```



### create

#### Make a view file
```
@extends('backend.layouts.master')

@section('main_content')
    {{-- error message --}}
    @include('backend.layouts.includes.messages')


    <form action="" method="POST">
        @csrf
        <div class="form-group">
          <label for="" class="form-label">Color Name</label>
          <input type="text" name="student_name" id="" class="form-control" placeholder="" aria-describedby="helpId">
          @error('student_name')
              <p class="text-danger">{{ $message; }}</p>
          @enderror
        </div>
        
        <button class="btn btn-primary mt-2" type="submit">Save</button>
        <button class="btn btn-danger mt-2" type="reset">Cancel</button>
    </form>
@endsection

```

#### make a migration file then write it's code then migrate the file
```
php artisan make:migraiton create_colors_table
```

database/migrations/2023_05_09_033605_create_colors_table.php
```
public function up(): void
    {
        Schema::create('colors', function (Blueprint $table) {
            $table->id();
            $table->string('color_name');
            $table->timestamps();
        });
    }
```

```
php artisan migrate    
```
now we get a table in database

#### Make a Model for Colors table then write it's code
```
php artisan make:model Color
```
app/Models/Color.php
```
class Color extends Model
{
    use HasFactory;
    protected $table = 'colors';
    protected $fillable = ['color_name'];
}
```

#### Make a controller and create a method to view the create for
```
php artisan make:controller ColorController
```

app/Http/Controllers/ColorController.php
```
class ColorController extends Controller
{
    public function create()
    {
        return view('backend.color.create');
    }
}
```
#### Now make a route to show create form
```
Route::get('/admin/color/create', [ColorController::class, 'create'])->name('color.create');
```
#### link the Route to view file
```
<li class="nav-item">
    <a class="nav-link collapsed" data-bs-target="#color-nav" data-bs-toggle="collapse" href="#">
      <i class="bi bi-menu-button-wide"></i><span>Color</span><i class="bi bi-chevron-down ms-auto"></i>
    </a>
    <ul id="color-nav" class="nav-content collapse " data-bs-parent="#sidebar-nav">
      <li>
        <a href="{{ route('color.create') }}">
          <i class="bi bi-circle"></i><span>Create</span>
        </a>
      </li>
      <li>
        <a href="">
          <i class="bi bi-circle"></i><span>List</span>
        </a>
      </li>
    </ul>
</li><!-- End Color Nav -->
```
































