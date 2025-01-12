# Authentication with React Router

This project demonstrates how to implement user authentication in a React application using **React Router** and **fetch API**. The implementation supports both login and signup modes, handling server communication, token storage, and expiration management.

## Features

- **Dynamic Authentication Modes**: Handle both login and signup flows based on URL parameters.
- **Form Data Handling**: Process user inputs and validate data.
- **Server Communication**: Send POST requests to the server with `fetch`.
- **JWT Token Management**: Store tokens in `localStorage` with expiration time.
- **Error Handling**: Handle HTTP status codes like `401`, `422`, and `500`.
- **Redirection**: Redirect users to the home page after successful authentication.

---

## Code Overview

### `AuthenticationPage` Component
This component renders the authentication form (`AuthForm`) and connects it to the logic defined in the `action` function.

```javascript
import { redirect } from "react-router-dom";
import AuthForm from "../components/AuthForm";

function AuthenticationPage() {
  return <AuthForm />;
}

export default AuthenticationPage;
```

### `action` Function
Handles form submission, validates data, communicates with the server, and manages token storage.

1. **URL Parameter Handling**:
   ```javascript
   const searchParams = new URL(request.url).searchParams;
   const mode = searchParams.get("mode") || "login";
   ```

2. **Mode Validation**:
   ```javascript
   if (mode !== "login" && mode !== "signup") {
     throw new Response(JSON.stringify({ message: "Invalid request" }), {
       status: 422,
     });
   }
   ```

3. **Form Data Processing**:
   ```javascript
   const data = await request.formData();
   const authData = {
     email: data.get("email"),
     password: data.get("password"),
   };
   ```

4. **Server Request**:
   ```javascript
   const response = await fetch(`http://localhost:8080/${mode}`, {
     method: "POST",
     headers: { "Content-Type": "application/json" },
     body: JSON.stringify(authData),
   });
   ```

5. **Response Handling**:
   ```javascript
   if (response.status === 422 || response.status === 401) {
     return response;
   }
   if (!response.ok) {
     throw new Response(JSON.stringify({ message: "Something went wrong" }), {
       status: 500,
     });
   }
   ```

6. **Token Management**:
   ```javascript
   const resData = await response.json();
   const token = resData.token;

   localStorage.setItem("token", token);
   const expiration = new Date();
   expiration.setHours(expiration.getHours() + 1);
   localStorage.setItem("expiration", expiration.toISOString());
   ```

7. **Redirect on Success**:
   ```javascript
   return redirect("/");
   ```

---

## JWT Token and Bearer Authorization

- The server returns a **JWT token**, which is stored in `localStorage` to maintain user authentication state.
- When making subsequent requests, the token is included in the `Authorization` header with the `Bearer` scheme:
  ```
  Authorization: Bearer <token>
  ```

---

## Error Handling

- **422**: Indicates invalid request data, such as an incorrect email format or short password.
- **401**: Indicates authentication failure, such as an invalid email or password.
- **500**: Indicates an unexpected server error.

---

## Project Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/JinLee0811/miniBlog_with_Auth.git
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Run the development server:
   ```bash
   npm start
   ```

---

## Learn More

- **React Router Documentation**: [https://reactrouter.com](https://reactrouter.com)
- **JWT Documentation**: [https://jwt.io](https://jwt.io)
- **Fetch API**: [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

For the complete implementation, check the repository: [GitHub](https://github.com/JinLee0811/miniBlog_with_Auth)
```
