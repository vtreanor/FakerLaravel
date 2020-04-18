Using New Faker Provider in Laravel Package Development
=======================================================

For testing purposes I needed to extend Fzaninotto/Faker to provide random details for use in a new
Laravel package relating to pets. These details are made available in new provider PetsFakerServiceProvider.

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


.. code-block:: html

  <?php
    $faker = new Faker\Generator();
    $provider = new Osirl\Pets\Providers\PetsFakerServiceProvider($faker);
    $faker->addProvider($provider);
  ?>
  
