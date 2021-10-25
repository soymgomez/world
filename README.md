
<p><img src="https://eu.ui-avatars.com/api/?name=Najm+Njeim?size=100" width="100"/></p>

A Laravel package to provide a list of the countries, cities, timezones, currencies and phone numbers formatting/validation helpers.

The package can be consumed through Facades, Helpers and Api routes.

## Installation  

```
composer require nnjeim/world

php artisan vendor:publish --tag=world

php artisan migrate

php artisan db:seed --class=WorldSeeder (requires ~ 5 - 10min)
```

### Usage

#### World Facade
``` 
use Nnjeim\World\World;

$action =  World::countries([
    'fields' => 'state,cities',
    'filters' => [
        'iso2' => 'FR',
    ]
]);

if ($action->success) {

    $countries = $action->data;
}
``` 

#### Country Helper Class
``` 
use Nnjeim\World\WorldHelper;

protected $world;

public function __construct(WorldHelper $world) {

    $this->world = $world;
}

$action = $this->world->cities([
    'country_id' => 182,
]);

if ($action->success) {

    $countries = $action->data;
}
```

### Available methods

| | | | |
| :--- | :--- |:--- |:--- |
| Name | Description | Arguments | Filterable |
| countries | lists all the countries | string fields | array filters |
| states | lists all the states | string fields | array filters |
| cities | lists all the cities | string fields | array filters |
| timezones | lists all the timezones | string fields | array filters |
| currencies | lists all the currencies | string fields | array filters |
| stripPhone | returns the stripped phone number with the country dialling code | number, phone_code | no |
| formatPhone | returns the formatted phone number with the country dialling code | number, phone_code | no |
| validatePhone | validated the format of a given phone number | number, phone_code | no |

### Countries database table fields
```
id, name, iso2, iso3, phone_code, dialling_pattern, region, sub_region, status
```
### States database table fields
```
id, name, country_id
```
### Cities database table fields
```
id, name, state_id, country_id
```
### Timezones database table fields
```
id, name, country_id
```
### Currencies database table fields
```
id, country_id, name, code, precision, symbol, symbol_native, symbol_first, decimal_mark, thousands_separator
```

### Available routes

All routes can be prefixed by any string. Ex admin, api, v1 ...  

##### Countries
| | |
| :--- | :--- |
| Method | GET |
| Route | /{prefix}/countries |
| Parameters* | comma seperated fields(countries table fields in addition to states, cities, currency and timezones), array filters |
| Example | /v1/countries?fields=iso2,cities&filters[phone_code]=44 |   
| response | success, message, data |  

##### States
| | |
| :--- | :--- |
| Method | GET |
| Route | /{prefix}/states |
| Parameters* | comma seperated fields(states table fields in addition to country and cities), array filters |
| Example | /v1/states?fields=country,cities&filters[country_id]=182 |   
| response | success, message, data |   

##### Cities
| | |
| :--- | :--- |
| Method | GET |
| Route | /{prefix}/cities |
| Parameters* | comma seperated fields(states table fields in addition to country and state), array filters |
| Example | /v1/cities?fields=country,state&filters[country_id]=182 |   
| response | success, message, data | 

##### Timezones
| | |
| :--- | :--- |
| Method | GET |
| Route | /{prefix}/timezones |
| Parameters* | comma seperated fields(states table fields in addition to the country), array filters |
| Example | /v1/timezones?fields=country&filters[country_id]=182 |   
| response | success, message, data | 

##### Currencies
| | |
| :--- | :--- |
| Method | GET |
| Route | /{prefix}/timezones |
| Parameters* | comma seperated fields(states table fields in addition to the country), array filters |
| Example | /v1/timezones?fields=country&filters[country_id]=182 |   
| response | success, message, data |

##### Validate Number
| | |
| :--- | :--- |
| Method | POST |
| Route | /{prefix}/phones/validate |
| Parameters | key, value |
| Example | /v1/phones/validate?number=060550987&phone_code=33 |

##### Strip Number
| | |
| :--- | :--- |
| Method | POST |
| Route | /{prefix}/phones/strip |
| Parameters | key, value |
| Example | /v1/phones/strip?number=060550987&phone_code=33 |

##### Format Number
| | |
| :--- | :--- |
| Method | POST |
| Route | /{prefix}/phones/format |
| Parameters | key, value |
| Example | /v1/phones/format?number=060550987&phone_code=33 |

### Helpers

```
formatNumber($number, $phone_code = null);

ex. formatNumber('06 78 909 876', '33')   returns +33 67 890 9876

stripNumber($number, $phone_code = null);

ex. stripNumber('06 78 909 876', '33') return 33678909876

if the argument $phone_code is not passed to the helpers, the used dialling code would be taken from

config('world.default_phone_code') 
```

### Localization  

The available locales are ar, br, de, en, es, fr, ja, kr, pl, pt, ro, ru and zh.  
The default locale is en.  
Include in the request header  
```
accept-language=locale
```

### Schema

<p><img src="./schema.jpg" width="600px"/></p>

### Testing

``` bash
composer test
```

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

`* optional`