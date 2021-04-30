# API cars

API with cars characteristics

## Run

1. Download docker desktop for your OS

   ```
   https://www.docker.com/products/docker-desktop
   ```

1. Run command from root of this project in terminal

   ```bash
   docker-compose up -d
   ```

_If you get an error **Filesharing has been cancelled** on Windows – you need to add project directory to Resources -> File Sharing in Docker Desktop. (see: https://stackoverflow.com/questions/60754297/docker-compose-failed-to-build-filesharing-has-been-cancelled)_

1. You can check running containers with command

   ```bash
   docker ps
   ```   
1. API located at
   
    ```
    http://localhost:8084
    ```
   
1. To stop all services use

   ```bash
   docker-compose stop
   ```         

# Task description

There is poorly designed API with next endpoints (how to run see above):
* **GET** /api/v2/cars
 ```json
[
  {
    "id": 27,
    "segment": "C-segment",
    "brand": "Volkswagen",
    "model": "Golf",
    "generation": "Mk8",
    "modification": "1.5 TSI"
  },
  {
    "id": 28,
    "segment": "C-segment",
    "brand": "Volkswagen",
    "model": "Golf",
    "generation": "Mk7",
    "modification": "2.0 TDI 4Motion"
  },
  ...
]
```
*  **GET** /api/v2/cars/{carId}
```json
{
  "id": 27,
  "segment": "C-segment",
  "brand": "Volkswagen",
  "model": "Golf",
  "generation": "Mk8",
  "modification": "1.5 TSI",
  "year_range": "2019-present",
  "engine_type": "GASOLINE",
  "engine_cylinders": "L4",
  "engine_displacement": 1498,
  "engine_horsepower": 150,
  "gearbox": "ROBOTIC",
  "wheel_drive": "FWD",
  "body_length": 4284,
  "body_width": 1789,
  "body_height": 1491,
  "body_style": "Hatchback",
  "acceleration": 8.5,
  "max_speed": 224
}
```
*  **GET** /api/v2/countries
```json
[
  {
    "id": 1,
    "title": "Czech Republic",
    "brands": [
      "Skoda"
    ]
  },
  {
    "id": 2,
    "title": "England",
    "brands": [
      "Bentley",
      "Jaguar"
    ]
  },
...
]
```

Let engine characteristics be:
* engine_type
* engine_cylinders
* engine_displacement
* engine_horsepower,

And let body characteristics be:
* body_length
* body_width
* body_height
* body_style

You need to develop RESTful application on Java (using this project as template) which:
1. Parses all data from **API cars characteristics** at the start and stores all this data in memory in some structure. It should be guaranteed that only one instance of that structure exists. For bonus points you can work with endpoints which ends on ```/paged``` (these endpoints use Spring Pageable https://docs.spring.io/spring-data/rest/docs/2.0.0.M1/reference/html/paging-chapter.html).
1. Presents endpoint to show array of cars in JSON format.
    ```
    GET /api/cars
    ```
    All car objects should include next fields in response: 
    * car identifier (**id**)
    * segment name (**segment**)
    * brand name (**brand**)
    * model name (**model**)
    * production country (**country**)
    * name of model generation (**generation**)
    * modification name (**modification**).

    The request may contain the following parameters (if the parameter is not correct, the application should not crash with 500 code; correct response should be returned, for example HTTP CODE 404 showing that nothing was found):
    1. **brand**: if it is specified then only cars produced under the specified brand should be present in response.
    1. **segment**: if it is specified then only cars belonging to the specified segment should be displayed.
    1. **minEngineDisplacement**: if it is specified then only cars which have engine displacement value greater than or equal to the specified value should be displayed (in case of an incorrect parameter, for example if it is not a number, issue HTTP CODE 400).
    1. **minEngineHorsepower**: if it is specified then only cars which engine power is greater than or equal to the specified value should be displayed (in case of an incorrect parameter, for example if it is not a number, issue HTTP CODE 400).
    1. **minMaxSpeed**: if it is specified then only cars which maximum speed is greater than or equal to the specified value should be displayed (in case of an incorrect parameter, for example if it is not a number, issue HTTP CODE 400).
    1. **search**: if it is specified then the search is performed on the following fields: model name, generation and modification name.
    1. **isFull**: this parameter accepts true/false as a value. If it is true then full information about the car should be displayed: the engine and body characteristics must be added to separate ** engine ** and ** body ** objects.
    1. _(bonus points)_ **year**: if it is specified then only cars that were produced in the specified year are displayed (in case of an incorrect parameter, for example if it is not a number, issue HTTP CODE 400).
    1. _(bonus points)_ **bodyStyle**: if it is specified then only cars with this body type are displayed.
    
    _Filtering by all parameters at the same time is not mandatory, but will be considered as bonus points._
    
    Request example:
    ```
    http://localhost:8081/api/cars?brand=Volkswagen&segment=E-segment&minEngineDisplacement=4.0&minEngineHorsepower=250&minMaxSpeed=200&search=5
    ```
    
1. Presents endpoints with the following dictionaries, ** based only on the data obtained from the external API **:
   * all possible types of fuel
   ```
   GET /api/fuel-types
   ```
   * all possible types of body
   ```
   GET /api/body-styles
   ```
   * all possible types of engine
   ```
   GET /api/engine-types
   ```
   * all possible types of wheel drives
   ```
   GET /api/wheel-drives
   ```
   * all possible types of gearbox
   ```
   GET /api/gearboxes
   ```   
1. Presents endpoint which returns the value of ** average maximum speed ** by brand/model (parameters in the ** brand **/** model ** query). If there is no brand or model specified, issue HTTP CODE 404. If both parameters are specified simultaneously, issue HTTP CODE 400.
   ```
   GET /api/max-speed
   ```
   Request example:
   ```
   http://localhost:8081/api/max-speed?brand=BMW
   ```
1. Unit tests for application will be considered as bonus points.

For verification the project must be published in public on some platform (github, gitlab, etc.).