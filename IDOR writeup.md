 # IDOR Vulnerability Testing - A Hands-On Guide🔍

Imagine you're using a web app and notice that changing a URL parameter gives you access to another user's data. Sounds scary, right? This is Insecure Direct Object Reference (IDOR)—a vulnerability that can expose sensitive user information if not handled properly.
In this interactive guide, we'll explore IDOR vulnerabilities, how to test for them, and most importantly, how to prevent them. Ready? Let’s dive in! 

## 🎯 Why Should You Care About IDOR?
IDOR vulnerabilities can have serious consequences like:  
	• Unauthorized access to personal data   
	• Account takeovers and password resets   
	• Unauthorized modifications (e.g., changing someone’s order details)   
	• Leaking sensitive business or financial data   
If you’re a developer, tester, or security enthusiast, knowing how to identify and mitigate IDOR is a must-have skill!   

## 🕵️‍♂️ How to Test for IDOR - Real-World Scenarios

### User Profile Access Test
Scenario: Can you view another user’s profile by modifying the user_id in the URL?

#### Conversation: 👩‍💻 
Security Analyst: Hey, I noticed something weird in our app. When I change my user ID in the URL, I can see another user’s profile.That sounds like an IDOR vulnerability!    
Dev: Can you please show it   

##### 🔹 Steps to Test:
	1. Log in as User A and navigate to their profile: https://example.com/user/1234  
	2. Capture the request using Burp Suite or Postman   
	3. Modify the 1234 to 5678 and resend the request  
	4. Can you now see User B’s profile?  

##### 🔹Secure Behavior: 
      The server should return 403 Forbidden or redirect you to your own profile.  

### API Endpoint IDOR Test
Scenario: Can you access someone else's data by modifying API parameters?

#### Conversation: 👩‍💻 
Security Engineer:I was checking the API, and I think I found something suspicious.   
QA Tester:What did you find?  
Security Engineer: If I change the user ID in an API request, I can see another person’s order history!   

##### 🔹 Steps to Test:
	1. Capture an API request like:  
           GET /api/user/1234/orders  
           Authorization: Bearer XYZ  
	2. Modify 1234 to another user’s ID (5678).  
	3. If the response contains another user's orders, IDOR exists!  

##### 🔹 Secure Behavior: 
       The server should check if the authenticated user owns the requested data.

### File Download IDOR Test

Scenario: Can you download someone else’s private file by changing the file name in the request?

#### Conversation:👩‍💻 
 Security Analyst: I just tested our file download function, and guess what? If I change the   filename in the request, I can access files that aren’t mine.    
 Dev: Do we need to check permissions on file access.   
##### 🔹 Steps to Test:
	1. Identify a file download request:  
   	   GET /download?file=report123.pdf  
	2. Modify report123.pdf to another filename like report567.pdf  
	3. If the file gets downloaded without authentication, IDOR is confirmed!   

##### 🔹Secure Behavior: 
      The system should verify who owns the file before allowing downloads.

### Password Reset IDOR Test
Scenario: Can you reset another user's password by modifying the email or user ID in a password reset request?  

#### Conversation:👨‍💻  
Security Engineer: I requested a password reset, but I got a link for another user’s account!    
Dev: Let’s dig into the logs and fix it.    
##### 🔹 Steps to Test:
	1. Request a password reset link and capture the request:  
           { "email": "userA@example.com" }    
	2. Modify userA@example.com to userB@example.com.  
	3. If the reset link is sent to your email instead of User B’s, IDOR is present!  

##### 🔹Secure Behavior: 
      The reset request should only work for the authenticated user and require additional   
      verification (e.g., 2FA).

### E-commerce Order Modification Test
  Scenario: Can you modify another customer’s order details?

#### Conversation:👨‍💻 
Customer Service:A customer just called saying their order status changed to ‘Cancelled’ without them doing anything.”
Security Team:Sounds suspicious. Let’s check the request logs.  
##### 🔹 Steps to Test:
	1. Place an order and capture the request:
           POST /api/orders/submit  
           {"order_id": "9876", "user_id": "1234"}  
	2. Modify user_id or order_id to another customer’s details.  
	3. If the system allows the change, IDOR is present!  

##### 🔹 Secure Behavior: 
 The backend should validate ownership of the order before making changes.

### Banking Portal Transaction Test
Scenario: Can you view another user’s bank transactions?

#### Conversation: 👩‍💻
Ethical Hacker:I noticed that if I change the transaction ID in the URL, I can see someone else’s payment details.    
Bank Security Team:That’s a major breach! We need to enforce strict access control.    
##### 🔹 Steps to Test:
	1. Navigate to your transactions page:
           GET /bank/transactions/1234  
	2. Modify 1234 to another transaction ID like 5678.  
	3. Can you see another customer’s transactions? If yes, IDOR exists.  

##### 🔹Secure Behavior: 
     The system should enforce strict user authentication and session validation.  


## 🛡️ How to Prevent IDOR

 1. Implement Proper Authorization Checks Every request should validate who is making the request before granting access.
 2. Use Secure Identifiers Instead of using sequential IDs (user_id=1234), use GUIDs or hashed identifiers.
 3. Enforce Role-Based Access Control (RBAC) Restrict data access based on user roles and permissions.
 4. Use Session & Token Validation Ensure every request is authenticated using secure session tokens.
 5. Log and Monitor Requests Use security monitoring tools to detect suspicious activity in real time.
 6. Conduct Regular Security Testing Use security tools like:
