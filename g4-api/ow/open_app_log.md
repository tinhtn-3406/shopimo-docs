# G4 APIs App

## Upsert lasted use app

```plantuml

actor "User" as user #SaddleBrown


entity "G4 Service" as g4 #lightGreen
database "DB" as db
database "Dynamo DB" as dynamo #B4A7E5


user -> g4: [Open app] **/api/open-app**
activate g4
g4 -> g4: Check member have exit
alt member not exits
  g4 --> user: return Status code 404
end


g4 -> g4: Check lasted use app
g4 -> db: Upsert field **last_used** in table **members** or new other table
g4 -> dynamo: Create log

g4 --> user: return Status code 200
deactivate g4
```

## How to using a Digital Card or a physical card 

```plantuml

actor "User" as user #SaddleBrown


entity "G4 Service" as g4 #lightGreen
database "DB" as db
database "Dynamo DB" as dynamo #B4A7E5


user -> g4: Payment POS
activate g4
g4 -> g4: Check member have exit
alt member not exits
  g4 --> user: return Status code 404
end


g4 -> g4: Check lasted use app
g4 -> db: Upsert field **last_used** in table **members** or new other table
g4 -> dynamo: Create log

g4 --> user: return Status code 200
deactivate g4
```