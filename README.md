# Quiz Platform - Complete Setup & Testing Guide

## 📋 Project Overview

This is a full-stack Quiz Platform with:

- **Frontend**: React with Vite (running on port 5173)
- **Backend**: Spring Boot microservices (running on port 8080)
    - Auth Service (authentication)
    - Quiz Service (quiz management)
    - Attempt Service (quiz attempts and results)
    - Discovery Service (service discovery)
    - Gateway Service (API gateway)

---

## 🚀 Quick Start (5 Minutes)

### Option 1: Using Docker Compose (Recommended)

```bash
cd quiz-platform
docker-compose up -d
```

This will start all backend services automatically.

### Option 2: Start Services Manually


#### Start Frontend

```bash
cd quiz-platform/quiz-frontend
npm install
npm run dev
```

#### Start Backend Services

```bash
# Terminal 1 - Auth Service
cd quiz-platform/auth-service
mvn spring-boot:run

# Terminal 2 - Quiz Service  
cd quiz-platform/quiz-service
mvn spring-boot:run

# Terminal 3 - Attempt Service
cd quiz-platform/attempt-service
mvn spring-boot:run

# Terminal 4 - Discovery Service (Optional)
cd quiz-platform/discovery-service
mvn spring-boot:run

# Terminal 5 - Gateway Service (Optional)
cd quiz-platform/gateway-service
mvn spring-boot:run
```

### 🌐 Open in Browser

| Service  | URL                                            |
|----------|------------------------------------------------|
| Frontend | [http://localhost:5173](http://localhost:5173) |
| Backend  | [http://localhost:8080](http://localhost:8080) |

> 💡 **Tip:** All backend services are accessible via the Gateway at port 8080.

---

## 📂 Project Structure

```
quiz-platform/
├── quiz-frontend/                 # React frontend
│   ├── src/
│   │   ├── api/                   # API service layer
│   │   ├── components/            # Reusable components
│   │   ├── context/               # React context (auth)
│   │   ├── pages/                 # Page components
│   │   ├── App.jsx                # Main app with routes
│   │   └── main.jsx               # Entry point
│   ├── QUICK_START.md
│   └── package.json
│
├── auth-service/                  # Spring Boot auth service
│   └── src/main/java/com/aryan/
│
├── quiz-service/                  # Spring Boot quiz service
│   └── src/main/java/com/aryan/
│
├── attempt-service/               # Spring Boot attempt service
│   └── src/main/java/com/aryan/
│
├── discovery-service/             # Service discovery (Eureka)
│   └── src/main/java/com/aryan/
│
├── gateway-service/               # API Gateway
│   └── src/main/java/com/aryan/
│
├── TESTING_GUIDE.md               # Detailed testing guide
└── docker-compose.yml             # Docker configuration
```

---

## 🎯 Frontend Features

### ✅ Implemented

- **Authentication**
    - User registration with email and password
    - User login with JWT token
    - Persistent login (token stored in localStorage)
    - Logout with token cleanup

- **Authorization**
    - Protected routes (require authentication)
    - Public routes (accessible to all)
    - Route redirects based on auth status
    - Automatic redirect to login on 401

- **Quiz Functionality**
    - Browse all available quizzes
    - View quiz cards with details (title, description, question count, difficulty)
    - Take quizzes one question at a time
    - Select answers with radio buttons
    - Navigate between questions (Previous/Next buttons)
    - Jump to specific questions
    - Submit quiz answers
    - View immediate results

- **Results**
    - Display score percentage with color-coded status
    - Show correct/incorrect answer counts
    - Display time taken
    - Review all questions with correct answers
    - Navigate back to quizzes

- **User Dashboard**
    - View attempt history in a professional table
    - See quiz name, date, score, status, and time taken
    - Access detailed results for any attempt

- **UI/UX**
    - Responsive navigation bar with mobile menu
    - Mobile-friendly design (works on phones/tablets)
    - Professional styling with gradients and animations
    - Loading indicators
    - Error handling and messages
    - Form validation

---

## 🔌 API Endpoints

### Auth Service

```
POST /auth/register
Body: { email, password }
Response: { token, user }

POST /auth/login
Body: { email, password }
Response: { token, user }
```

### Quiz Service

```
GET /quiz
Response: [{ id, title, description, questionCount, difficulty, ... }]

GET /quiz/:id
Response: { id, title, questions: [{ id, text, options: [...] }] }
```

### Attempt Service

```
GET /attempt
Response: [{ id, quizId, quizTitle, correctAnswers, totalQuestions, createdAt, ... }]

POST /attempt
Body: { quizId, answers: [{ questionId, selectedOptionId }] }
Response: { attemptId, id, ... }

GET /attempt/:attemptId
Response: { correctAnswers, totalQuestions, reviewAnswers, ... }
```

---

## 🧪 Testing Routes

### Public Routes (No auth required)

- `/` - Home page
- `/login` - Login (redirects to /quizzes if already logged in)
- `/register` - Register (redirects to /quizzes if already logged in)
- `*` - 404 Not Found

### Protected Routes (Auth required)

- `/quizzes` - Browse all quizzes
- `/quiz/:id` - Take a quiz
- `/result/:attemptId` - View quiz results
- `/attempts` - View attempt history

### Test Workflow

1. **Open browser**: http://localhost:5173
2. **Register**: Create new account
3. **Login**: Login with your account
4. **Browse**: View available quizzes
5. **Take Quiz**: Answer all questions and submit
6. **View Result**: See your score and review answers
7. **History**: View all your attempts
8. **Logout**: Logout and verify redirect to login

---

## 🔐 Authentication Flow

### Registration

```
User fills form → POST /auth/register → Token received → Redirect to /login
```

### Login

```
User enters credentials → POST /auth/login → Token stored in localStorage 
→ JWT added to axios headers → Redirect to /quizzes
```

### Protected Routes

```
User accesses /quizzes → Check AuthContext.isAuthenticated 
→ True: Show page | False: Redirect to /login
```

### Logout

```
User clicks logout → Remove token from localStorage → Update AuthContext 
→ Clear axios Authorization header → Redirect to /login
```

### Unauthorized Response

```
Protected API call returns 401 → Remove token → Redirect to /login
```

---

## 🛠️ Technology Stack

### Frontend

- **React 18** - UI framework
- **Vite** - Build tool
- **React Router v6** - Client-side routing
- **Axios** - HTTP client
- **CSS3** - Styling (no external CSS framework)

### Backend

- **Spring Boot** - Framework
- **Spring Security** - Authentication
- **JWT** - Token-based auth
- **Spring Data JPA** - Database ORM
- **MySQL/PostgreSQL** - Database

### DevOps

- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration

---

## 📖 Detailed Documentation

- **Frontend Setup**: See `quiz-frontend/QUICK_START.md`
- **Testing Guide**: See `TESTING_GUIDE.md`
- **Backend Setup**: See respective service README files

---

## 🔍 Troubleshooting

### Frontend not loading

```bash
# Check if port is in use
lsof -i :5173

# Kill process on that port
kill -9 <PID>

# Restart
npm run dev
```

### Backend API not responding

```bash
# Check if backend is running
curl http://localhost:8080/quiz

# Check service logs for errors
# In backend terminal, look for exception messages
```

### JWT token issues

```javascript
// In browser console:
localStorage.getItem('token')     // Should return JWT
localStorage.clear()               // Clear auth data
location.reload()                  // Refresh page
```

### CORS errors

- Ensure backend has CORS configuration
- Check browser console for specific errors
- Verify backend URL in axiosInstance

### Quiz not loading

- Verify quiz data exists in backend database
- Check network tab in DevTools for failed requests
- Look at backend logs for database errors

---

## ✅ Testing Checklist

### Setup

- [ ] Frontend running on http://localhost:5173
- [ ] Backend services running on http://localhost:8080
- [ ] No errors in browser console
- [ ] Navbar visible at top of page

### Authentication

- [ ] Can register new account
- [ ] Can login with credentials
- [ ] Token stored in localStorage after login
- [ ] User email shows in navbar
- [ ] Can logout and return to login

### Routes

- [ ] Unauthenticated users redirected to /login
- [ ] Authenticated users redirected from /login to /quizzes
- [ ] Can access /quizzes when logged in
- [ ] Cannot access /quizzes when logged out

### Quizzes

- [ ] Quizzes load and display as cards
- [ ] Can click quiz card to start quiz
- [ ] Questions display one at a time
- [ ] Can navigate between questions
- [ ] Can select answers
- [ ] Cannot submit without answering all

### Results

- [ ] Results page shows score percentage
- [ ] Results page shows correct/total count
- [ ] Can see review of answers
- [ ] Can return to quizzes from result page

### Attempt History

- [ ] Can access history page
- [ ] Attempts show in table format
- [ ] Can view result from history

### Responsive Design

- [ ] Works on desktop (1920px+)
- [ ] Works on tablet (768px-1024px)
- [ ] Works on mobile (320px-480px)
- [ ] Hamburger menu appears on small screens

---

## 📞 Support & Debugging

### Enable Debug Mode

```javascript
// In main.jsx:
import { AuthProvider } from './context/AuthContext'
import React from 'react'

// Add React.StrictMode to catch issues
```

### Check Network Requests

1. Open DevTools (F12)
2. Go to Network tab
3. Perform action
4. Check request/response

### View API Calls

```javascript
// In browser console:
// Check what's being sent
// Check response status
// Check Authorization header
```

### Backend Logs

- Look for error messages in backend terminal
- Check database connectivity
- Verify JWT signing key configuration

---

## 🎓 Learning Resources

- React Router: https://reactrouter.com/
- Axios: https://axios-http.com/
- Spring Boot: https://spring.io/projects/spring-boot
- JWT: https://jwt.io/
- Vite: https://vitejs.dev/

---

## 📝 Notes

- Passwords should be hashed on backend (never store plaintext)
- JWT tokens should have expiration time
- Implement refresh token mechanism for production
- Add rate limiting to authentication endpoints
- Validate all inputs on both frontend and backend
- Use HTTPS in production (not HTTP)
- Store sensitive data in environment variables

---

## 🎉 Success!

Once everything is running and you can:

1. Register and login
2. Browse quizzes
3. Take a quiz
4. See results
5. View attempt history

**Congratulations!** Your Quiz Platform is fully functional! 🚀

---

**Last Updated**: January 25, 2026
**Version**: 1.0.1
