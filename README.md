# 🔐 FastAPI Authentication API

A secure, production-ready authentication system built with FastAPI, featuring JWT tokens, password hashing, and SQLite database storage.

## ✨ Features

- **User Registration & Login**: Secure user account creation and authentication
- **JWT Token Authentication**: Stateless authentication with configurable expiration
- **Password Security**: Bcrypt hashing for secure password storage
- **Protected Endpoints**: Route protection with JWT token validation
- **CORS Support**: Cross-origin resource sharing enabled
- **SQLite Database**: Lightweight, file-based database with automatic table creation
- **RESTful API**: Clean, intuitive API design following REST principles
- **Interactive Documentation**: Auto-generated API docs with Swagger UI

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- pip (Python package installer)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/fastapi-auth.git
   cd fastapi-auth
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv authenv
   # On Windows
   authenv\Scripts\activate
   # On macOS/Linux
   source authenv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install fastapi uvicorn python-jose[cryptography] passlib[bcrypt] python-multipart
   ```

4. **Run the application**
   ```bash
   python -m uvicorn main:app --reload --host 0.0.0.0 --port 8000
   ```

5. **Access the application**
   - API: http://localhost:8000
   - Interactive docs: http://localhost:8000/docs
   - Alternative docs: http://localhost:8000/redoc

## 📚 API Documentation

### Authentication Endpoints

#### Register User
```http
POST /register
Content-Type: application/json

{
  "username": "your_username",
  "password": "your_password"
}
```

**Response:**
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "token_type": "bearer"
}
```

#### Login
```http
POST /token
Content-Type: application/x-www-form-urlencoded

username=your_username&password=your_password
```

**Response:**
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "token_type": "bearer"
}
```

#### Get Current User
```http
GET /users/me
Authorization: Bearer your_jwt_token
```

**Response:**
```json
{
  "username": "your_username",
  "hashed_password": "hashed_password_string"
}
```

## 🔧 Configuration

### Environment Variables

The following configurations can be customized in `main.py`:

```python
SECRET_KEY = "your-secret-key-change-this-in-production"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30
```

### Security Settings

- **JWT Algorithm**: HS256 (HMAC with SHA-256)
- **Token Expiration**: 30 minutes (configurable)
- **Password Hashing**: Bcrypt with automatic salt generation
- **CORS**: Configured for development (allows all origins)

## 🛡️ Security Features

- **Password Hashing**: Uses bcrypt for secure password storage
- **JWT Tokens**: Secure, stateless authentication
- **SQL Injection Protection**: Parameterized queries prevent SQL injection
- **Token Validation**: Comprehensive JWT token verification
- **Error Handling**: Secure error responses without information leakage

## 🗄️ Database

The application uses SQLite with automatic table creation:

```sql
CREATE TABLE users (
    username TEXT PRIMARY KEY,
    hashed_password TEXT
);
```

## 🧪 Testing

### Manual Testing with cURL

1. **Register a new user:**
   ```bash
   curl -X POST "http://localhost:8000/register" \
        -H "Content-Type: application/json" \
        -d '{"username": "testuser", "password": "testpass"}'
   ```

2. **Login:**
   ```bash
   curl -X POST "http://localhost:8000/token" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        -d "username=testuser&password=testpass"
   ```

3. **Access protected endpoint:**
   ```bash
   curl -X GET "http://localhost:8000/users/me" \
        -H "Authorization: Bearer YOUR_JWT_TOKEN"
   ```

### Frontend Testing

Open `index.html` in your browser to test the login functionality with a simple web interface.

## 📁 Project Structure

```
fastapi-auth/
├── main.py              # Main FastAPI application
├── index.html           # Simple frontend for testing
├── users.db             # SQLite database (auto-created)
├── authenv/             # Virtual environment
├── requirements.txt     # Python dependencies
└── README.md           # This file
```

## 🔄 Development

### Adding New Endpoints

To add new protected endpoints, use the `get_current_user` dependency:

```python
@app.get("/protected-route")
async def protected_endpoint(current_user: dict = Depends(get_current_user)):
    return {"message": f"Hello {current_user['username']}!"}
```

### Database Modifications

To modify the database schema, update the `init_db()` function and restart the application.

## 🚀 Production Deployment

### Security Considerations

1. **Change the SECRET_KEY** to a strong, random string
2. **Restrict CORS origins** to your specific domains
3. **Use environment variables** for sensitive configuration
4. **Enable HTTPS** in production
5. **Implement rate limiting** for authentication endpoints
6. **Add logging** for security events

### Environment Variables

Create a `.env` file:
```env
SECRET_KEY=your-super-secret-key-here
DATABASE_URL=sqlite:///./users.db
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

### Docker Deployment

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [FastAPI](https://fastapi.tiangolo.com/) - Modern, fast web framework
- [python-jose](https://python-jose.readthedocs.io/) - JWT implementation
- [passlib](https://passlib.readthedocs.io/) - Password hashing library
- [SQLite](https://www.sqlite.org/) - Lightweight database

## 📞 Support

If you have any questions or need help, please open an issue on GitHub.

---

**⭐ Star this repository if you found it helpful!**
