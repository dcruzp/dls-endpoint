# Register User API

This task is with the goal for register a new user and link that user with a Contact 

### Register a new ***User*** using Identity Server 

- Register a new user. For that use the endpoint *register*, now the endpoint receive a body with the fields **email** and **password**. That should change for obtain more information when attempt to register a new user. Below are the specifiaction how must be the behaviour.

    - ```POST {host:port}/identity/register```
    - BODY request specifiaction
        ```
        user:   [string] 
        email:  [string] 
        pass:   [string] 
        phone:  [string]  
        ``` 
    - Response  
        ```
        201    Created()
        400    Unhautorize() 
        ```
    
- Create a new endpoint to get all contacts from database in a paginated and simplified way. The specification of this new endpoint are descibe below. 

    - ```POST {host:port}/api/contacts/simplified```
    - QUERY request specifiaction
        ```
        type:   [string] 
        name:   [string]    specifiaction -> FistName + MiddleName + LastName 
        ```
        - NOTE: here the name must be the contact of fields FirstName, MiddleName, LastName, that are fields of Contact entity. 
        *Contain* (*Join*(filter.FirstName,filter.LastName,filter.MiddleName), *Join*(db.FirstName,db.MiddleName,db.LastName)) 
    - 
        