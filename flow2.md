@startuml
autonumber

actor User
participant FrontEnd as FE
participant BackEnd as BE
participant SNS as SNS
participant Keycloak as KC

FE --> BE: Get sns client id
BE --> FE: Send sns client id
User -> FE: Click an SNS button on MyPage
FE -> User: Redirect to the SNS authorization endpoint
note left SNS
Request as authorization code flow
end note

User -> SNS: Open the authorization endpoint
SNS -> User: Show sign-in page
User -> SNS: Sign-in
SNS -> User: Redirect to NFC redirect_url with code
User -> FE: Open redirect_url with code
FE -> BE: Send the code to get token
BE -> SNS: Get access token with the code
SNS -> BE: Return tokens
note left SNS
* Access Token
* Refresh Token
* ID Token
end note

alt apple
  autonumber 13.1
  BE -> BE: Decode id token payload
  autonumber 14.1
  BE -> BE: Extract user info
else another SNS
  autonumber 13.2
  BE -> SNS: Get user info
  autonumber 14.2
  SNS -> BE: Return user info
end

autonumber 15
BE -> KC: Add a social login provider to the user
note right BE
* SNS alias
** google
** yahoo
** niconico
* Provider User ID
* Provider User Name
end note

KC -> BE: Return response
BE -> FE: Return response
FE -> User: Show result
@enduml
