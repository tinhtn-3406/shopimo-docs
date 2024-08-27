# Create installId when using app first time

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db
database "Redis" as redis

user -> g4: **<Install>**
activate g4
g4 -> db: Get **AuthToken** based on **retail_code**

alt authToken not exits
  g4 --> user: return error **ErrorCode_INVALID_AUTH_TOKEN**
end


g4 -> db: Create **Install**
g4 -> db: Create **BearerToken**
g4 -> db: Create **Session**
g4 -> redis: **Del** session in redis
g4 --> user: return **InstallId**, **BearerToken**
deactivate g4
```
