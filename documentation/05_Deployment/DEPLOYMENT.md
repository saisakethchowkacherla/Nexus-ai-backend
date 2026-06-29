# Deployment Documentation

## Prerequisites
- Python 3.10 or newer
- `requirements.txt` installed
- `yolov8n.pt` model file available at repository root
- Access to camera frames or image uploads

## Local Deployment
1. Create and activate a virtual environment:

```bash
python -m venv venv
source venv/Scripts/activate
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Start the server:

```bash
python run.py
```

4. Access the backend at:

```
http://0.0.0.0:8000
```

## Production Deployment
Use Uvicorn with workers for production workloads.

Example:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4
```

## Environment Variables
The current backend does not use custom environment variables. The FastAPI app is configured to run on `0.0.0.0:8000` by default.

## Frontend Integration
The backend is configured with CORS for `http://localhost:5173` to allow frontend requests from the React application.

## Notes
- Ensure `drivers.db` is writable by the running process.
- For improved performance, serve behind a reverse proxy such as Nginx.
- If GPU acceleration is available, consider adjusting InsightFace and YOLO runtime providers.

## Scaling
- For higher traffic, deploy multiple Uvicorn worker processes
- Replace SQLite with a networked database for multi-instance deployments
- Add health checks and logging for observability
