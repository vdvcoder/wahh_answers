# Chapter 2 - Core Defense Mechanisms 
  

1. ### Why are an application's mechanisms for handling user access only as strong as the weakest of these components?

    >In a typical application, access is handled using a trio of mechanisms relating to authentication, session management, and access control. These components are highly interdependent, and a weakness in any one of them will undermine the effectiveness of the overall access handling mechanism. 
    For example, a defective authentication mechanism may enable an attacker to login as any user and so gain unauthorized access. If session tokens can be predicted, an attacker may be able to masquerade as any logged in user and gain access to their data. If access controls are broken, then any user may be able to directly use functionality that is supposed to be protected.

2. #### What is the difference between a session and a session token?**

    >A session is a set of data structures held on the server, which are used to track the state of the user’s interaction with the application. A session token is a unique string that the application maps to the session, and is submitted by the user to reidentify themselves across successive requests.

3. #### Why is it not always possible to use a whitelist-based approach to input validation?

    >There are many situations where an application may be forced to accept data for processing that does not match a list or pattern of input that is known to be “good”. For example, many people’s names contain characters that can be used in various attacks. If an application wishes to allow people to register under their real names, it needs to accept input that may be malicious, and ensure that this is handled and processed in a safe manner nevertheless.

4. #### You are attacking an application that implements an administrative function. You do not have any valid credentials to use the function. Why should you nevertheless pay close attention to it?

    >Defects in the any of the core mechanisms for handling access may enable you to gain unauthorized access to the administrative functionality. Further, data that you submit as a low privileged user may ultimately be displayed to administrative users, enabling you to attack them by submitting malicious data designed to compromise their session when it is viewed.

5. #### An input validation mechanism designed to block cross-site scripting attacks performs the following sequence of steps on an item of input:
     1. *Strip any `<script>` expressions that appear.*
     2. *Truncate the input to 50 characters.*
     3. *Remove any quotation marks within the input.*
     4. *URL-decode the input.*
     5. *If any items were deleted, return to step 1.*
    
     *Can you bypass this validation mechanism to smuggle the following data past it?*  
     `"><script>alert("foo")</script>`

     >Yes. If it were not for Step 4, this mechanism would be robust in terms of filtering the specific items it is designed to block. However, because your input is decoded after the filtering steps have been performed, you can simply URL-encode selected characters in your payload to evade the filter:

    ```
    %22>%3cscript>alert(%22foo%22)</script>
    ```
    ```
    %22>%3Cscript>alert(%22foo%22)%3C/script>
    ```

    >If Step 4 were performed first (or even not at all) then this bypass would not be possible.
