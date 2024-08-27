# Get session when using open app

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db
database "Redis" as redis

user -> g4: **<GetSession>**
activate g4
g4 -> g4: Get installId from context
g4 -> redis: Get session by installId
alt session in redis exits
  g4 --> user: return session info
end


g4 -> db: Get session in database
g4 -> g4: Upsert field **MemberType** , **CheckinAt**
note left: **MemberType** = 1 and \n**CheckinAt** = "0001-01-01T00:00:00+00:00"
alt session exits
  g4 -> db: Update new session
else request not found
  g4 -> db: Create new session
end

g4 -> redis: Set session in redis
g4 --> user: return session info
deactivate g4
```