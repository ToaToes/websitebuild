

Diagram that explains CORS visually:

```
Frontend Browser (http://localhost:3000)
        |
        |  -- AJAX / fetch request -->
        |
Backend Server (http://127.0.0.1:8000)
        |
        |  Checks "Origin" header
        |  If allowed, adds CORS headers:
        |     Access-Control-Allow-Origin: http://localhost:3000
        |
        |  <--- Response with data ----
        |
Browser allows JS to read the response ✅
```
