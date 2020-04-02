## covidapi [DRAFT]

A specification on a "common ground" for multiple pandemic-contact-tracking-apps.
Providing a "unified" backend for leveraging on the larger user base. 

**the e-mail-standard for pandemic tracking**   

### introduction
the road so far

After having some look into several bluetooth / code based systems I found that they are not far apart in the basic aspects.
Sometimes the different apps seemed to add a lot of additional features. 
However for them to be successful they would  require a full scale adoption, making a single solution unlikely. 

Also the outlook of a single "government chosen" or a centralized tracking architecture did not appeal to me.    
In my opinion a centralized approach will never fly internationally in terms of politics and scalability. (let alone nationally)

So after digesting all the input I created a "common ground" specification. 
As I believe only a global and cooperative effort is the way to ensure the best outcome. 

### basic goals

* support a federated architecture
* distribute covid-status message to "acquaintances" without revealing your own identity
* offer authorities the means of distributing authenticated status information, but otherwise authority agnostic    
* grant access to status-message only to the specific app/user  
* make illicit tracking of users severely difficult
 
#### side goals

* do not hinder efficient scaling
* stay extensible to additional features
* leverage on well known standards
* offer a (authority) gated status "reset"  

### concept

Making it possible for multiple systems to coexist in a federated approach utilizing a cascading message distribution. 

Permanently change the advertised reference-IDs by utilizing encryption with random IVs.  
Encrypting the UUID with the apps "home-server" public key makes it possible to efficiently map it to the UUID on the servers side, when they get pushed.
Using JOSE (RFC 7516, RFC 7517, RFC 7518, RFC 7519) as central PKI "standard" for exchanging encrypted, anonymous and verified messages.                 

Authorities may choose to hold the privilege of generating tokens (JWT) at their discretion. Making it improbable to disseminate "fake" status messages in the first place. Or in a more liberal way the message can be submitted unverified.    
In the end the JWT-id may be associated with official documentation on authorities level or have no association at all. 
By verification of certificates this can be changed with no or little changes in code.       

#### basic mode of operation

devices generate new JWE (JSON Web Encryption) based reference-IDs of their UUID and advertise then via BT 
 
![advertise unique ids (you could renew that like always) ](/docs/img/blehdah.svg)

other devices record these reference-IDs until the device operator (user) needs to disseminate a status message 

![distribute status message](/docs/img/poststatus.svg)

get the current status 

![check status](/docs/img/getstatus2.svg)

and never forget to be able to forget that status again.  

![delete status](/docs/img/deletestatus.svg)

#### extensions / ideas

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
  
* the JWE could include an extended payload 
  * for example a hmac of the uuid including a device secret could be used to verify validity of presence and present this in the get-status request         
  
  
### open questions

* maybe specify reduced JWE-like token that include enough features for privacy keeping IDs. e.g. JWI
  * e.g. ditching CEK, directly asymmetrically encrypting the UUID

* is there an *adequate* alternative to JOSE?!  
     * there are alternatives for parts
     * with JOSE you can do a lot wrong and the hackish way of utilizing JWE for reference-IDs is certainly no intended use   
        
        
-----