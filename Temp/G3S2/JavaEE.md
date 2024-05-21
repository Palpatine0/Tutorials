# Zillow

## Sequence Diagram

#### Zillow-File
```plantuml
@startuml
actor Client as "Client Browser"

participant "FileController" as Controller
participant "FileServiceImpl" as Service
participant "FileDaoImpl" as Dao
participant "FastFileStorageClient" as StorageClient
database "MongoDB" as DB

Client -> Controller : request("/file/getBanner")
activate Controller
    Controller -> Service : getBanner()
    activate Service
        Service -> Dao : selectBanner(query)
        activate Dao
            Dao -> DB : Query MongoDB
            DB -> Dao : Return banners
        deactivate Dao
        Dao -> Service : List<Banner>
    deactivate Service
    Service -> Controller : Return ZillowResult with banners
deactivate Controller
Controller -> Client : Return ZillowResult with banners

Client -> Controller : request("/file/uploadImage")
activate Controller
    Controller -> Service : uploadImage(fileBytes, fileName)
    activate Service
        Service -> StorageClient : uploadFile(inputStream, size, fileSuffix, null)
        activate StorageClient
            StorageClient -> Service : Return StorePath
        deactivate StorageClient
        Service -> Dao : saveHouseImage(HouseImage)
        activate Dao
            Dao -> DB : Save HouseImage
            DB -> Dao : Confirm save
        deactivate Dao
        Dao -> Service : Acknowledge save
    deactivate Service
    Service -> Controller : Return ZillowResult with imageURL
deactivate Controller
Controller -> Client : Return ZillowResult with imageURL

Client -> Controller : request("/file/uploadImageNoPrefix")
activate Controller
    Controller -> Service : uploadImageNoPrefix(fileBytes, fileName)
    activate Service
        Service -> StorageClient : uploadFile(inputStream, size, fileSuffix, null)
 
            StorageClient -> Service : Return StorePath
 
        Service -> Controller : Return ZillowResult with fullPath
    deactivate Service
deactivate Controller
Controller -> Client : Return ZillowResult with fullPath

Client -> Controller : request("/file/delete")
activate Controller
    Controller -> Service : delete(filePath)
    activate Service
        Service -> StorageClient : deleteFile(filePath)
            StorageClient -> Service : Confirm delete
        Service -> Controller : Return ZillowResult ok
    deactivate Service
deactivate Controller
Controller -> Client : Return ZillowResult ok

note right of DB : MongoDB handles all data storage\n for Banners and House Images.
note right of StorageClient : FastDFS client handles file storage\n on a distributed file system.

@enduml
```



#### Zillow-Trendy
```plantuml
@startuml
actor Client as "Client Browser"

participant "TrendyController" as Controller
participant "TrendyServiceImpl" as Service
participant "TrendyDaoImpl" as Dao
database "MongoDB" as DB

Client -> Controller : request("/trendy/getTrendy")
activate Controller
    Controller -> Service : getTrendy(city)
    activate Service
        Service -> Dao : getTrendy(query)
        activate Dao
            Dao -> DB : Query MongoDB
            DB -> Dao : Return trendy items
        deactivate Dao
        Service -> Controller : Return ZillowResult with trendy items
    deactivate Service
deactivate Controller
Controller -> Client : Return ZillowResult with trendy items
@enduml
```

#### Zillow-Search
```plantuml
@startuml
actor Client as "Client Browser"

participant "SearchController" as Controller
participant "SearchServiceImpl" as Service
participant "SearchDaoImpl" as Dao
database "Elasticsearch" as ES

Client -> Controller : request("/search/searchByCity")
activate Controller
    Controller -> Service : searchByCity(city, page, rows)
    activate Service
        Service -> Dao : searchByCity(city, page, rows)
        activate Dao
            Dao -> ES : Query Elasticsearch
            ES -> Dao : Return search results
        deactivate Dao
        Service -> Controller : Return ZillowResult with search results
    deactivate Service
deactivate Controller
Controller -> Client : Return ZillowResult with search results

Client -> Controller : request("/search/searchByKeyWord")
activate Controller
    Controller -> Service : searchByKeyWord(city, content, page, rows)
    activate Service
        Service -> Dao : searchByKeyWord(city, content, page, rows)
        activate Dao
            Dao -> ES : Query Elasticsearch
            ES -> Dao : Return search results
        deactivate Dao
        Service -> Controller : Return ZillowResult with search results
    deactivate Service
deactivate Controller
Controller -> Client : Return ZillowResult with search results

Client -> Controller : request("/search/ESReload")
activate Controller
    Controller -> Service : esReload()
    activate Service
        Service -> Dao : ESInit()
        activate Dao
            Dao -> MongoDB : Query MongoDB
            MongoDB -> Dao : Return item list
            Dao -> ES : Batch insert to ES
        deactivate Dao
        Service -> Client : Return ZillowResult "Done"
    deactivate Service
deactivate Controller
Controller -> Client : Return ZillowResult "Done"

@enduml
```

#### Zillow-Item
```plantuml
@startuml
actor Client as "Client Browser"

participant "ItemController" as ItemController
participant "ItemServiceImpl" as ItemService
participant "ItemDaoImpl" as ItemDao
participant "OrderServiceFeignClient" as OrderServiceFeignClient
database "MongoDB" as MongoDB
database "Redis" as Redis

Client -> ItemController : request("/item/getItemByID")
activate ItemController
    ItemController -> ItemService : getItemByID(id)
    activate ItemService
        ItemService -> Redis : check cache for item
        Redis -> ItemService
        alt Cache Miss
            ItemService -> ItemDao : findItemById(id)
            activate ItemDao
                ItemDao -> MongoDB : find item by ID
                MongoDB -> ItemDao : return item details
            deactivate ItemDao
            ItemService -> Redis : cache item details
        end
        ItemService --> ItemController : return item details
    deactivate ItemService
ItemController --> Client
deactivate ItemController

Client -> ItemController : request("/item/deleteItemByID")
activate ItemController
    ItemController -> ItemService : deleteItemByID(id)
    activate ItemService
        ItemService -> ItemDao : deleteItemByID(id)
        activate ItemDao
            ItemDao -> MongoDB : delete item by ID
            MongoDB -> ItemDao : confirm deletion
        deactivate ItemDao
        ItemService -> Redis : evict item from cache
    deactivate ItemService
    ItemController --> Client : return deletion status
deactivate ItemController

Client -> ItemController : request("/item/addItem")
activate ItemController
    ItemController -> ItemService : addItem(item details)
    activate ItemService
        ItemService -> ItemDao : saveItem(item details)
        activate ItemDao
            ItemDao -> MongoDB : save new item
            MongoDB -> ItemDao : confirm save
        deactivate ItemDao
    deactivate ItemService
    ItemController --> Client : return addition status
deactivate ItemController

Client -> ItemController : request("/item/updateItemStatusById")
activate ItemController
    ItemController -> ItemService : updateItemStatusById(id, status)
    activate ItemService
        ItemService -> ItemDao : updateItemStatusById(id, status)
        activate ItemDao
            ItemDao -> MongoDB : update item status
            MongoDB -> ItemDao : confirm status update
        deactivate ItemDao
        ItemService -> Redis : evict item from cache
    deactivate ItemService
    ItemController --> Client : return update status
deactivate ItemController

Client -> ItemController : request("/item/getOrder")
activate ItemController
    ItemController -> OrderServiceFeignClient : getOrder(phone)
    activate OrderServiceFeignClient
        OrderServiceFeignClient --> ItemController : return orders
    deactivate OrderServiceFeignClient
ItemController --> Client
deactivate ItemController

@enduml
```


#### Zillow-Comment
```plantuml
@startuml
actor Client as "Client Browser"
participant "CommentController" as Controller
participant "CommentServiceImpl" as Service
participant "CommentDaoImpl" as Dao
participant "OrderDaoImpl" as OrderDao

Client -> Controller: request("/comment/addComment")
activate Controller
Controller -> Service: addComment(orderId, commentContent, phone)
activate Service
Service -> OrderDao: getOrders(orderId)
activate OrderDao
OrderDao --> Service: Return order
deactivate OrderDao
Service -> Dao: saveComment(comment)
activate Dao
Dao --> Service: Comment saved successfully
deactivate Dao
Service -> OrderDao: updateCommentStatus(orderId, 0)
activate OrderDao
OrderDao --> Service: Comment status updated successfully
deactivate OrderDao
Service --> Client: Return ZillowResult with success message
deactivate Service
deactivate Controller

Client -> Controller: request("/comment/getComment")
activate Controller
Controller -> Service: getCommentByItemID(itemId, page, rows)
activate Service
Service -> Dao: getCommentByItemId(query)
activate Dao
Dao --> Service: Return list of comments
deactivate Dao
Service --> Client: Return ZillowResult with comments
deactivate Service
deactivate Controller

@enduml
```


#### Zillow-User
```plantuml
@startuml
actor Client as "Client Browser"

participant "UserController" as UserController
participant "UserServiceImpl" as UserService
participant "UserDaoImpl" as UserDao
participant "VerificationDaoImpl" as VerificationDao
participant "AuthenticationService" as AuthenticationService
participant "DetailService" as DetailService
participant "SecurityConfig" as SecurityConfig
database "Redis" as Redis
database "MongoDB" as MongoDB

Client -> UserController : request("/user/register")
activate UserController
    UserController -> UserService : register(username, password, phone)
    activate UserService
        UserService -> UserDao : getUserByUsername(username)
        activate UserDao
            UserDao -> MongoDB : check username existence
            MongoDB -> UserDao : return user details
        deactivate UserDao
        UserService --> UserController : Return ZillowResult with registration status
    deactivate UserService
UserController --> Client
deactivate UserController

Client -> SecurityConfig : request("/user/login")
activate SecurityConfig
    SecurityConfig -> AuthenticationService : authenticate(username, password)
    activate AuthenticationService
        AuthenticationService -> DetailService : loadUserByUsername(username)
        activate DetailService
            DetailService -> UserDao : getUserByUsername(username)
            activate UserDao
                UserDao -> MongoDB : fetch user details
                MongoDB -> UserDao : return user details
            deactivate UserDao
        DetailService --> AuthenticationService
        deactivate DetailService
        AuthenticationService -> VerificationDao : getVerificationCode(phone)
        activate VerificationDao
            VerificationDao -> Redis : fetch verification code
            Redis -> VerificationDao
        deactivate VerificationDao
        AuthenticationService --> SecurityConfig : Return ZillowResult with login status
    deactivate AuthenticationService
SecurityConfig --> Client
deactivate SecurityConfig

Client -> UserController : request("/user/deleteUser")
activate UserController
    UserController -> UserService : deleteUser(id)
    activate UserService
        UserService -> UserDao : deleteUserById(id)
        activate UserDao
            UserDao -> MongoDB : delete user by ID
            MongoDB -> UserDao : confirm deletion
        deactivate UserDao
        UserService --> UserController : Return ZillowResult with deletion status
    deactivate UserService
UserController --> Client
deactivate UserController

Client -> UserController : request("/user/getUser")
activate UserController
    UserController -> UserService : getUser()
    activate UserService
        UserService -> UserDao : selectUsers(query)
        activate UserDao
            UserDao -> MongoDB : fetch all users
            MongoDB -> UserDao : return user list
        deactivate UserDao
        UserService --> UserController : Return ZillowResult with user data
    deactivate UserService
UserController --> Client
deactivate UserController

Client -> UserController : request("/user/sendVerificationCode")
activate UserController
    UserController -> UserService : sendVerificationCode(phone)
    activate UserService
        UserService -> VerificationDao : setVerificationCode(phone, code)
        activate VerificationDao
            VerificationDao -> Redis : store verification code
            Redis -> VerificationDao
        deactivate VerificationDao
        UserService --> UserController : Return ZillowResult with verification code status
    deactivate UserService
UserController --> Client
deactivate UserController

@enduml
```


#### Zillow-BuyAction
```plantuml
@startuml
actor Client as "Client Browser"

participant "BuyActionController" as BuyController
participant "BuyActionServiceImpl" as BuyService
participant "BuyActionDaoImpl" as BuyActionDao
database "Redis" as Redis
participant "StreamBridge" as Stream
participant "BuyActionMessageConsumer" as Consumer
participant "BuyActionMessageServiceImpl" as MessageService
participant "BMItemDaoImpl" as BMItemDao
participant "BMOrderDaoImplImpl" as BMOrderDao
database "MongoDB" as MongoDB

Client -> BuyController : buyAction(id, user)
activate BuyController
    BuyController -> BuyService : buyAction(id, user)
    activate BuyService
        BuyService -> BuyActionDao : getItem(key)
        activate BuyActionDao
            BuyActionDao -> Redis : fetch item details
            Redis -> BuyActionDao : item details
        deactivate BuyActionDao

        alt item is not rented
            BuyService -> Stream : send(zillowMessenger-out-0, message)
            Stream --> BuyService : message sent successfully
            BuyService --> BuyController : ZillowResult (success)
        else item is rented
            BuyService --> BuyController : ZillowResult (error)
        end
    deactivate BuyService
BuyController --> Client

Client -> Consumer : consume message
activate Consumer
    Consumer -> MessageService : buyAction(itemId, username)
    activate MessageService
        MessageService -> BMItemDao : updateItem(itemId, true)
        activate BMItemDao
            BMItemDao -> MongoDB : set item as rented
            MongoDB -> BMItemDao : update successful
        deactivate BMItemDao

        alt update successful
            MessageService -> BMOrderDao : saveOrder(order)
            activate BMOrderDao
                BMOrderDao -> MongoDB : save new order
                MongoDB -> BMOrderDao : save successful
            deactivate BMOrderDao
            MessageService --> Consumer : true (update successful)
        else
            MessageService --> Consumer : false (update failed)
        end
    deactivate MessageService
Consumer --> Client : log result

@enduml
```

#### Zillow-BuyAction
```plantuml
@startuml
actor Client as "Client Browser"

participant "OrderController" as OrderCtrl
participant "OrderServiceImpl" as OrderService
participant "OrderDaoImpl" as OrderDao
database "MongoDB" as MongoDB

Client -> OrderCtrl : getOrder(phone)
activate OrderCtrl
    OrderCtrl -> OrderService : getOrder(phone)
    activate OrderService
        OrderService -> OrderDao : getOrders(phone)
        activate OrderDao
            OrderDao -> MongoDB : find orders by phone
            MongoDB -> OrderDao : return list of orders
        deactivate OrderDao
        OrderService --> OrderCtrl : return list of orders
    deactivate OrderService
OrderCtrl --> Client
deactivate OrderCtrl

@enduml
```