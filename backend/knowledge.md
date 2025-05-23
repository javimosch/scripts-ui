# Backend Knowledge Base

## Technology Stack
- Express.js
- WebSocket (ws package)
- Multer for file uploads
- dotenv for environment configuration

## API Endpoints

### Scripts
- GET `/api/scripts`: List all scripts
- POST `/api/scripts/upload`: Upload new scripts
- DELETE `/api/scripts/:script`: Delete a script

### Sources
- GET `/api/sources`: List all sources
- POST `/api/sources`: Add new source
- PUT `/api/sources/:id`: Update source
- DELETE `/api/sources/:id`: Delete source

## Script Execution
- Node.js (.js): Direct execution with environment variables and command-line arguments
- Deno (.ts): Uses --env flag for environment variables, supports command-line arguments
- Shell (.sh): Executed with bash, supports command-line arguments
- Arguments are passed directly to the script after the script path

## Environment Variables
- Node.js/Shell: Passed through process.env
- Deno: Passed as --env KEY=VALUE arguments
- Global variables are merged with script-specific ones
- Environment variables for Deno are written to temporary .env file

## File Management
- Upload size limit: 1MB
- Allowed extensions: .js, .ts, .sh
- Shell scripts automatically get execute permissions
- Files stored in SCRIPTS_DIR (configurable)

## Source Management
- Default source is protected (no modify/delete)
- Sources stored in data/sources.json
- Source paths must be absolute or relative to project root

## WebSocket Protocol
Server expects:
```json
{
  "type": "run",
  "script": "script_name",
  "config": {
    "args": "string",      // Optional command-line arguments
    "denoFlags": "string", // Optional Deno-specific flags
    "env": "object"        // Optional environment variables
  }
}
```

Server sends:
```json
{
  "type": "output|error|exit",
  "data": "string"
}
```

## Security Considerations
- Validate file extensions before upload
- Protect default source from modification
- Sanitize script paths to prevent directory traversal
- Limit file upload size
- Validate environment variable names
- Command-line arguments are passed directly to scripts (no sanitization)