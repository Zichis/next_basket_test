<!-- ABOUT THE PROJECT -->
## About The Project
This project is a Technical Assessment for Next Basket company for Senior Backend Role. There are two microservices (User and Notification) communicating with each other via message bus.



### Built With
* [Symfony](symfony.com)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

Follow the instructions to setup project.

### Installation
1. Clone the project. Two separate directories will be created for user and notification microservices.
    ```sh
    git clone https://github.com/Zichis/next_basket_test.git
    git submodule init 
    git submodule update
    ```

2. Install dependencies on the two services. You can use different terminals.
    ```sh
        cd users_service
        composer install
        cp .env.dist .env
    ```
    ```sh
        cd notifications_service
        composer install
        cp .env.dist .env
    ```

3. Run docker-compose
    ```sh
    docker-compose up --build
    ```
    If build is successful, you can access the services on your browser. localhost:8020 for users service and localhost:8030 for notifications

### Database Configuration
1. You have to get into the shell of the users service and setup database
    ```sh
    docker exec -it users_service bash
    php bin/console doctrine:database:create
    php bin/console make:migration
    php bin/console doctrine:migrations:migrate
    ```
    Your database should be setup by now.

### Test with Postman
Send a POST request to http://localhost:8020/users. With body
```
    {
        "email":"dbeckham@company.com",
        "firstName":"David",
        "lastName":"Beckham"
    }
```
Get into notification container
```sh
docker exec -it notifications_service bash
php bin/console messenger:consume -vv external_messages
```
User will be created and stored in the database of users_service. Message will be logged in notifications when it has been consumed. The message broker used is rabbitmq. You can access it on the browser http://localhost:15673. Username is "user" and Password is "password".

### Testing
```sh
docker exec -it users_service bash
php bin/console --env=test doctrine:database:create
php bin/console --env=test doctrine:shema:create
vendor/bin/phpunit
```