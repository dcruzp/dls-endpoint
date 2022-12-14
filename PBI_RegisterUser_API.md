# Register User API

This task is with the goal for register a new user and link that user with a Contact 

### Register a new ***User*** using Identity Server 

- Register a new user. For that use the endpoint *register*, now the endpoint receive a body with the fields **email** and **password**. That should change for obtain more information when attempt to register a new user. Below are the specifiaction how must be the behaviour.

    - ```POST {host:port}/identity/register```
    - BODY request specifiaction
        ```
        {
            user:   [string] 
            email:  [string] 
            pass:   [string] 
            phone:  [string]  
        }
        ``` 
    - Response  
        ```
        201    Created()
        400    Unhautorize() 
        ```
    
- Create a new endpoint to get all contacts from database in a paginated and simplified way. The specification of this new endpoint are descibe below. 

    - ```GET {host:port}/api/contacts/simplified```
    - FILTER request specifiaction
        ```
        {
            type:   [string] 
            name:   [string]    (FistName + MiddleName + LastName) 
        }
        ```
        - NOTE: here the field name of ***filter*** must be the concatenation of the FirstName, MiddleName and LastName of contact that should be contained for the concatenation of Contact fields: FirstName, MiddleName, LastName, that are fields of Contact entity ***db_contact***. 
        *Contain* (*Join*(db_contact.FirstName,db_contact.LastName,db_contact.MiddleName), filter.name) 
    - Response
        ```
        200   Ok()
        400   BadRequet() 
        ```
        If status code response was BadRequest() ***400*** then a error ocurred while attemping get data. Otherwise if status code response was Ok() ***200***. Then the response body could be a paginated entity. 
        
        - Response structure
            ```
            {
                pageNumber:           [integer]
                pageSize:             [integer]
                firstPage:            [string][url]
                lastPage:             [string][url]
                totalPages:           [integer]
                totalRecords:         [integer]
                nextPage:             [string][url]
                previousPage:         [string][url]
                data:                 [array](here is where come all data)
                succeeded:            [boolean]
                errors:               [string]
                message:              [string] 
            }
            ```
            Inside data filed must come a array of ***contacts*** that must have the structure  described below. 
            ```
            {
                id:         [guid]
                name:       [string]
                type:       [string]
                email:      [string]
            }
            ```
            - [NOTES]
                1. *id* is the Id of Contact 
                1. Here the *name* field is the contatenation of fields FirstName + MiddleName + LastName. That have the entity *Contact* 
                2. *type* is the field name that have the entity EntityMetadata inside the entity Contact.
                3. *email* is the field Email of the first entity from entity ContactEmail that match the query descibed below:
                    from ContactEmails field , that is a list of entities of type ContactEmail, all entities that in her ContactEmailPurposes list have at leat one where her purpose is ***Main*** (with type generic: '7CBDB17B-931F-4259-842E-73A6DD4710F3'). 
                    - Pseudocode
                    ```
                    ContactEmail.First(x => x.ContactEmailPurpose.Contain(y => y.EmailPurposeId  == '7CBDB17B-931F-4259-842E-73A6DD4710F3'))
                    ```     

- Assing a Contact to a user. For that we need the user's id and contact. So should check if a endpoint exist for this functionality, and  if it does not exist, create a new endpoint for that. The endpoint specification will descibed below: 

    - ```POST  {host:port}/api/{id}/link{contact_id}```
        - [NOTES]
            - *id* field in the route is a guid instance that represent the user's id 
            - *contact_id* field in the route is a guid that represent the contact's id
    - Response
        ```
        200   Ok()           - the contact was assigned to user
        401   Unhautorize()  - unhautorize user 
        400   BadRequest()   - something was  wrong while attempted link user with contact   
        ``` 
    - here should check first if user and contact exist with the given ids, and notified if it does not exist. 
    


### Link User Contact Process 

1. Create a new User if it does't exist. You can create  anew user doing a request to endpoint ```POST  {host:port}/identity/register``` with body described in the specifications above. 

2. Get the contact's is with you want to link the user. you can use the endpoint  ```GET {host:port}/api/contacts/simplified```, that will give you the contact's id thah you need to link with a user. 

3. User the endpoint ```POST  {host:port}/api/{id}/link{contact_id}``` descrived in 3rd point  above to link the user with contact. If all is ok then user and contact will be linked. 




        