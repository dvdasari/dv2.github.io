---
layout:      post
title:       CakePHP 3 Notes for a Rails Developer
---

Run server locally: <br>
`bin/cake server`

### Migrations
Run migrations: <br>
`bin/cake migrations migrate`

Rollback migrations: <br>
`bin/cake migrations rollback`

Create migration: <br>
`bin/cake bake migration CreateProducts name:string description:text created modified`

  * The primary key column named `id` will be added implicitly.
  * default column type will be `string` if not mentioned
  * `created` and `modified` will be created as `datetime`

Add Column to an existing table: <br>
`bin/cake bake migration AddPriceToProducts price:decimal`

Add column and index to an existing table: <br>
`bin/cake bake migration AddNameIndexToProducts name:string:index`

And for more on migrations, [check this page](http://book.cakephp.org/3.0/en/migrations.html)

### Create Model, Views, Controllers (equivalent to scaffolding in Rails)

1. Create the migration
2. Run the migrations
3. Run bake all command for the resource. For products that would be: <br>
`bin/cake bake all products`

Model validations are done in: <br>
`src/Model/Table/ProductTable.php`
inside `validationDefault` method

Example validation:

{% highlight php %}
public function validationDefault(Validator $validator)
{
  $validator
    ->notEmpty('name')
    ->add('name', 'unique', ['rule' => 'validateUnique', 'provider' => 'table', 'message' => 'Name should be unique']);

  return $validator;
}
{% endhighlight %}


Model associations are done in: (same file as above)<br>
`src/Model/Table/ProductTable.php`
inside `initialize` method

{% highlight php %}
public function initialize(array $config)
{
  ....
  $this->belongsTo('Projects', [
      'foreignKey' => 'project_id'
  ]);
  ....
}
{% endhighlight %}

### Console

Is there a console similar to `bin/rails c` ?
Yes there is one

`bin/cake console`

To find a list of all projects:

```
>>> $projects = Cake\ORM\TableRegistry::get('Projects');
=> App\Model\Table\ProjectsTable {#208
     +"registryAlias": "Projects",
     +"table": "projects",
     +"alias": "Projects",
     +"entityClass": "App\Model\Entity\Project",
     +"associations": [],
     +"behaviors": [
       "Timestamp",
     ],
     +"defaultConnection": "default",
     +"connectionName": "default",
   }

>>> $projects->find()->all();
=> Cake\ORM\ResultSet {#242
     +"items": [
       App\Model\Entity\Project {#256
         +"id": 1,
         +"name": "Project one update",
         +"description": "description here ",
         +"created": Cake\I18n\FrozenTime {#257
           +"time": "2016-09-02T14:43:29+00:00",
           +"timezone": "UTC",
           +"fixedNowTime": false,
         },
         +"modified": Cake\I18n\FrozenTime {#244
           +"time": "2016-09-02T14:43:38+00:00",
           +"timezone": "UTC",
           +"fixedNowTime": false,
         },
         +"[new]": false,
         +"[accessible]": [
           "*" => true,
         ],
         +"[dirty]": [],
         +"[original]": [],
         +"[virtual]": [],
         +"[errors]": [],
         +"[invalid]": [],
         +"[repository]": "Projects",
       },
     ],
   }
```

### List Routes: <br>
`bin/cake routes`

### Creating namespaced controllers
for creating routes like `/admin/users/5`

To bake everything: <br>
`bin/cake bake all Users --prefix=admin`

To bake `controller`: <br>
`bin/cake bake controller Users --prefix Admin`

To bake `views`: <br>
`bin/cake bake template Users --prefix Admin`

### How to do User password hashing?

Open the user entity file `src/Model/Entity/User.php` <br>
Add `use Cake\Auth\DefaultPasswordHasher;` <br>
Add this method: <br>

```
protected function _setPassword($password)
{
  return (new DefaultPasswordHasher)->hash($password);
}
```
Now user password will be hashed whenever a user is created or updated.


### How to add Authentication component and allow user to login and logout.

Open `src/Controller/AppController.php` <br>
Add the below inside `initialize` method.
`$this->loadComponent('Auth');`

```
$this->loadComponent('Auth', [
  'authenticate' => [
    'Form' => ['fields' => ['username' => 'username']]
  ]
]);
```

CakePHP standard is to use an `username` and `password` for loging in. If `email` is used instead of `username`, the code will look as below:

```
$this->loadComponent('Auth', [
  'authenticate' => [
    'Form' => ['fields' => ['username' => 'email']]
  ]
]);
```

Next we need to setup a login and logout method:

Open `src/Controller/UsersController.php`

and paste the login and logout methods

```
public function login()
{
    if ($this->request->is('post')) {
        $user = $this->Auth->identify();
        if ($user) {
            $this->Auth->setUser($user);
            return $this->redirect($this->Auth->redirectUrl());
        }
        $this->Flash->error(__('Invalid email or password, try again'));
    }
}

public function logout()
{
    return $this->redirect($this->Auth->logout());
}
```

create Login Form at: `src/Template/Users/login.ctp`

```
<div class="users form">
<?= $this->Flash->render('auth') ?>
<?= $this->Form->create() ?>
    <fieldset>
        <legend><?= __('Please enter your email and password') ?></legend>
        <?= $this->Form->input('email') ?>
        <?= $this->Form->input('password') ?>
    </fieldset>
<?= $this->Form->button(__('Login')); ?>
<?= $this->Form->end() ?>
</div>
```

### How to show user logged in / logged out info in navbar

Add this in `src/Template/Layout/default.ctp`

```
<ul class="right">
  <?php if ($this->request->session()->read('Auth.User')): ?>
    <li><a>Logged in as <?php echo $this->request->session()->read('Auth.User.email') ?></a></li>
    <li><a href="/users/logout">Logout</a></li>
  <?php else: ?>
    <li><a href="/users/login">Login</a></li>
  <?php endif; ?>
  <li><a href="/">Home</a></li>
</ul>
```
