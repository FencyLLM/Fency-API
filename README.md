# Fency API Documentation

Transform reality into encoded art. Generate images with AI and convert them to ASCII masterpieces.

## Base URL

```
https://fency.onrender.com
```

## Endpoints

### Generate Image

Generate AI images from text prompts using Google's Imagen-4 model.

**Endpoint:** `POST /api/generate-image`

**Request Body:**
```json
{
  "prompt": "your image description here",
  "variations": 1
}
```

**Parameters:**
- `prompt` (string, required): Text description of the image to generate
- `variations` (number, optional): Number of image variations to create. Default: 1

**Response:**
```json
{
  "success": true,
  "jobId": "unique-job-identifier",
  "estimatedTime": "30-60 seconds"
}
```

**Example:**
```javascript
const response = await fetch('https://fency.onrender.com/api/generate-image', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    prompt: 'a cyberpunk city at night with neon lights',
    variations: 1
  })
});

const data = await response.json();
console.log(data.jobId); // Use this to check status
```

---

### Check Generation Status

Poll the status of your image generation job.

**Endpoint:** `GET /api/image-status/:jobId`

**Parameters:**
- `jobId` (string, required): The job ID returned from generate-image endpoint

**Response (Processing):**
```json
{
  "status": "processing"
}
```

**Response (Success):**
```json
{
  "status": "succeeded",
  "images": [
    "https://replicate.delivery/pbxt/image-url.png"
  ]
}
```

**Response (Failed):**
```json
{
  "status": "failed"
}
```

**Example:**
```javascript
const checkStatus = async (jobId) => {
  const response = await fetch(`https://fency.onrender.com/api/image-status/${jobId}`);
  const data = await response.json();

  if (data.status === 'succeeded') {
    console.log('Image ready:', data.images[0]);
  } else if (data.status === 'processing') {
    console.log('Still generating...');
    setTimeout(() => checkStatus(jobId), 2000); // Check again in 2s
  }
};
```

---

### Get Media File

Retrieve generated media files by ID.

**Endpoint:** `GET /api/media/:fileId`

**Parameters:**
- `fileId` (string, required): The media file identifier

**Response:**
Returns the media file with appropriate content-type headers.

---

## Image Specifications

- **Aspect Ratio:** 16:9 (widescreen)
- **Model:** Google Imagen-4
- **Output Format:** PNG
- **Generation Time:** Approximately 30-60 seconds

---

## ASCII Encoder

The Fency encoder transforms images into monochrome ASCII art using character density mapping.

### Encoding Settings

**Font Size:**
- Image mode: 5-20px (default: 8px)
- Video mode: 10-20px (default: 11px)

**Character Spacing:**
- Range: -10 to 0px (default: -5px)
- Negative values create denser output

**Output Resolution:**
- Images: 700px height, aspect-ratio preserved
- Videos: 1080px height (Full HD)

### Character Map

The encoder uses 7 characters representing brightness levels:

```
' '  → Space (darkest, 0-51 brightness)
"'"  → Apostrophe (51-102)
':'  → Colon (102-140)
'i'  → Lowercase i (140-170)
'I'  → Uppercase I (170-200)
'J'  → Uppercase J (200-210)
'$'  → Dollar sign (brightest, 210-255)
```

### Video Encoding

- **Recording FPS:** 120 (ultra-smooth playback)
- **Output Format:** WebM
- **Bitrate:** 25 Mbps (maximum quality)
- **Codec:** VP9 (fallback to VP8/WebM)

---

## Rate Limits

The API uses a token-based queue system to manage rate limits across multiple Replicate API keys. Requests are automatically queued and processed as tokens become available.

---

## Links

- **Website:** [fency.xyz](https://fency.xyz)
- **X/Twitter:** [@fencyLLM](https://x.com/fencyLLM)

---

## Error Handling

All endpoints return appropriate HTTP status codes:

- `200` - Success
- `400` - Bad Request (missing or invalid parameters)
- `404` - Not Found (invalid jobId or fileId)
- `500` - Internal Server Error

**Error Response Format:**
```json
{
  "success": false,
  "message": "Error description here"
}
```

---

*The grid is eternal. The truth is monochrome. The transformation starts now.*
