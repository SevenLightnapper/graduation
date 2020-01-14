# Voting system for restaurant
(Graduation project Topjava)

Design and implement a REST API using Hibernate/Spring/SpringMVC (or Spring-Boot) without frontend.

Build a voting system for deciding where to have lunch.

- 2 types of users: admin and regular users
- Admin can input a restaurant and it's lunch menu of the day (2-5 items usually, just a dish name and price)
- Menu changes each day (admins do the updates)
- Users can vote on which restaurant they want to have lunch at
- Only one vote counted per user
- If user votes again the same day:
    - If it is before 11:00 we assume that he changed his mind.
    - If it is after 11:00 then it is too late, vote can't be changed

Each restaurant provides new menu each day.

## Used Tools and Technologies
* Maven
* SpringBoot
* Spring Fata JPA
* HSQLDB
* Hibernate
* Java 8

## Rest API 
### User
Admin Actions:
- GET /rest/admin/users - get a list of all users
- GET /rest/admin/users/{userId} - get user profile by id
- GET /rest/admin/users/by?email={email} - get user profile by email
- POST /rest/admin/users - create user profile
- PUT /rest/admin/{userId} - update user profile by id
- DELETE /rest/admin/{userId} - delete user profile by id

User Actions:
- GET /rest/profile - get your profile
- UPDATE /rest/profile - update your profile
- DELETE /rest/profile - delete your profile

New User Action:
- POST /rest/profile - register as a new user

### Restaurant
Admin Actions:
- GET /rest/admin/restaurants - get a list of all restaurants
- POST /rest/admin/restaurants - create a profile of the restaurant
- PUT /rest/admin/restaurants/{restaurantId} - update restaurant profile by id
- DELETE /rest/admin/restaurants/{restaurantId} - delete restaurant profile by id

User Actions:
- GET /rest/restaurants - get a list of all restaurants with menu for today
- GET /rest/restaurants/{restaurantId} - get a restaurant menu for today by id
- GET /rest/restaurants/{restaurantId}?date={date} - get the restaurant menu by id on the specified date
- GET /rest/restaurants?date={date} - get the menu of all restaurants on the specified date

### Menu
Admin Actions:
- GET /rest/admin/restaurants/{restaurantId}/dishes - get a list of all restaurant dishes by restaurant id
- GET /rest/admin/restaurants/{restaurantId}/dishes/{dishId} - get dish by id
- POST /rest/admin/restaurants/{restaurantId}dishes - create a dish in the restaurant menu
- PUT /rest/admin/restaurants/{restaurantId}/dishes/{dishId} - update a dish in the restaurant menu
- DELETE /rest/admin/restaurants/{restaurantId}/dishes/{dishId} - delete dish by id

### Vote
Admin Action:
- GET /rest/admin/users/votes - get a list of all the votes for today

User Actions:
- POST /rest/votes - vote for a restaurant by id
- PUT /rest/votes - vote again for a restaurant by id
- GET /rest/votes - get your vote for today
- GET /rest/votes/filter?startDate={startDate}&endDate={endDate} -  get a list of your votes for the specified period

## Curl Command Lines 
## Admin:

#### get All Users
`curl -s http://localhost:8080/graduation/rest/admin/users --user admin@mail.ru:password`

#### get User 100001
`curl -s http://localhost:8080/graduation/rest/admin/users/100001 --user admin@mail.ru:password`

#### get User by email user@mail.ru
`curl -s http://localhost:8080/graduation/rest/admin/users/by?email=user@mail.ru --user admin@mail.ru:password`

#### create User
`curl -s -X POST -d '{"name":"NewUser12", "email":"user12@mail.ru", "password":"password", "roles":["ROLE_USER"]}' -H 'Content-Type:application/json;charset=UTF-8' http://localhost:8080/graduation/rest/admin/users --user admin@mail.ru:password`

#### delete User 100002
`curl -s -X DELETE http://localhost:8080/graduation/rest/admin/users/100002 --user admin@mail.ru:password`

#### update User 100000
`curl -s -X PUT -d '{"name":"NewUser2", "email":"user2@mail.ru", "password":"password2"}' -H 'Content-Type:application/json;charset=UTF-8' http://localhost:8080/graduation/rest/admin/users/100000 --user admin@mail.ru:password`

## User:

#### get profile
`curl -s http://localhost:8080/graduation/rest/profile --user user@mail.ru:password`

#### update profile
`curl -s -X PUT -d '{"name":"NewUser2", "email":"user2@mail.ru", "password":"password2"}' -H 'Content-Type:application/json;charset=UTF-8' http://localhost:8080/graduation/rest/profile --user user@mail.ru:password`

#### delete profile
`curl -s -X DELETE http://localhost:8080/graduation/rest/profile --user user12@mail.ru:password`

## Restaurant Admin:

#### get Restaurants
`curl -s http://localhost:8080/graduation/rest/admin/restaurants --user admin@mail.ru:password`

#### create Restaurant
`curl -s -X POST -d '{"name":"New Restaurant"}' -H 'Content-Type:application/json;charset=UTF-8' http://localhost:8080/graduation/rest/admin/restaurants --user admin@mail.ru:password`

#### update Restaurant 100001
`curl -s -X PUT -d '{"name":"Restaurant1_update"}' -H 'Content-Type: application/json' http://localhost:8080/graduation/rest/admin/restaurants/100001 --user admin@mail.ru:password`

#### delete Restaurant 100002
`curl -s -X DELETE http://localhost:8080/graduation/rest/admin/restaurants/100002 --user admin@mail.ru:password`

## Restaurant User:

#### get all Restaurants for today
`curl -s http://localhost:8080/graduation/rest/restaurants --user user@mail.ru:password`

#### get Restaurant 100001 for today
`curl -s http://localhost:8080/graduation/rest/restaurants/100001 --user user@mail.ru:password`

#### get Restaurant 100002 with dish for date 2019-12-12
`curl -s http://localhost:8080/graduation/rest/restaurants/100002?date=2019-12-12 --user user@mail.ru:password`

#### get All Restaurants with dish for date 2019-11-11
`curl -s http://localhost:8080/graduation/rest/restaurants?date=2019-11-11 --user user@mail.ru:password`

## Dish Admin:

#### get All Dish for restaurant 100001
`curl -s http://localhost:8080/graduation/rest/admin/restaurants/100001/dishes --user admin@mail.ru:password`

#### get Dish 100007
`curl -s http://localhost:8080/graduation/rest/admin/restaurants/100003/dishes/100007 --user admin@mail.ru:password`

#### create Dish
`curl -s -X POST -d '{"name":"New dish","price":100,"date":"2020-01-08"}' -H 'Content-Type:application/json;charset=UTF-8' http://localhost:8080/graduation/rest/admin/restaurants/100002/dishes --user admin@mail.ru:password`

#### update Dish 100003
`curl -s -X PUT -d '{"name":"Updated_DISH3","price":200,"date":"2020-01-08"}' -H 'Content-Type: application/json' http://localhost:8080/graduation/rest/admin/restaurants/100001/dishes/100003 --user admin@mail.ru:password`

#### delete Dish 100006
`curl -s -X DELETE http://localhost:8080/graduation/rest/admin/restaurants/100003/dishes/100006 --user admin@mail.ru:password`

## Vote Admin:

#### get all Votes for today
`curl -s http://localhost:8080/graduation/rest/admin/users/votes/ --user admin@mail.ru:password`

## Vote User:

#### Vote for Restaurant 100003
`curl -s -X POST -d '{"restaurantId":"100003"}' -H "Content-Type:application/json;charset=UTF-8" http://localhost:8080/graduation/rest/votes --user user@mail.ru:password`

#### Change Vote for Restaurant 100004
`curl -s -X PUT -d '{"restaurantId":"100004"}' -H "Content-Type:application/json;charset=UTF-8" http://localhost:8080/graduation/rest/votes/100001 --user user@mail.ru:password`

#### get Vote for today
`curl -s http://localhost:8080/graduation/rest/votes/ --user user@mail.ru:password`

#### filter Votes
`curl -s "http://localhost:8080/graduation/rest/votes/filter?startDate=2019-12-12&endDate=2020-01-14" --user user@mail.ru:password`