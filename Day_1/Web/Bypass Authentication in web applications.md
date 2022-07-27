# Bypass Authentication in web applications

- ## Difinition of Authentication 

- Authentication is the process of verifying the identity of an individual. A user can interact with a web application using multiple actions. Access to certain actions or pages can be restricted using user levels. Authorization is the process of controlling user access via assigned roles & privileges

- ## Some ways to bypass authentication in web applications
- ### Bypass Authentication using sql injection 
 ![image info](https://raw.githubusercontent.com/ADNXB/test/main/owasp_injection_10.png)
### first we need to check for potential SQL injection vulnerability we have entered a single quote in to the Name field and submitted the request 
.
 ![image info](https://raw.githubusercontent.com/ADNXB/test/main/owasp_injection_11.png)

#### so we generated an SQL error message , let's exploit this 
![image info](https://raw.githubusercontent.com/ADNXB/test/main/owasp_injection_12.png)
#### Enter some appropriate syntax to modify the SQL query into the "Name" input in this example we used ```' or 1=1 -- .This causes the application to perform the query:SELECT * FROM users WHERE username = '' OR 1=1-- ' AND password = 'RandomPassword'```Because the comment sequence (--) causes the remainder of the query to be ignored, this is equivalent to ```SELECT * FROM users WHERE username = ' ' OR 1=1```

![image](https://raw.githubusercontent.com/ADNXB/test/main/owasp_injection_13%20(1).png)
#### As we can see, we successfully log in as administrator

- ##  bypass authentication using Session ID prediction
- ### definition of session id prediction
#### The session prediction attack focuses on predicting session ID values that permit an attacker to bypass the authentication schema of an application. By analyzing and understanding the session ID generation process, an attacker can predict a valid session ID value and get access to the application

- ## how to exploit it 
![image](https://raw.githubusercontent.com/ADNXB/test/main/cookies.jpg)

#### As we can see here in this web application the session id variable is represented by JSESSIONID and its value is "user01" , so if we try to change this value by user03 or admin we can access their account, we can find in other scenarios the cookies are encrypted by Known cipher We can decrypt it, change the value, encrypt it again, and send the request 

- ## bypass jwt token
- ### difinition of jwt token
 #### A JSON Web Token (JWT) is a JSON object that is defined in RFC 7519 as a safe way of transmitting information between two parties. Information in the JWT is digitally-signed, so that it can be verified and trusted 

- ### how to bypass jwt token 
right now we will talk just about one way to bypass jwt token and it is by setting ```alg : none``` 

![image](https://raw.githubusercontent.com/ADNXB/test/main/alg-none.jpg)

#### When the server receives such form of token , as a result of “none” algorithm it does not perform the signature verification . Thus now the attacker could manipulate the value payload without any server side detection and get vertical or horizontal privilege escalation.

- ## the end
### i hope you guys enjoy my first article and I will be glad if you notice any mistakes I made and tell me.




