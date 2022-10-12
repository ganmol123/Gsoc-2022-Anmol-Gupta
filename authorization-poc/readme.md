# Authorization Example with Keycloak

This is a simple example of Authentication and Authorization connecting the Java Application with Keycloak. Here I have a simple case studey which goes as, Suppose a district with many schools wants to create a single sign-on system. They want to maintain their students' and teachers' single account IDs across multiple schools using a centralized platform. This lets each user have the same role, but with different access and privileges at each school.

## Steps on Keycloak

- Create a keycloak realm. Name it 'education', set Enabled as ON and click on CREATE.
- Now, on the roles page, select the Realm roles tab. Click on ADD ROLE to create two seperate roles for this realm called 'teacher' and 'student'.
- On the clients page, create a client named 'school' and save it.
- Now on the Settimgs tab, enter Client ID as school, Enabled as ON. Consent Required as OFF, Client Protocol as openid-connect, Access Type as confidential, Standard Flow Enabled as ON, Impact Flow Enabled as OFF and Direct Access Grants Enbled as ON.
- Now in the Authentication Flow Overrides Part, et Browser Flow as browser and Direct Grant FLow as direct grant.
- In the Roles tab, go to Add Role and create roles - add-student-grade, get-student-grade and get-student-portfolio for the client.
- In the Client Scopes tab and in the Default Client Scopes section, add "roles" and "profile" to the Assigned Default Client Scopes.
- On the school details page, select Mappers and then Create Protocol Mappers, and set mappers to display the client roles on the Userinfo API.

```
Name: roles
Mapper Type: User Realm Role
Multivalued: ON
Token Claim Name: roles
Claim JSON Type: String
Add to ID token: OFF
Add to access token: OFF
Add to userinfo: ON
```

- On the Users page, select Add user, create the new users, and click Save.

```
Username: anmol
Email: anmolgup.1999@gmail.com
First Name: Anmol
Last Name: Gupta
User Enabled: ON
Email Verified: OFF
```

- Last step will be go to the Role Mappings tab, select the Client Roles for each user in school. These should be add-student-grade, get-student-grade, and get-student-portfolio.
- This concludes keycloak configiuration.

## Integrating Keycloak with a Java Sample Application

Here I have a very simple Java application. This application connects with the Keycloak instances and uses Keycloak's authentication and authorization capability through its REST API.

- We first develop the Java application starting with a pom.xml file. (.pom.xml)
- We will also need to create a properties files in which we will pass the env values. (.main/resources/application.properties)
- We now need to get the Keycloak Certificate ID. Which can be retrieved by going to the realm page -> Keys -> Active. The ID's can be found under 'Kid'.
- Now we start developing the integration code which will involve many Keycloak API's. Here is an exmple of a simple logout API.

```
@Value("${keycloak.logout}")
private String keycloakLogout;

public void logout(String refreshToken) throws Exception {
    MultiValueMap<String, String> map = new LinkedMultiValueMap<>();
    map.add("client_id",clientId);
    map.add("client_secret",clientSecret);
    map.add("refresh_token",refreshToken);
    HttpEntity<MultiValueMap<String, String>> request = new HttpEntity<>(map, null);
    restTemplate.postForObject(keycloakLogout, request, String.class);
    }
```

- For logging out, we can see above that we use refresh_token and not the access_token. The API can check if the bearer token is valid or not in order to validate whether a request is bringing a valid credential or not.

## Authorization

- We have two approches for authorization.
- One way is to determine what role a bearer token brings by verifying it against Keycloak's userinfo API. We will have the following response from Keycloak.

```
{
    "sub": "ef2cbe43-9748-40e5-aed9-fe981e3082d5",
    "roles": ["teacher"],
    "name": "Anmol G",
    "preferred_username": "Anmol",
    "given_name": "Anmol",
    "family_name": "G"
}
```

- Here we have a roles tag and we can validate the access right based on that.
- The drawback is the multiple roundtrip request between your application and Keycloak for each request, which results in higher latency.
- Second way is to validate a role within the bearer token. Here we read the contents of the JWT token, which are sent through each request.
- To decode the JWT token, we need the public key used for signing it. Keycloak provides a JWKS endpoint and it's contents can be viewed by using a simple curl command shown below,

```
$ curl -L -X GET 'http://localhost:8080/auth/realms/education/protocol/openid-connect/certs'
```

- Below is the sample response,

```
{
    "keys": [
        {
            "kid": "key_id",
            "kty": "RSA",
            "alg": "RS256",
            "use": "sig",
            "n": "public_key",
            "e": "AQAB"
        }
    ]
}
```

- Here kid means key_id, alg is algorithm and n is the public_key. This public key can be used to decode the JWT token and access roles from the JWT claim. Sample decoded JWT token as follows:

```
{
  "jti": "85edca8c-a4a6-4a4c-b8c0-356043e7ba7d",
  "exp": 1498879194,
  "nbf": 0,
  "iat": 1498879190,
  "iss": "http://localhost:8080/auth/realms/education",
  "sub": "ef2cbe43-9748-40e5-aed9-fe981e3082d5",
  "typ": "Bearer",
  "azp": "school",
  "auth_time": 0,
  "session_state": "f8ab78f8-15ee-403d-8db7-7052a8647c65",
  "acr": "1",
  "realm_access": {
    "roles": [
      "teacher"
    ]
  },
  "resource_access": {
    "school": {
      "roles": [
        "get-student-grade",
        "get-student-portfolio",
        "get-student-grade"
      ]
    }
  },
  "scope": "profile",
  "name": "Anmol G",
  "preferred_username": "Anmol",
  "given_name": "Anmol",
  "family_name": "G"
}
```

- This approach is more favourable beacuse we can place the public key from keycloak in a cache, which reduces the round-trip request and it increases application latency and performance.

## Conclusion

I have demonstrated how to enable many aspects of authentication and authorization using Keycloak REST API and developing a simple Java application that connects to your Keycloak instances, and uses Keycloak's authentication and authorization capability through its REST API.

## Tools Used

- Java 11
- Spring Boot
- Keycloak 4.8.3

## Keycloak API Created

- Login API
- Logout API
- User Info API
- JWK Certs API

## Application API Created

```
http://localhost:8082/login
http://localhost:8082/logout
http://localhost:8082/student
http://localhost:8082/teacher
http://localhost:8082/valid
```

## Resources

-[Authorization - PH-EE](https://docs.google.com/document/d/1ybUzb5mLya2gdYAFSt6PpaTc22N_D6-es_ZyaFcAlzM/edit?usp=sharing)
