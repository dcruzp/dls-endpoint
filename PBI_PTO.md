# PBI PTO

### Endpoints 

- ### ``` GET PTO/Holidays``` 

    This endpoint return a list of all holidays. You can filter that results applying a filter to query. 

    - Query Specifiaction 
        ```
        fromdate      [datetime]      
        todate        [datetime]
        name          [string]
        pagenumber    [integer]
        pagesize      [integer]
        ```
        [NOTES] 
        ```fromdate``` is the date to compare all the dates of the ```Holidays.date``` filed where all are greather or equal than the ```fromdate``` field that come form filter.

        ```todate``` is the date to compare all the dates of the ```Holidays.date```  field where all are less or aqual than the ```todate``` field that come from filter. 

        ```name``` is the name of the Holiday that could by comparer with ```Holiday.name``` , the comparer could be a a **contain** funtion, in the way that if Holiday.name.Contain(name) then the filter function should return ***true***

        ```pagenumber``` is the page number the should be returned  when paginated the resultd . this field is a integer and should be greather than zero. 

        ```pagesize``` is the amount of records that should be returned in the paginated result. this should be a integer greather than zero    


    - Body Response    

        The body response is paginated, and the data that comes inside the paginates response, in the field data (that is a array) have the next structure:

        ```
        date:         [datetime] 
        name          [name] 
        ```

        [NOTE] 
        the ```date``` field is a ```datetime``` that is the date when is the holiday celebrate, and the ```name``` field is a string that represent the name by witch is known.  

       
- ```POST PTO/Holidays```
    This endpoint insert a new Holiday in the table ```Holidays```. The body of this endpoint have the next structure: 

    - Body Structure
        ```
        date          [datetime]
        name          [string]
        ```

        [Note] 
            ```date``` field should have the structure of a datetime type, and ``name`` field is a string that shouldn't be more longer than 50 charactres. 

