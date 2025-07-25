# API Documentation: 

`POST /run-download`

**Endpoint:**  
`https://posb-app-be.onrender.com/run-download`

## Description

Checks "opened" and "closed" reports for the previous working day. If success response then the reports are present else they are not.

---

## Request

### Method

`POST`

### Headers

- `Content-Type: application/json`

### Body Parameters

| Name          | Type   | Required | Default         | Description                                                      |
|---------------|--------|----------|-----------------|------------------------------------------------------------------|
| `type`        | string | No       | `"opened"`      | Type of report to download. (Currently always downloads both.)   |
| `download_dir`| string | No       | Current working directory | Directory path to save the downloaded reports.                   |

**Example Request Body:**
```json
{
  "type": "opened",
  "download_dir": "/path/to/save"
}
```

---

## Response

### Success

- **Status Code:** `200 OK`
- **Content:**
    ```json
    {
      "status": "success",
      "message": "opened&closed reports present/downloaded."
    }
    ```

### Error

- **Status Code:** `500 Internal Server Error`
- **Content:**
    ```json
    {
      "status": "error",
      "message": "<error details>"
    }
    ```

---

## Example Usage

**cURL:**
```bash
curl -X POST https://posb-app-be.onrender.com/run-download \
  -H "Content-Type: application/json" \
  -d '{"type": "opened", "download_dir": "/tmp/reports"}'
```

---

## Notes

- Checks the files presence and the API responds immediately.
- The `type` parameter is reserved for future use; currently, both "opened" and "closed" reports are always downloaded.
- The `download_dir` parameter is optional. If not provided, the server's current working directory is used.
- This endpoint is CORS-enabled.

---

## Contact

For support or questions, contact the POSB Automation Team(handled by CVRROCKET).
cvrrocket@gmail.com
