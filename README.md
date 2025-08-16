# FastAPI Docker Test

This repository contains a FastAPI application dockerized using Docker and Docker Compose. The application manages user data stored in a `users.json` file in the `app/data` directory, which is created automatically if it doesn't exist. Data persistence is achieved using a Docker volume.

## API Endpoints
- **GET /**: Returns a hello message (`{"message": "Hello from FastAPI"}`).
- **GET /users**: Returns a list of users stored in `app/data/users.json`.
- **POST /users**: Accepts user data (e.g., `{"first_name": "John", "last_name": "Doe", "age": 30}`) and appends it to `users.json`.

## Prerequisites
- Docker
- Docker Compose

## Repository Structure
```
docker-fastapi-test/
├── app/
│   ├── main.py         # FastAPI application code
│   ├── data/          # Directory for users.json (created automatically)
├── Dockerfile         # Docker configuration
├── docker-compose.yml # Docker Compose configuration
├── requirements.txt   # Python dependencies
├── README.md          # This file
```

## Setup Instructions
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/<your-username>/docker-fastapi-test
   cd docker-fastapi-test
   ```

2. **Build and Run**:
   ```bash
   docker-compose up --build
   ```
   This builds the Docker image and starts the container. The application is accessible at `http://localhost:8000`.

3. **Access Interactive Docs**:
   Open `http://localhost:8000/docs` in a browser to test endpoints via Swagger UI.

## Testing the Application
1. **Check Hello Message**:
   ```bash
   http://localhost:8000
   ```
   Expected output: `{"message": "Hello from FastAPI"}`

2. **Add a User**:
   ```bash
   curl -X POST -H "Content-Type: application/json" -d '{"first_name": "Akash", "last_name": "K", "age": 22}' http://localhost:8000/users
   ```

3. **List Users**:
   ```bash
   http://localhost:8000/users
   ```
   Expected output: `[{"first_name": "Akash", "last_name": "K", "age": 22}]`

## Verifying Data Persistence
1. **Add a User** (as above).
2. **Stop and Remove Container**:
   ```bash
   docker-compose down
   ```
3. **Recreate Container**:
   ```bash
   docker-compose up --build
   ```
4. **Verify Data**:
   ```bash
   http://localhost:8000/users
   ```
   The output should include the previously added user, confirming that `users.json` persists via the `fastapi-data` volume.

5. **Inspect Volume (Optional)**:
   ```bash
   docker volume ls  # Lists 'fastapi-data'
   docker run --rm -v fastapi-data:/data busybox cat /data/users.json
   ```

## Troubleshooting
- **Data Not Saving**: Ensure `app/main.py` creates `app/data/users.json` and has write permissions. The `Dockerfile` sets `chmod -R 777 /app/app/data`.
- **Container Logs**: Check for errors:
   ```bash
   docker logs fastapi-app
   ```
- **Volume Issues**: Remove and recreate the volume if needed:
   ```bash
   docker volume rm fastapi-data
   docker-compose up --build
   ```

## Dependencies
- `fastapi==0.103.0`
- `uvicorn==0.23.2`

See `requirements.txt` for the full list.

## Reference
- [FastAPI Tutorial](https://fastapi.tiangolo.com/tutorial/first-steps/)
