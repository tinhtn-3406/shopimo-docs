# G4 APIs App

## Get session when using open app

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db
database "Redis" as redis

user -> g4: **<GetSession>** [/api/session]
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

## Create installId when using app first time

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db
database "Redis" as redis

user -> g4: **<Install>** [/api/install]
activate g4
g4 -> db: Get **AuthToken** based on **retail_code**

g4 -> g4: Check **AuthToken**
activate g4 #DarkSalmon
alt authToken not exits
  g4 --> user: return error **ErrorCode_INVALID_AUTH_TOKEN**
end
deactivate g4

g4 -> db: Create **Install**
g4 -> db: Create **BearerToken**
g4 -> db: Create **Session**
g4 -> redis: **Del** session in redis
g4 --> user: return **InstallId**, **BearerToken**
deactivate g4
```

## Register member and send mfa PinCode

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
participant "SMS" as sms  << (S,#ADD1B2) www.sms-ope.com >>  order 1
database "DB" as db

user -> g4: **<EntryMemberPrecheck>** 
activate g4
g4 -> g4: Validate input
activate g4 #DarkSalmon
alt validation error
  g4 --> user: return **Error**
end

deactivate g4

g4 -> g4: **<SendMfaPinCode>**:  Create **token**, **pinCode**

activate g4 #DarkSalmon
g4 -> sms: Send PinCode

g4 -> db: Save **MfaSession** in database
g4 --> user: return **MfaToken**, **Expire**
deactivate g4

deactivate g4
```

## Verify mfa PinCode

- User already received mfa PinCode

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db

user -> g4: **<VerifyMfaPinCode>**
g4 -> db: Get info **MfaSession** based on **installId**, \n**MfaToken** and **expire_at**
activate g4
g4 -> g4: Check MfaSession have exists
activate g4 #DarkSalmon
alt MfaSession not exists
  g4 -> db: Increase **error_count** in table **MfaToken**
  g4 --> user: return **pb.ErrorCode_INVALID_PARAMETER**
end

deactivate g4

g4 -> g4: Check **ErrorCount** > 5

activate g4 #DarkSalmon
alt ErrorCount greater than or equal to 5
  g4 --> user: return **pb.ErrorCode_INVALID_PARAMETER**
end
deactivate g4

g4 -> db: Save info in **Session**
note right
Data
MemberId
IsAuthMfa = true
end note

g4 --> user: Response **Empty{}**

deactivate g4
```

## Verify mfa PinCode

- User already received mfa PinCode

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db

user -> g4: **<VerifyMfaPinCode>**
g4 -> db: Get info **MfaSession** based on **installId**, \n**MfaToken** and **expire_at**
activate g4
g4 -> g4: Check MfaSession have exists
activate g4 #DarkSalmon
alt MfaSession not exists
  g4 -> db: Increase **error_count** in table **MfaToken**
  g4 --> user: return **pb.ErrorCode_INVALID_PARAMETER**
end

deactivate g4

g4 -> g4: Check **ErrorCount** > 5

activate g4 #DarkSalmon
alt ErrorCount greater than or equal to 5
  g4 --> user: return **pb.ErrorCode_INVALID_PARAMETER**
end
deactivate g4

g4 -> db: Save info in **Session**
note right
Data
MemberId
IsAuthMfa = true
end note

g4 --> user: Response **Empty{}**

deactivate g4
```

## Link member card

- Member already `Verify mfa PinCode` success

```plantuml
actor "User" as user #SaddleBrown
entity "G4 Service" as g4 #lightGreen
database "DB" as db

user -> g4: **<VerifyMfaPinCode>**
g4 -> db: Get info **MfaSession** based on **installId**, \n**MfaToken** and **expire_at**
activate g4
g4 -> g4: Check MfaSession have exists
activate g4 #DarkSalmon
alt MfaSession not exists
  g4 -> db: Increase **error_count** in table **MfaToken**
  g4 --> user: return **pb.ErrorCode_INVALID_PARAMETER**
end

deactivate g4

g4 -> g4: Check **ErrorCount** > 5

activate g4 #DarkSalmon
alt ErrorCount greater than or equal to 5
  g4 --> user: return **pb.ErrorCode_INVALID_PARAMETER**
end
deactivate g4

g4 -> db: Save info in **Session**
note right
Data
MemberId
IsAuthMfa = true
end note

g4 --> user: Response **Empty{}**

deactivate g4
```
