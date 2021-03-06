Using New Faker Provider in Laravel Package Development
=======================================================

For testing purposes I needed to extend Fzaninotto/Faker to provide random details for use in a new
Laravel package relating to pets. These details are made available in new provider PetsFakerServiceProvider. This provider
is located in package Osirl\Pets which resolves to /packages/osirl/pets/src/Providers/PetsFakerServiceProvider.php
The structure is further explored below

.. code-block:: php
  
  <?php
    namespace Osirl\Pets\Providers;
    class PetsFakerServiceProvider extends \Faker\Provider\Base {
      protected static $dogNames = ['Rover', 'Spot','Fido', 'Butch'];
      protected static $dogBreeds = ['Labrador', 'Spanial','Cocker','Boxer', 'Schnauzer'];
      protected static $dogGender = ['Male', 'Female'];
      protected static $dogBirthDates = ['25/3/2017', '13/8/2015', '2/12/2016'];
      protected static $dogDescriptions = ['Short fat hairy legs', 'Long skinny hairy legs', 'Cheerful Chappie'];

      public static function dogName(){ return static::randomElement(static::$dogNames);}
      public static function dogBreed(){return static::randomElement(static::$dogBreeds);}
      public static function dogGender(){return static::randomElement(static::$dogGender);}
      public static function dogBirthdate(){return static::randomElement(static::$dogBirthDates);}
      public static function dogDescription(){return static::randomElement(static::$dogDescriptions);}
    }

To use the extended Faker functionality, it must be made available to the web page. To do this, add the following to the routes file

.. code-block:: php

  Route::get('create',  function(){
    $faker = new Faker\Generator();
    $provider = new Osirl\Pets\Providers\PetsFakerServiceProvider($faker);
    $faker->addProvider($provider);
    return view('pets::dogs.create',['faker' => $faker]);
  });

  <!-- This next snippet is an excerpt from the web page showing how to use the new Faker provider -->
  <div class="form-group ">
    {{ Form::text('name', $faker->dogName, ['class' => 'form-control', 'placeholder' => 'Name']) }}
    {{ $errors->first('name','<span class="help-block">:message</span>') }}
  </div>
  
The package structure is as follows

.. code-block::

  ──packages
    └──osirl
       └──pets
          └──src

The package namespace is Osirl\\Pets as shown below from the composer.json file

.. code-block::

  "autoload": {
    "psr-4": {
        "App\\": "app/",
        "Osirl\\Pets\\": "packages/osirl/pets/src"
    },
  
