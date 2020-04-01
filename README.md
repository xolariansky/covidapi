## covidapi

specifying a common specification for multiple covid tracking apps to include and leverage on the large user base and "unified" backend

### goals

* distribute covid-status
* protect against flooding / make idempotent  
* distribute verification message 
* grant access to message only if user is verified 

#### side goals

* support a decentralized server architecture

### concept

#### basic mode of operation

devices generate new JWE messages for their UUID and advertise those 
 
![advertise unique ids (you could renew that like always) ](/docs/img/blehdah.svg)

other devices record these UUIDs until the device operator needs to disseminate a status message 

![distribute status message](/docs/img/poststatus.svg)

get the current status 

![check status](/docs/img/getstatus.svg)

and never forget to forget that status again, dah  

![delete status](/docs/img/deletestatus.svg)

#### extensions

* proximity notification suppression whitelist   
  * you could design your app to include recognizable features in the public header
  * share a fixed CEK or share a generation pattern/seed 
  * or use additional CEKs, utilizing the general JWE serialization with multiple recipients (RFC_7516 7.2) 
  
* push notification
  * simple as that, you register your UUID and push token with your server
  
* disseminate only "verified" information
  * you are free to distrust / trust any tokens you might like
  * on what basis the tokens are generated is irrelevant and might include intensive tracking and verification (e.g. Personal-ID-ID)    
  * the server should store and offer the JWT ID , making it possible to track fraud
  

### TODO

* maybe specify reduced JWE-like token that include enough features for privacy keeping IDs. e.g. JWI
  * e.g. ditching CEK, directly asymmetrically encrypting the UUID
     

