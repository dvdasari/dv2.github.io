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

Add Column to an existing table: <br>
`bin/cake bake migration AddPriceToProducts price:decimal`

Add column and index to an existing table: <br>
`bin/cake bake migration AddNameIndexToProducts name:string:index`

And for more on migrations, [check this page](http://book.cakephp.org/3.0/en/migrations.html)

### Create Model, Views, Controllers (equivalent of scaffolding in Rails)

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
