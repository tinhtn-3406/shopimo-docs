# Register member and send PinCode

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db
database "Redis" as redis

user -> g4: **<EntryMemberPrecheck>**
activate g4
g4 -> g4: Validate input
activate g4 #DarkSalmon
alt validation error
  g4 --> user: **Error**
  destroy g4
end

deactivate g4

user ->g4: **<SendMfaPinCode>**
g4 -> g4: Create **token**, **pinCode**

activate g4 #DarkSalmon

deactivate g4


deactivate g4
```