# UserManage Endpoint

### GET UserManager
```
{
    string Name
    string UserName
    string Email
    string UserType
    bool? Enable
    string Phone
    DateTime CreatedDate
    string Roles          -> all roles assigned to given user
}[]
```

### GET UserManager/{id}
```
{
    string Name
    string UserName
    string Email
    string userType
    bool? Enable
    string Phone
    DateTime CreatedDate
    string claims         -> all claims assigned to given user 
    string Roles          -> all roles assigned to given user
}
```

### PUT UserManager/{id}
```
{
    string claims  -> all claims assigned to a given user  [key:value]
    string roles   -> all roles assigned to a given user  [value]
}
```

### GET UserManager/roles
```
{
    string id 
    string name 
    string NormalizedName
    string ConcurrencyStamp
}[]
```

### POST UserManager/roles
##### input body
```
{
    string name 
    string NormalizedName
    string ConcurrencyStamp
}
```

### DELETE UserManager/roles/{id}
##### input query
```
    id -> the id of role to delete 
```
### POST UserManage
   - undefine 

### GET UserManager/roles/{id}/claims
##### input query 
```
id -> the id of role
``` 
```
{
    string Id 
    string ClaimType 
    string ClaimValue
}[]
```

### POST UserManager/roles/{id}/claims
##### input query 
```
id -> the id of role
``` 
##### input body
```
{
    string ClaimType 
    string ClaimValue
}
```

### DELETE UserManager/roles/claims/{id}
###### input query
```
id -> the id of claim of the role 
```


### GET UserManager/contact

```
{
    string name
    string login 
    string phone 
    string email 
    DateTime createDate 
}[]
```

### GET UserManager/contact/agents
```
{
    Guid id 
    Guid ContactId 
    Guid agentId 
    Guid resourceId
    DateTime createDate 
    string createBy  
}[]
```

### POST UserManage/assign/user/contact
###### input body 
```
{
    string userId 
    Guid contactId
}
```


### POST UserManage/assing/contact/agent
###### input body 
```
{
    Guid contactId 
    Guid agentId 
    Guid resourceId 
}
```