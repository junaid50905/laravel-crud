
### create

#### Make a view file
```
@extends('backend.layouts.master')

@section('main_content')
    {{-- error message --}}
    @include('backend.layouts.includes.messages')


    <form action="" method="POST">
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
#### make a list where i want to show our data and link the

### Store

#### Make a request file for color
```
php artisan make:request ColorRequest
```
authorize will be true in autorize method and writ rules in rules method

app/Http/Requests/ColorRequest.php
```
public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        return [
            'color_name' => ['required', 'max:255'],
        ];
    }
```

#### create a method named store in ColorController
```
public function store()
    {
        dd('check');
    }
```
here, we just checked the method is working or not. 

#### make a route for store method then connect the route with form

routes/web.php
```
Route::get('/admin/color/store', [ColorController::class, 'store'])->name('color.store');
```
connect the store route link with form action attribute and put the @csrf token after the form element

backend/color/create.php
```
<form action="{{ route('color.store') }}" method="POST">
    @csrf
    <div class="form-group">
      <label for="" class="form-label">Color Name</label>
      <input type="text" name="color_name" id="" class="form-control" placeholder="" aria-describedby="helpId">
      @error('color_name')
          <p class="text-danger">{{ $message; }}</p>
      @enderror
    </div>

    <button class="btn btn-primary mt-2" type="submit">Save</button>
    <button class="btn btn-danger mt-2" type="reset">Cancel</button>
</form>
```
#### write the store code
```
try {
            $color = $request->all();
            Color::create($color);
            return redirect()->route('color.list')->withSuccess_add('Successfully added new product');
        } catch (Exception $e) {
            return redirect()->back()->withError_add($e->getMessage());
        }
 ```
 store is completed.
 
 
 ### list
 
 #### write the code of list method into controller
 ```
 public function list()
    {
        $colors = Color::all();
        return view('backend.color.list', compact('colors'));
    }
 ```
 
 #### view the list
 ```
 @extends('backend.layouts.master')

@section('main_content')
    {{-- success message --}}
    @include('backend.layouts.includes.messages')

    <a href="{{ route('color.create') }}" class="btn btn-sm btn-outline-primary my-2">Add color</a>
    <div class="table-responsive">
        <table class="table">
            <thead>
                <tr>
                    <th class="col-md-1">SN</th>
                    <th class="col-md-2">Color Name</th>
                    <th class="col-md-3">Action</th>
                </tr>
            </thead>
            <tbody>
                @php
                    $sn = 1
                @endphp
                @foreach ($colors as $color)
                    <tr>
                        <td>{{ $sn++ }}</td>
                        <td>{{ $color->color_name }}</td>
                        <td>
                            <a class="btn btn-sm btn-warning" href="">Edit</a>
                            <button class="btn btn-sm btn-danger">Delete</button>
                        </td>
                    </tr>
                @endforeach                    
                    
            </tbody>
        </table>
    </div>
    
@endsection
 ```
 




























































