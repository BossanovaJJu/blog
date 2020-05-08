
---
title: "Using React in Laravel-라라벨에서 리액트를 활용해 Task App 만들어보기"
date: 2020-04-21T17:40:14+09:00
---


# Using React in a Laravel application
[![HitCount](http://hits.dwyl.com/{username}/{project}.svg)](http://hits.dwyl.com/{username}/{project})


### 참조 내용

[Using React in a Laravel application](https://blog.pusher.com/react-laravel-application) 원문 컨텐츠를 읽어보고 예제를 따라해보면서 관련 내용과 제 생각과 피드백을 적어보았습니다.  많이 부족하지만 재미있게 봐주세요!

> 여기의 글들은 제가 느끼고 생각하는 것들에 대해  이야기하는 글들이니 말투가 친절하지 않더라도 이해해주세요.^^




시작하기에 앞서..
제가 생각하는 라라벨은 백엔드와 관련되어 DB의 데이터를 더 쉽게 컨트롤 할 수 있고 라라벨에서 지원하는 각종 기능과 함수들로 더 빠르게 어플리케이션을 만들 수 있게 도와주는 프레임워크 같습니다.  모던 PHP로 구성되어 있고 지금 당장 PHP의 깊은 지식이 있지 않은 저 같은 사람들도 간단한 어플리케이션을 만들 수 있게 도와줍니다.

>이런 걸 양산형 개발자라고 하는것인가;; 

사실 프론트엔드단의 특정한 라이브러리나 프레임워크를 사용하지 않아도 왠만한 어플리케이션은 라라벨만을 가지고 구현 가능하지 않을까 생각이 듭니다.  기본적으로 라라벨에 프론트엔드 라이브러리인 Vue.js가 내재되어 있는거 보면 '백엔드 라라벨 + 프론트엔드 뷰,리액트 = 멋지고 효율적인 어플리케이션' 라는 공식이 성립되지 않을까라는 생각을 해보았습니다. 

>아직까지 라라벨로든 리액트로든 제대로 된 어플리케이션을 만들어 본 경험이 없으니 알 수가 없지;; 어여 만들어보자꾸나 ㅠㅠ

라라벨과 관련되어 기초적인 내용을 익히다가 프론트엔드 단과 어떻게 조인을 하는지 궁금해졌습니다.  리액트를 어떻게 세팅하고, DB데이터와 리액트가 어떻게 연동되는지 알고 싶어서 관련 정보를 찾고 공부하게 되었습니다.

> 예전 스터디 모임에서 유일하게 접해본 게 React 였기에 라라벨 + 리액트 조합으로 가즈아!!



### Prerequisites

이번 예제를 따라하기 위해서는 밑의 사항과 관련된 지식과 프로그램 세팅이 된 상태여야 합니다.
> 나 역시 php와 javascript 등 지식이 부족한 상태지만 무작정 따라해보는 것을 목표로 삼았습니다. 나 같은 초보자가  천천히 이해하면서 따라할 수 있는 난이도라면 누구라도 할 수 있다라는 희망을 주고 싶다.

- PHP 그리고 Larvel에 대한 기본 지식
- Javascript 그리고 React에 대한 기본 지식
- 컴퓨터에 PHP가 installed 된 상태
- 컴퓨터에 Composer가 installed 된 상태
- 컴퓨터에 Laravel installer가 installed 된 상태
- 컴퓨터에 SQLite가 installed 된 상태 

PHP,Composer,Laravel 기본 세팅은 구글에 너무 좋은 정보들이 있습니다. window환경이든 mac환경이든 참조한 내용으로 직접 install해보면서 예상치 못하게 생기는 Error와 Exception 이슈와 관련되어 다시 구글링으로 문제를 해결해 보는 걸 추천드립니다.

DB와 관련되어 mysql만 사용해봤었는데 이번에 SQLite를 처음으로 알게되었고 사용해 보았습니다. mysql과 sqlite 각각 무엇인고 장단점이 무엇인지 구글링을 통해 확인해 보세요.



### What we'll be building

예제를 만드는 목적은 라라벨에서 리액트가 어떻게 사용되는지를 보여주는 것입니다. 저희는 '할 일 관리' 앱을 만들 예정입니다.

실제 예제는 유저가 'project'를 생성할 수 있고 생성한 'project'에서 내가 해야 할 일, 즉 'task'를 추가 할 수 있는 앱입니다.
![Taskman App Gif](https://BossanovaJJu.github.io/img/taskman.gif)

1. 'main'페이지에 create new project 라는 버튼이 있고 하단에 각 project들이 리스트로 보여집니다. create 버튼을 누르게 되면 'create project' 페이지로 이동합니다.
2. 'create project' 페이지에서는 project name과 project description을 사용자가 텍스트로 입력할 수 있고 create 버튼을 누르게 되면 다시 메인페이지로 이동을 하게 되고 새로 생성된 project 내용을 리스트로 확인 할 수 있습니다.
3. 'main'페이지에 있는 project 리스트 각 우측에는 해당하는 project의 task가 몇개인지 보여지고 project를 클릭하면 해당 '클릭한 내용의 project' 페이지로 이동하게 됩니다.
4. 'project명'페이지에 들어가면 task 입력란과 add 버튼이 있고 add버튼을 누르게 되면 입력한 task가 하단의 리스트로 보여지게 됩니다. 하단 task리스트 우측에는 'completed' 체크 버튼이 있고 completed버튼을 누르면 해당 task는 리스트에서 제외되게 됩니다.
h
> 리액트로 매우 간단한 'ToDoList' 예제를 만들어 본 기억을 떠올려 보았다. (백엔드쪽 연동 X) 하도 오래되서 자세히는 기억나지 않지만  add 버튼을 눌렀을때 이벤트가 일어나면 setState와 컴포넌트의 라이프싸이클을 이용해서 할일 리스트를 생성하고 지워줬던거 같다.



### Planning the application
우리가 만들려고 하는 할 일 관리 앱 (여기서는 taskman으로 명칭을 정하겠다.) taskman 앱은 2개의 메인 컴포넌트가 필요합니다.
 - Tasks 컴포넌트 : 각 task 항목에 대해서 사용자가 편하게 할 일을 완료하기 위한 간결한 제목을 입력할 수 있는 기능이 필요하고, 각 해당 할 일에 대해 완료했는지 안했는지 보여져야 합니다.  결국 tasks는 프로젝트와 연관되거나 유사한 할 일들로 이루어져 있습니다.
 - Projects 컴포넌트 :  Projects 그룹은 프로젝트에 대한 설명과 해당하는 모든 tasks를 포함하고 있습니다.. Projects 또한 해당 프로젝트를 완료했는지 안했는지 보여져야 합니다.

	[각 해당하는 DB의 table과 field 속성] - laravel의 migrate를 이용해 생성할 예정
	- tasks : id , title, project_id, is_completed, create_at, updated_at
	- projects : id, name, description, is_completed, create_at, updated_at
	
	



## Creating 'taskman' App



### 1. Getting stated

라라벨의 새로운 프로젝트르롤 생성해보자.
```
$ laravel new taskman
```

라라벨 taskman 프로젝트가 문제없이 install 되었다면, 그 다음으로 할 일은 라라벨에 default로 설정되어 있는 Vue.js의 scaffolding을 React로 변경해야 합니다.(스캐폴딩 명령어는 라라벨 버전에 따라 다르니 라라벨 공식 사이트 문서를 참조하자.)

> 2020년3월 기준으로 최신 라라벨 프로젝트를 설치하게 되면 "laravel/framework" : 7.0 버전으로 install된다. 이번 예제에서는 7.0기준으로 진행해보았다. 
>
> 6.x 기준이라면 밑의 `artisan`명령어 입력전에 `$composer require laravel/ui`명령어로 laravel/ui 를 install해줘야 한다. 
>
> 현재 기준으로 "laravel/ui": 2.0 버전으로 install되는데 "laravel/framework" : 6.x 버전이라면 `$composer require laravel/ui "^1.2"`와 같은 1.x버전으로 install 시켜줘야 한다. 
>
> (현재 laravel/ui 2.0버전은 laravel/framework 7.0 버전에서만 사용이 가능한 거 같다.)

```
$ cd taskman
$ php artisan preset react  //laravel 5.5 기준
$ php artisan ui react      //laravel 6.x or 7.0 기준
```

문제없이 install되었다면 resource > js 디렉토리 내부에 component 디렉토리가 생성되고 `app.js`파일안에 `require('.components/Example');`코드가 추가되어 있습니다.  나중에 Example 컴포넌트에 필요한 코드들이 업데이트 될 예정입니다.

아래 명령어를 실행하여 어플리케이션 dependencies(종속성)을 설치합니다.
```
$ npm install && npm run dev
```



### 2. Creating the app - models and migrations

> 라라벨에서 Model의 개념 (Eloquent ORM) Migrations이 무엇인지 구글링을 통해 간단하게 알아보자.

위의 planning the application 부분을 기준으로 우리는 2개의 Model이 입니다. 바로  **Task** and **Project**. 아래의 코드로 2개의 모델을 만들어 봅시다.
```
$ php artisan make:model Task -m
$ php artisan make:model Project -m
```

model을 생성할때 붙이는 `-m flag` 명령어는 app 디렉토리의 model파일과 더불어 database > migrations 디렉토리에 두개의 migration 파일들을 생성시켜줍니다.

다음은 각 model의 대량할당(Mass Assignment)이 필요한 속성을 $fillable로 정의해 줍니다. 아래의 코드를 추가해 봅시다.

> 기본적인 Eloquent모델의 대량 할당(Mass Assignment)관련된 정보와 Model의 `$fillable`속성에 대한 구글링 자료를 확인해보자.

```laravel
//app > Task.php
class Task extends Model
{
    protected $fillable = ['title', 'project_id'];
}
```

역시 app/Project.php 파일을 open해서 코드를 추가해 줍시다.

```php
//app > Project.php
class Project extends Model 
{
	protected $fillable = ['title', 'description'];
	
	public function tasks() 
	{
		return $this->hasMany(Task::class);
	}
}
```

`$fillable`을 이용해 대량할당을 위한 필드 속성을 지정해줍니다.
덧붙여서 대량할당을 위한 필드 속성을 지정해주기 위해서는 Project와 Task모델 사이의 관계를 정의해줘야 하는데 그게 바로 `tasks()` 메소드입니다.

> 라라벨 Eloquent ORM의 대량할당(Mass Assign)부분에 대한 정보를 구글링해서 이해하고 넘어가보자. 나도 100% 이해하지는 못했지만 간단히 말하자면 개발자들이 편하게 데이터베이스에 많은 수의 입력을 할당할 수 있게 도와주는 기능을 라라벨에서 제공하고 있는거 같다. [참고자료 바로가기](https://dev.to/zubairmohsin33/understanding-mass-assignment-in-laravel-eloquent-orm-331g)

여기서 관계는 일 대 다 관계(one-to-many relationship)입니다. project는 많은 수의 tasks를 가질 수 있지만 반댈 task는 해당하는 하나의 특정 project에 속해 있어야 합니다.

> [라라벨 코리아 Eloquent:Relationships-관계](https://laravel.kr/docs/6.x/eloquent-relationships) 내용을 참조하자.

반대로 Task 모델에서는 반대되는 역 관계에 대해서는 따로 정의하지 않았습니다. 이번 튜토리얼의 목적에 필요한 것들만 정의하며 진행에 나갈 예정입니다.




다음으로 모델을 생성할때 `-m flag`로 추가된 migrations 파일에 밑의 코드를 추가합니다.
```php
//database > migrations > 2020_03_24_220458_create_tasks_table.php

public function up()
{
	Schema::create('tasks', function (Blueprint $table) {
		$table->increments('id');
		$table->string('title');
		$table->unsignedInesger('project_id');
		$table->boolean('is_completed')-> default(0);
		$table->timestamps();
	});
}
```
> 'is_completed' 필드의 디폴트 값 `default(0)`은 `false`로 이해하면 될 거 같다.



마찬가지로 project migrations 파일에도 밑의 코드를 추가합니다.
```php
//database > migrations > 2020_03_24_220458_create_projets_table.php
public function up()
{
	Schema::create('projects', function (Blueprint $table) {
		$table->increments('id');
		$table->string('title');
		$table->text('description');
		$table->boolean('is_completed')->default(0);
		$table->timestamps();
	});
}
```
migrations 명령어를 실행하기 전 데이터베이스 세팅을 해야 합니다. 여기 예제에서는 **SQLite**를 사용해 진행 할 예정입니다. [sqlite 공식사이트 참조](https://www.sqlite.org/index.html) 'database.sqlite' 파일을 생성하고 진행하는 taskman 디렉토리에 해당 sqlite파일을 옮깁니다. 

문제없이 완료되었다면 다음으로 '.env'파일을 수정해야 합니다.
```
// .env
DB_CONNECTION=sqlite
DB_DATABASE=/Users/macbook/Dropbox/01_Study/BackEnd/laravel/taskman/laravel_react.db -->디렉토리 경로는 각각의 경로에 맞게 적어줍니다.
```

> 나같은 경우 sqlite파일 이름을 'laravel_react.db'란 이름으로 생성했다. 진행하는데 전혀 문제가 없는거 보면 파일 확장자가 '.db' 나 '.sqlite' 이든 상관없는거 같다. 기존에 공부할때는 mysql의 mariaDB를 이용해 DB생성 및 연결을 하고 테스트를 했었는데 SQLite는 처음 사용해 보았다. 간단하게 SQLite를 정의하자면 로컬 베이스의(서버가 필요없음) DB이다. 구글링을 통해 sqlite에 대해 간단히 알아보고 넘어가는 걸 추천한다. 

마지막으로 migrations 시켜줍니다.
```
$ php artisan migrate
```



### 3. Creating the app API

첫 번째로 `routes/api.php` API 엔드포인트를 설정해 줍니다.

> 여기서 가장 헷갈린 부분은 '왜 `web.php`가 아닌 `api.php`에서 Route설정을 해줄까?'' 였다. 기존 다른 포스트의 예제들은 대부분 `web.php`에서 엔드포인트를 설정했었다.(반대로 생각하면 기존 다른 예제들을 따라하면서 왜 `web.php`에 Route설정을 해주는 걸까? 라는 질문을 먼저 던졌어야 하지 않나라는 생각이 든다.) 
>
> 그냥 넘어갈 수는 없어서 관련된 정보를 찾아본 후 최대한 간단하게 정리해 보았다. (100%이해한 부분이 아니기 때문에 자세한 내용은 직접 구글링을 해보자.) 
>
> - `web.php`
> 	- 웹 인터페이스를 위한 Route들을 정의한다.(브라우저를 통해서 유입되는 Route url 정의라고 표현할 수 있을지 모르겠다.) 
> 	- web middleware 그룹이 할당되어 있다. (ex. 세션상태,CSRF보호 등)
> - `api.php` 
> 	- stateless 즉 상태가 없는 형태의 요청들을 Route로 정의한다(??상태가 없는 형태의 요청이 의미를 아직 모르겠다.) 
> 	- api 전용 middleware 그룹이 할당되어 있다. (ex. auth,securuty 등)
> 	- prefix로 모든 Route에 자동으로 'api/' 가 붙게 된다.
> 	
>
> 여기서 **미들웨어(middleware)**란? 애플리케이션으로 들어온 HTTP요청(CRUD)을 간편하게 필터링 할 수 있게 라라벨에서 제공하는 기능이다.

```php
// routes > api.php

Route::get('project', 'ProjectController@index');
Route::post('projects','ProjectController@store');
Route::get('project/{id}', 'ProjectController@show');
Route::put('project/{project}', 'ProjectController@markAsCompleted');

Route::post('tasks', 'TaskController@store');
Route::put('tasks/{task}', 'TaskController@markAsCompleted');
```
여기서 엔드포인트 `get방식`의 'projects' 는 모든 프로젝트들을 가져오고, 'projects/{project}'는 해당하는 단일 프로젝트만을 가져옵니다.

그 다음 `post방식`의 'projects' 엔드포인트와 'tasks' 엔드포인트는 각각 프로젝트와 할 일들을 생성할 수 있습니다.

마지막 `put방식`의 'projects/{project}' 와 'tasks/{task}'는 각각 완료했는지 or 안했는지 대해 체크할 수 있습니다.




다음으로 엔드포인트로 설정해 놓은 2개의 컨트롤러 **ProjectController**와 **TaskController**를 생성시켜 줍니다.
```
$ php artisan make:controller ProjectController
$ php artisan make:controller TaskController
```



생성된 **ProjectController**에 아래의 코드를 추가해 줍니다.

```php
// app > Http > Controllers > ProjectController.php
public function index() 
{
	$projects = Project::where('is_completed', false)
						->orderBy('created_at', 'desc')
						->withCount(['task'=> function ($query) {
							$query->where('is_completed', false);
						}])
						->get();
	return $projects->toJson();
}
```
첫번째로 `index()` 메소드는 모든 프로젝트들을 가져옵니다. (가져오는 조건과 데이터 살펴보기)
 1. 프로젝트가 '완료됨'이라고 표시되지 않은 프로젝트이고(여기서는 `is_completed: false`인 프로젝트) 
 2. '완료됨'이라고 표시되지 않은 조건을 충족시키는 프로젝트들을 생성된 순서로(`create_at`) 내림차순(`desc`)으로 가져옵니다.
 3. 각 프로젝트에는 할 일들(tasks)이 포함되어 있는데, 이 포함된 할 일의 총 갯수가 몇개인지에 대한 정보도 `withCount()`메소드를 통해 가져옵니다. 여기서도 프로젝트 조건과 마찬가지로 '완료됨'이라고 표시되지 않은 할 일들의 총 숫자값을 가져옵니다.
 4. 조건에 충족한 모든 데이터($projects) 값들을 json 형식으로 return 받습니다.

> `withCount()` 메소드는 처음 보았다. 라라벨의 helper함수인거 같긴 한데 일단 여기서는 넘어가고 예제를 전부 따라해보고 자세히 알아봐야 겠다.




```php
// app > Http > Controllers > ProjectController.php
public function store(Request $request)
{
	$validateData = $request->validate([
		'title' => 'required',
		'description' => 'required'
	]);
	$project = Project::create([
		'title' => $validateData['title'],
		'description' => $validateData['description']
	]);
	return response()->json('Project created');
}
```
두번째로 `store(Request $request)` 메소드는 새로운 프로젝트를 생성하게 해줍니다.
 1. 사용자가 입력한 'title'과 'description' 수신 데이터가 각 필드에 정의된 규칙에 맞는지 `validate() 메소드`를 통해 검증됩니다.
 2. 입력한 데이터가 문제가 없다면 데이터베이스에 새로운 프로젝트 데이터를 입력할 수 있습니다.
 3. JSON 형식으로 'project가 생성되었음' 라고 응답값을 return 받습니다.




```php
// app > Http > Controllers > ProjectController.php
public function show($id)
{
	$project = Project::with(['tasks' => function($query) {
		$query->where('is_completed', 'false');
	}])->find($id);
	return $project->toJson();
}
```
세 번째로, `show($id)` 메소드 입니다.
 1. 프로젝트들이 가지고 있는 id필드 값을 이용해 단독 프로젝트를 가져옵니다.
 2. 단독 프로젝트에 포함되어 있는 할 일('tasks')들 중 '완료됨'이라고 표시되지 않은 할 일들만 가져옵니다.
 3. 조건에 충족한 모든 데이터($projects) 값들을 json 형식으로 return 받습니다.




```php
// app > Http > Controllers > ProjectController.php
public function markAsCompleted(Project $project)
{
	$project->is_completed = true;
	$project->update();
	
	return response()->json('project update');
}
```
마지막으로 `markAsCompleted(Project $project)` 메소드는 간단하게 프로젝트의 'is_completed' 값을 'true'로 세팅 후 업데이트 시켜줍니다.






다음으로 **TaskController.php**에 아래의 코드를 추가해 줍시다.
```php
// app > Http > Controllers > TaskController.php
public function store(Request $request) 
{
	$validateData = $request->validate([
		'title' = 'require'
	]);
	$task = Task::create([
		'title' = $validateData['title'],
		'project_id' => $request->project_id
	]);
	return $task->toJson();
}
public function markAsCompleted(Task $task)
{
	$task->is_completed = treu;
	$task->update();
	return response()->json('Task updated');
}
```

TaskController의 `store()`&`markAsCompleted()` 메서드들은 ProjectController 메서드들과 같은 기능을 하므로 추가적인 코멘트는 하지 않고 넘어가겠습니다.
> 예제에서는 이렇게 넘어갔지만 ProjectController와 다른점은 `project_id`유무이다. 위에서도 나왔듯이 one-to-many relationships를 이 `project_id`를 활용해서 각 프로젝트에 대한 할 일 들을 세팅해주는게 아닐까 싶다. 그러니까 각 할 일들 전부 POST방식으로 생성되면서 내가 어디 프로젝트에 속해있는지 `project_id`값을 부여받는 의미로 해석해도 될지 모르겠다. (Project Model에서 `tasks()` 메소드의 `hasMany()`메소드도 연관이 있을거 같다.)  일단 예제를 계속 진행하면서 이 부분에 대해서 나중에 정리해보도록 하자.


이제 'taskman' 어플리케이션의 API 세팅이 완료되었습니다.
> 오 지금까지 했던 것들이 API세팅 이였구나!!



다음으로 FrontEnd 쪽으로 넘어가보도록 하죠.
> 드디어 React가 등장하는구나, 위의 데이터들을 어떻게 활용해서 뿌려지게 만드는지 update는 어떻게 되는지 너무 궁금하다. (두근두근)



### 4. Creating a wildcard route

우리의 어플리케이션의 라우팅을 처리하기 위해 'React Router'를 사용할 것입니다.
그럴러면 모든 어플리케이션 루트에 대해 위한 단일 view 파일을 렌더링 해야 합니다.

```php
//routes > web.php
Route::view('/{path?}', 'app');
```
> 3.creating the app API에서 설정한 모든 루트들의 view단을 리액트로 처리하기 위한 코드가 아닐까 싶다. 여기서 문득 루트 설정을 왜 `web.php`에서 해줬을까 생각해 보았다.`api.php`는 stateless, 즉 형체가 없는 루트들을 정의한다고 나와있었는데 이 말은 즉 데이터들의 루트를 말하는게 아닐까 싶다. 반대로 'web.php'는 형체가 있는 실제로 데이터 보여지는 view와 관련된 루트를 정의하는게 아닐까?? 허허 아직은 잘 모르겠다.

여기서 와일드카드 루트를 정의했습니다.
'app.blade.php' view 파일이 모든 루트들과 관련되어 렌더링하게 되고 이 view파일 안에서 리액트 컴포넌트들이 렌더링 될 것 입니다.

이렇게 된다면 리액트가 제공하는 모든 장점들을 사용할 수 있께 됩니다.


다음으로 'resources/views' 디렉토리에 'app.blade.php' 뷰파일을 생성한 후, 밑의 코드들을 추가해 줍시다.

```php
//resources/view/app.blade.php
<!DOCTYPE html>
<html lang="{{ app()->getLocale() }}">
<head>
	<meta charest="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<!-- CSRF Token -->
	<meta name="csrf-token" content="{{ csrf-toekn }}">
	<title>Taskman</title>
	<!-- Styles -->
	<link href="{{ assets('css/app.css') }}" rel="stylesheet">
</head>
<body>
	<div id="app"></div>
	<script src="{{ asset('js/app.js') }}"></script>
</body>
</html>
```

CSS파일과 Javascript 참조 파일을 추가합니다. (리액트와 다른 의존성 묶음 포함)
id값이 'app'인 빈 div에 리액트 컴포넌트들이 렌더링 될 것 입니다.

또한 메타태그에 CSRF 토큰이 포함되어 있음을 알 수 있습니다.

우리는 Axios를 사용해 보내는 모든 HTTP 요청을 공통 header에 첨부할 것 입니다.
이것에 대한 정의는 'resource/assets/js/bootstrap,js' 파일에 할 예정입니다.

> 여기서 처음 본 코드는 `{{ app()->getLocale() }}` | `{{ csrf_token() }}` | `{{ asset('url') }}`  이렇게 3가지이다. 
>
> 1.getLocale 메서드 : app파사드의 getLocale메서드는 현재 로케일?(언어)나 주어진 로케일?(언어)를 확인할 수 있다고 나와있다. 현재 사용되고 있는 로컬언어를 참조하는 메소드가 아닐까라는 생각을 해본다. [지역화 참조](https://laravel.com/docs/7.x/localization)
>
> 2.csrf_toekn 메소드 : `csrf_token()`메소드 자체는 라라벨의 **helper 함수이다.** 라라벨은 어플리케이션에 의해 관리되는 모든 활성화된 사용자 세션마다 CSRF "토큰"을 자동으로 만들어 줍니다. 이 토큰은 인증된 사용자가 어플리케이션에 request을 할 수 있는 고유한 사용자라는 것을 확인하는데 사용된다고 나와있다. 
>
> HTML을 정의할때, CSRF토큰을 포함시켜서 CSRF보호 미들웨어가 requeset-요청을 검증할 수 있도록 해야한다고 나와있다. 분명 예전에 csrf_token메서드를 추가하지 않거나 `@csrf` 블레이드 지시어를 추가하지 않게되면 '크로스 사이트 공격'과 관련된 error 메세지가 떴었더거 같다. 나중에 테스트를 해보자. [CSRF보호 참조](https://laravel.kr/docs/6.x/csrf) 
>
> 3.asset 메소드 : `asset(url)` 역시 라라벨의 **helper 함수**로서 url변수를 설정하여 asset의 URL 호스트를 설정할 수 있다고 한다.  [helpder함수 참조](https://laravel.kr/docs/6.x/helpers#method-csrf-token)



### 5. Creating the App Component
리액트 컴포넌트들의 기본(베이스)이 'App' 컴포넌트 입니다. 
기존 'resources > js > components > Example.js' 파일의 이름을 'App.js'로 수정하고 안에 있는 코드 내용도 아래의 내용으로 업데이트 합니다.

```react
//resources > js > components > App.js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Header from './Header';

class App extends Component {
	render() {
		return(
			<Router>
				<div>
					<Header/>
				</div>
			</Router>
		)
	}
}
ReactDOM.render(<App/>, document.getElementById('app'))
```
'Header' 컴포넌트가 렌더링 되는데 모든 페이지들 부분에 'Header'컴포넌트가 렌더링 되서 들어갈 것입니다. (Header 컴포넌트 부분은 밑에서 생성 과정을 보여줄 것입니다.)

React router를 활용하기 위해서는 'react-router-dom' 이 install되어 있어야 합니다.

```
$ npm install react-router-dom
```

> 리액트는 SPA, 즉 단일 페이지 어플리케이션이다.기존의 웹어플리케이션들이 각 페이지를 요청하면 서버쪽에서 리소스를 받아와 렌더링해서 보여줬다면 리액트는 유저의 브라우저가 뷰 렌더링을 담당하고 필요한 데이터만 서버에서 전달받아 보여주는 형식이라고 한다. 
> 
> 싱글 페이지라고 해도 여러 화면이 필요하기 때문에 각 주소가 필요하다. 결국 다른 주소에 따라 다른 뷰를 보여주는 **라우팅** 기능이 필요한 것이다. 하지만 리액트 자체에는 이 기능이 내장되어 있지 않다. 그래서 그 기능을 도와주는 라이브러리 `react-router-dom`이 필요한 것이라고 한다. (브라우저에서만 사용되는 리액트 라우터이다.)

'react-router-dom'이 install되는 동안 'resource/assets/js/app.js' 파일을 열어 example로 되어있는 코드를 아래의 코드로 수정합니다.
```react
require('./components/app');
```



### 6. Creating the Header component

위에서 언급했던 'Header' 컴포넌트를 만들어 봅시다.
'resources > assets > js > components' 폴더에 'Header.js'파일을 생성해서 아래의 코드를 업데이트 합니다.

```react
import React from 'react';
import { Link } from 'react-router-dom';

const Header = () => (
	<nav className='navbar navbar-expand-md navbar-dark bg-dark navbar-laravel'>
		<div className='container'>
			<Link className='navbar-brand'>Taskman</Link>
		</div>
	</nav>
)
export default Header;
```
여기서 React Router 에서 **Link**컴포넌트를 활용하고 있다.
즉 Taskman앱 어느 페이지를 찾아가든 새로고침되지 않을 것이다.
> 위의 내용은 그대로 번역한 말인데 '새로고침 되지 않을 것이다'란 말을 잘 이해하지는 못했다. Header라고 하면 보통 어플리케이션에서는 상단 position:fixed로 모든 페이지에서 고정으로 보여지게 되는데 그 뜻을 말하는게 아닐까? (아님말구..ㅠㅠ)

> css스타일은 Bootstrap로 설정해주었다. 사실 Bootstrap을 거의 사용해 본적이 없어서 자세한 내용은 [Boostrap공식 홈페이지](https://getbootstrap.com/) 를 참조해야 겠다.

> 뒤에서 예제들을 진행하다보면 자세히 알 수 있겠지만 간단하게 `App.js`와 `Header.js`를 살펴보면 App.js에서  리액트에서 라우팅이 필요한 컴포넌트들을 <Router>로 감싸주고(현재는 <Header>가 포함되어 있다.) 나중에는 필요한 라우팅 컴포넌트들이 추가되고 <Route>기능도 사용하게 되지 않을까? 라는 생각이 문득 들었다.

### 7. Displaying all projects yet to be completed
이제 실제 project들을 보여지게 만들어 봅시다.
아직 완료되지 않은 프로젝트들의 리스트가 보여져야 합니다.
'resources > assets > js > components' 폴더안에 'ProjectList.js'파일을 생성 후 아래의 코드를 업데이트 합니다.

```react
import React, {Component} from 'react';
import {Link} from 'react-router-dom';
import Axios from 'axios';

class Project extends Component {
	constructor() {
		super()
		this.state = {
			projects: []
		}
	}
	componentDidMount() {
		Axios.get('/api/projects').then(response => {
			this.setState({
				projects: response.data
			})
		})
	}
	render() {
		const { projects } = this.state;
		return(
			<div className='container py-4'>
				<div className='row justify-content-center'>
					<div className='col-md-8'>
						<div className='card'>
							<div className='card-header'>All Projects</div>
							<div className='card-body'>
								<Link className='btn btn-primary btn-sm mb-3' to='/create'>
                  Create New Project 
								</Link>
								<ul className='list-group list-group'>
									{projects.map(project => (
										<Link 
											className='list-group-item list-group-item-action d-flex justify-content-between align-items-center'
											to={`/${project.id}`}
											key={project.id}
										>
											{project.name}
											<span className='badge badge-primary badge-pill'>
												{project.tasks_count}
											</span>
										</Link>
									))}
								</ul>
							</div>
						</div>
					</div>
				</div>
			</div>						
		)
	}
}
export default ProjectList
```

1. 'projects' state를 빈 배열의 초기화 상태로 정의했습니다.
2. 리액트 라이프 싸이클인 `componentDidMount`메소드를 사용했습니다.
3. 앱의 API엔드포인트에 대한, 즉 완료되지 않음이라고 표시되어 있는 프로젝트들을 가져오는 HTTP요청을 `Axios`라이브러리를 활용합니다. 그렇게 되면 요청한 데이터에 대한 응답 데이터를 앱의 API로부터 가져온 후 'projects'의 state로 업데이트 됩니다.
4. 그 결과 'projects'의 state에 대한 업데이트가 반복되면서 프로젝트들의 전체 목록이 보여지게 됩니다.

> Axios는 node.js와 브라우저를 위한 HTTP 통신 js 라이브러리 fetch API와 비교했을때 장점도 더 많다고 나와있다.(axios는 여기서 처음 써보았다)
> 기존에 **3.Creating the app API**에서 설정한 '/api/projects' Route에 index 컨트롤러 메소드를 활용하게 되는거 같다. 얼핏 보자면 백엔드단 라라벨에서 API로 DB에 있는 데이터를 중간에서 Axios가 받아와 리액트로 전달해즈고 결국 프론트엔드단의 리액트의 `projects` state값에 저장되어 실제 우리에게 보여지는게 아닐까? 

테스트 하기 전, `App.js` 컴포넌트에 아래의 코드를 업데이트 합시다.

```react
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Header from './Header';
import ProjectList from './ProjectList';

class App extends Component {
	render() {
		return(
			<Router>
				<div>
					<Header />
					<Switch>
						<Route exact path='/' component={ProjectList} />
					</Switch>
				</div>
			</Router>		
		)
	}
}
ReactDOM.render(<App />, document.getElementById('app'));
```
여기서 '/'(mainPage) 새로운 라우트를 추가했습니다.
언제 어디서든 '/' 경로로 방문한다면 `ProjectList` 컴포넌트가 렌더링 될 것입니다.

### 8. Creating a new project
'ProjectList'컴포넌트에는 `<Link to='/create'>`로 새로운 프로젝트 생성 페이지로 링크되어 있습니다. 해당 페이지를 만들어 봅시다.
'resources > assets > js > components' 폴더 안에 **NewProject.js**파일을 생성하고 아래의 코드를 따라 업데이트 해 줍니다.

```react
//resources > assets > js > components > NewProject.js
import React, { Component } from 'react';
import Axios from 'axios';

class NewProject extends Component {
	constructor(props) {
		super(props);
		this.setState = {
			name: '',
			description: '',
			erros: []
		}
	}
	handleFieldChange = event => {
		this.setState({
			[event.target.name]: event.target.value
		})
	}
  handleCreateNewProject = event => {
  	event.preventDefault();
  	const { history } = this.props;
  	const project = {
  		name: this.state.name,
  		description: this.state.description
  	}
  	
  	Axios.post('/api/projects', project)
  	.then( response => {
  		history.push('/')
  	})
  	.catch( error => {
  		this.setState({
  			errors: error.response.data.errors
  		})	
  	})
  }
  hasErrorFor = field => {
		return !!this.state.errors[field]
	}
	renderErrorFor = field => {
		if(this.hasErrorFor(field)) {
			return(
				<span className='invalid-feedback'>
					<strong>{this.state.erros[field][0]}</strong>
				</span>
			)
		}
	}
	render() {
		return(
			<div className='container py-4'>
				<div clasaName='row justify-content-center'>
					<div className='col-md-6'>
						<div className='card'>
							<div className='card-header'>Create New Project</div>
							<div className='card-body'>
								<form onSubmit={this.handleCreateNewProject}>
									<div className='form-group'>
										<label htmlFor='name'>Project Name</label>
										<input
											id='name'
											type='text'
											className={ `form-control ${this.hasErrorFor('name') ? 'is-invalid' : ' '}` }
											name='name'
											value={this.state.name}
											onChange={this.handleFieldChange}
										/>
										{this.renderErrorFor('name')}
									</div>
                  <div className='form-group'>
                  	<label htmlFor='description'>Project description</label>
                  	<textarea
                  		id='description'
                  		className={ `form-control ${this.hasErrorFor('description' ? 'is-invalid' : ' ')}` }
                  		name='description'
                  		row='10'
                  		onChange={this.handleFieldChange}
                  	/>
                  	{this.renderErrorFor('description')}
                  </div>
                  <button className='btn btn-primary'>Create</button>
                 </form>
                </div>
              </div>
            </div>
         </div>
    	</div>             
		)
	}	
}
```

> 원문의 코드에서는 `constructor()`메소드에서 각 메소드들에 대해 this 바인딩을 위한 `this.handleFieldChange = this.handleFieldChange(this)`코드가 추가되어 있었다. 번거롭기도 하고 일정한 this를 상속받기 위해서 화살표 함수를 사용했다.

1. 위의 컴포넌트는 새로운 프로젝트 생성을 위한 form을 렌더링 해줍니다.  'name & description & errros'의 state값들을 초기화 해주었습니다.
2. 새로운 프로젝트 생성폼 안의 input 필드(name & descipriotn)에서 텍스트가 입력될때 즉, onChange이벤트가 발생할때마다 호출되는 `handleFieldChange()`메소드를 정의했습니다. 이벤트로 발생한 input값들을 `[event.target.name]`속성을 이용해 초기화 되어있는 state(name & description) 각각에 값을 전달해 줍니다.
3. 다음으로 submit 제출 버튼을 눌렀을때 발생하는 `handleCreateNewProject()` 메소드를 정의했습니다.
	- 첫번째로 사용자의 폼 제출과 과련 기본 동작을 막기 위해 `preventDefault`를 호출합니다.
	- 두번째로 `this.props`를 이용해 `history`에 부모의(ProjectList) state값을 넣어 줍니다. (나중에 HTTP요청으로 제대로 데이터를 가져왔다면 부모의(ProjectList) state값을 업데이트 시켜줘야 합니다.)
	- 세번째로 우리가 설정해놓은 앱 API 엔드포인트에 폼 데이터(project)와 함께 HTTP 요청을 전달합니다. 만약 문제가 없다면 new project 생성 버튼을 눌렀을때 간단히 **redirect**되어 'ProjectList' 컴포넌트 페이지로 리디렉트 될 것입니다. (반대 상황이라면 앱 API 에서 가져온 응답 error 데이터를 state의 errors에 업데이트 될 것입니다.)
4.`hasErrorFor()`메소드는 지정된 필드값들이(name,description)이 에러가 있는지 없는지를 체크한 후 'true','false'값을 리턴시켜 줍니다. 결국 `renderErrorFor()`메소드에서 이 리턴값을 이용해 필드값에 문제가 있을 경우 에러메세지를 노출시켜 줍니다.

ProjectList 컴포넌트와 마찬가지로, `App` 컴포넌트에 `NewProject`를 추가시켜 줍시다.
```react
import React, { Component } from 'react';
import ReactDOM fro 'react-dom';
import { Browser as Router, Switch, Router } from 'react-router-dom';
import Header from './Header';
import ProjectList from './components/ProjectList';
import NewProject from './components/NewProject';

class App extends Component {
	render() {
		return(
			<Router>
				<div className='container'>
					<Header />
					<Switch>
						<Route exact path='/' component={ProjectList} />
						<Route path='/create' component={NewProject} />
					</Switch>
				</div>
			</Router>
		)
	}
}
ReactDOM.render('<App />', document.getElementById('app'))
```
위와같이 새로운 프로젝트를 만들기 위해 `/create` 루트를 정의했습니다. 해당 경로(페이지)에 방문할때마다 `NewProject`컴포넌트가 렌더링 될 것입니다.





### 9. Display a single project
지금부터 단일 프로젝트가 보여지게 해봅시다.
아시다시피 `ProjectList`컴포넌트에서는 각 project의 id값 순서로 프로젝트들이 리스트로 보여집니다.

`resource/assets/js/components` 디렉토리에 `SingleProject.js`파일을 생성 후 아래의 코드를 추가해 줍니다.

```react
//resources/assets/js/components/SingleProject.js
import React, { Component } from 'react';
import Axios from 'react-router-dom';

class SingleProject extends Component {
	constructor(props) {
		super(props)
		this.state = {
			project: {},
			tasks: []
		}
	}
	
	componentDidMount() {
		const projectId = this.props.match.params.id
		Axios.get(`/api/projects/${projectId})`).get(response => {
			this.setState({
				project: response.data,
				tasks: response.data.tasks
			})
		})
	}
	render() {
		const { project, tasks } = this.props;
		return(
			<div className='container py-4'>
				<div className='row justify-content-center'>
					<div className='col-md-8'>
						<div className='card'>
							<div className='card-header'>{project.title}</div>
							<div className='card-body'>
								<p>{project.description}</p>
								<button className='btn btn-primary btn-sm'>
									Mark as completed
								</button>
								<hr />
								<ul className='list-group mt-3'>
									{tasks.map(task => (
										<li 
											className='list-group-item d-flex justify-content-between align-items-center'
											key={taks.id}
										>
											{task.title}
											<button className='btn btn-primary btn-sm'>
												Mark as completed
											</button>
										</li>
									))}
								</ul>
							</div>
						</div>
					</div>
				</div>
			</div>
		)
	}
}
export default SingleProject;
```
> 단일 project값을 받아오는 거면 왜 `project state 속성값은` 단순히 문자열 값 `' '`이 아닌 초기화 할 줄 알았는데 `{}`로 초기화를 해주었는지 이해가 가지 않았다. 
> projects table이 가지고 있는 `id,description,title,is_completed`모든 필드 값들을 담아주기 위해서 `{}` 필드값으로 할당해주었다는걸 나중에 알게 되었다.

1. project와 task 각 state 속성 값들을 정의해 줍니다.
2. project state 속성값은 지정된(해당 projectId) 프로젝트값을 할당해주고 tasks state 속성값은 해당 프로젝트에 포함되어 있는 할일 목록들을 할당해 줍니다.
3. `componentDidMount`라이프 싸이클 메소드는 Axios를 이용해 해당 프로젝트의 지정된 `projectId`값을 가져오기 위해 우리가 설정해 놓은 앱 API로 HTTP 요청을 보냅니다.
4. `this.props.match.params.id`를 사용해 가져온 `projectId`값은 URL로 전달됩니다.
5. 응답 데이터에 문제가 없다면 state속성값(project & tasks)들이 setState로 업데이트 됩니다.

> `this.props`는 잘 알고 있었지만 `this.props.match.params.id`는 여기서 처음 보게 되었다. this.props 필드값들 중 id와 매칭되는 필드값을 가져오는게 아닐까 싶다.

마침내, 프로젝트와 속해있는 할 일들에 대해 자세히 보여지게 되었습니다. 또한 각 프로젝트와 할일들이 완료가 되었는지를 체크할 수 있는 버튼도 추가되었습니다.

다음으로 `App`컴포넌트에 `SingleProject`컴포넌트를 추가해 줍시다.

```react
import React, { Component } from 'react';
import { BrowserRouter as Router, Switch, Route } from 'react-router-dom';
import ProjectList from './components/ProjectList';
import Header from './components/Header';
import NewProject from './components/NewProject';
import SingleProject from './components/SingleProject';

class App extends Component {
	render() {
		return(
			<Router>
				<div className='container'>
					<Header />
					<Switch>
						<Route exact path='/' component={ProjectList}/>
						<Route path='/create' component={NewProject}/>
						<Route path='/:id' component={SingleProject}/>
					</Switch>
				</div>
			</Router>
		)
	}
}
export default App;
```
> `path값`이 `/:id` 이런식으로 들어가는 건 처음 본다.






### 10. Marking a project as completed

프로젝트가 완료되었는지 표시할 수 있는 코드를 추가해 봅시다.
`SinglProject 컴포넌트`에 아래의 코드를 업데이트 합니다.
```react
//resources > assets > js > components > SingleProject.js
//메소드 추가하기
handleMarkProjectAsCompleted = () => {
	const { history } = this.props;
	Axios.put(`/api/projects/${this.state.project.id}`)
		.then(response => history.push('/'))
}
```

1. `markAsCompleted 버튼`을 클릭하게 되면 `handleMarkAsCompleted()`메소드가 호출됩니다.
2. 이 메소드는 프로젝트 ID에 따라 완료되었는지 표시하게 하는 우리가 설정해놓은 앱의 API(api.php)에 PUT HTTP요청을 하게 됩니다.
3. 만약 요청이 성공했다면, 메인페이지로 리다이렉트 되어서 새로운 projects 리스트로 업데이트 될 것 입니다.

다음으로 `SingleProject 컴포넌트` 하단에 있는 `markAsCompleted`버튼의 클릭이벤트에 `handleMarkAsCompleted`메소드를 추가해 줍니다.

```react
//resources > assets > js > components > SingleProject.js
<button 
	class='btn btn-primary btn-sm'
	onClick={this.handleMarkAsCompleted}
>
	markAsCompleted
</button>
```

### 11. Adding a task to project
프로젝트에 새로운 task를 추가하는 기능을 추가해 봅시다.
`SingleProject` 컴포넌트에 아래의 코드를 추가합니다.

```react
// resources > assets > js > components > SingleProject.js

//constructor 초기화 함수에 state 값 추가하기
this.state = {
	..., //기존에 선언한 project,tasks속성값들
	taksTitle: '',
	taskErrors: []
}

//다른 메서들과 같은 위치에 추가
handleFieldChange = event => {
	this.setState({
		tasksTitle: event.target.value
	})
}
handleAddNewTask = event => {
	event.preventDefault()
	const task = {
		taskTitle: this.state.taskTitle,
		projectId: this.state.project.id
	}
	Axios.post('/api/tasks', task)
		.then(response => {
			this.setState({
				taskTitle: ''
			})
			this.setState(prevState => ({
				tasks: prevState.tasks.concat(response.data)
			}))
		})
		.catch(error => {
			this.setState({
				taskErrors: error.response.data.errors
			})
		})
}
hasErrorFor = field => {
	return !!this.state.taskErrors[field]
}
renderErrorFor = field => {
	if(this.hasErrorFor) {
		return (
			<span className='invalid-feedback'>
				<strong>{this.state.taskErrors[field][0]}</strong>
			</span>
		)
	}
}
```
1. `handleFieldChange() & hasErrorFor() & renderErrorFor()` 메소드들은 모두 `NewProject`컴포넌트와 동일합니다. (증복되는 설명은 빼도록 하겠습니다.)
2. `handleAddNewTask`메소드 역시 `NewProject > handleCreateNewProject`메소드와 비슷합니다.
> 본문에서는 위의 2가지 메소드 차이점에 대한 따른 언급은 없었다. 분명 input에 입력한 값을 submit로 새로운 project와 task를 생성하는 기능은 똑같지만, API에 요청한 데이터를 받아온 후 리스트에 업데이트 하는 방식은 차이가 있다. 
> `NewProject`컴포넌트에서는 const { history } = this.props; / history.push('/') 활용해서 업데이트 되고 `SingleProject`컴포넌트에서는 처음 보게 된 this.setState()의 `prevState`를 이용해 'tasks:prevState.tasks.concat(response.data)'로 업데이트 되는 차이가 있다.
3. 만약 API에 HTTP요청이 문제가 없다면 첫번째로 form의 input value값을 초기화 시킨 다음 state tasks에 새로운 task가 업데이트 되어 리스트로 보여질 것입니다.

다음으로, `SingleProject > render() > hr/` 코드 밑에 아래의 코드를 추가해 줍니다.
```react
<form onSubmit={this.handleAddNewTask}>
	<div className='input-group'>
		<input
			type='text'
			name='title'
			className={`form-control ${this.hasErrorFor('title') ? 'is-invalid' : ''}`}
			placeholder='Task title'
			value={this.state.taskTitle}
			onChange={this.handleFieldChange}
		/>
		<div className='input-group-append'>
			<button className='btn btn-primary'>Add</button>
		</div>
		{this.renderErrorFor('title')}
	</div>
</form>
```
이로서 새로운 task를 추가하는 기능 구현이 완료되었습니다.

### 12. Marking a task as completed
마지막으로 각 task에 완료되었는지 체크하는 기능을 만들어 봅시다.
위에서 다뤘던 project에서 완료 여부를 체크하는 기능과 거의 유사합니다.
아래의 코드를 `SingleProject`컴포넌트에 추가해 봅시다.
```react
//resources > assets > js > components > SingleProject.js

handleMarkTaskAsCompleted = taskId => {
	Axios.put(`/api/tasks/${taskId}`).then(reponse => {
		this.setState(prevState => ({
			tasks: prevState.tasks.filter(task => {
				task.id !== taskId
			})
		}))
	})
}
```
1. 위에서 `handleMarkProjectAsCompleted` 메소드와 같지 않습니다.
> project단에 있는 `markAsCompleted`버튼을 누르게 되면 `history.push('/')`로 메인페이지로 리다이렉트 되었었다.
2. `handleMarkTaskAsCompleted`메소드에서는 task단에 있는 `markAsCompleted` 버튼을 누르게 되면 bind(this, task.id)를 이용해 현재 사용자가 클릭한 task의 해당하는 id값을 bind를 이용해 가져올 수 있습니다.
3. 결국 이 id값으로 앱 API에 put방식으로 HTTP 요청을 하게 됩니다.
4. 문제없이 요청이 성공했다면 `filter()`메소드를 활용해 현재 클릭한 task id값과 기존의 task들의 각id값을 전부 비교하여 같지 않은 task들만 state tasks에 업데이트 되어  task 리스트가 새롭게 보여지게 됩니다,

다음으로 Task단 `markAsCompleted` 버튼에 핸들러 메소드를 추가해 줍니다.
```react
//resources > assets > js > components > SingleProject.js 
<button 
	className="btn btn-primary btn-sm"
	onClick={this.handleMarkTaskAsCompleted.bind(this, task.id)}
/>
```
> `onClick={() => this.handleMarkTaskAsCompleted(task.id)}` 이런식으로 화살표 함수를 이용해 bind(this)를 해주지 않아도 된다.

위의 버튼을 클릭하게되면 `handleMarkTaskAsCompleted`메소드에 해당 task의 id값을 전달해 줍니다.

### 13. Testing out the APP
모든 작업 과정이 완료 되었습니다. 
앱 테스트를 하기 전에 [라라벨 mix](https://laravel.kr/docs/6.x/mix)를 활용해 자바스크립트를 컴파일 하기 위해 터미널의 아래의 코드를 실행 시킵니다.
```
$npm run dev
```
문제없이 진행되었다면 로컬 서버 http://127.0.0.1:8000 에서 직접 테스트 해봅시다.
```
$php artisan serve
```

#### !! 실제 테스트를 진행하면서 생긴 Error 이슈와 해결방법 !!
> 아무 문제없이 테스트를 해볼 수 있을 거란 예상은 하지 않았다.
##### [Erro.1] `$npm run dev` 명령어 실행시 발생
```
//에러 메세지를 간단히 정리해보자면
Support for the experimental syntax 'classProperties' isn't currently enabled (14:20):

  21 |          })
  22 |  }
> 23 |  handleMarkProjectAsCompleted = () => {
     |                               ^
  24 |          const { history } = this.props;
  25 |          axios.put(`/api/projects/${this.state.project.id}`)
  26 |                  .then(response => history.push('/'))
  
Add @babel/plugin-proposal-class-properties (https://git.io/vbSL) to the 'plugin' section of your Babel config to enable transformation.
```
> 뭔가 `@babel/plugin-proposal-class-properties` 가 install되지 않았고 그와 관련된 설정을 해주지 않아서 생기지 않았을까 라는 생각을 해보았다. 해결방법을 위해 구글 서치 후 정리해 보았다.

[해결방법]
1.@babel/plugin-proposal-class-properties 설치하기
```
npm install --save-dev @babel/plugin-proposal-class-properties
```
2.`.babelrc`파일 생성 후 아래의 코드 추가
```
{
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react"
     ],
    "plugins": [
        "@babel/plugin-proposal-class-properties"
     ]
}
```


##### [Error.2] 각종 오타로 생긴 문제들
1. ProjectList.js에서 <Link to={'/${project.id}'} '이 아닌 `로 오타 수정
2. SinglProject.js에서 state의 taskTitle과 <input name='title'> 미스 매칭으로 taskTitle->title로 수정
3. `Uncaught (in promise) TypeError: _this2.state is not a function`이런 에러 메세지가 나왔었는데 알고보니 SingleProject.js > componentDidMount메소드에서 this.setState({})가 아닌 this.state로 되어 있었음

> 위의 이슈들을 전부 수정하고 나니 아무 문제없이 Taskman APP을 테스트 할 수 있었다.


#### //작업 후기 ####
처음으로 영어 원문을 번역해가며 예제를 따라 만들어 보았다. 회사에 일찍 출근해 짬짬히 진행해서 그런지 시간이 엄청나게 소비된거 같다.

기존에는 코드 나온거만 보고 대충 따라 쳐보고 결과물에만 포커스를 맞췄다면 이번에는 과정, 즉 왜 이 코드를 추가했는지 어떤 역할을 하는지 등 원문을 차분히 번역해가며 이해하는데에 집중했다.

나의 모자른 코딩 지식과 영어 지식에 다시 한번 나를 되돌아보는 계기가 되었던거 같다. (모르는 영단어가 이리 많을 줄이야 ㅠㅠ) 난생 처음으로 포기 하지 않고 끝까지 물고 늘어져 완료한 점에 대해서는 나 자신한테 고생했다고 말해주고 싶다.

결과물만 봤을때 굉장히 간단한 웹 어플리케이션이지만 백엔드 단의 라라벨과 프론트 단의 리액트가 어떻게 연동이 되는지 전체적으로 이해하는데 정말 좋은 예제가 된거 같다.

다음으로 간단하게 만든 웹 서비스를 AWS(아마존 웹 서비스)에 직접 업로드 해보고 빌드와 버전관리 등을 체험해볼지, 리액트의 데이터 관리 state가 아닌 mobX나 redux를 활용해보는 공부를 해볼지 고민해봐야겠다.

이 예제 덕분에 와이프의 맥북을 몰래 사무실로 가져와 맥북에 익숙해지고 언제 어디서든 맥북만 있으면 코딩할 수 있는 작은 습관을 들일 수 있게 되어 정말 감사하게 생각한다.




