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
