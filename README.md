# Airport Data Gateway Configuration

This repository contains the configuration switch for the airport data gateway system. It allows manual switching between the primary server and fallback GitHub repository data source.

## Configuration File

The `config.json` file controls which data source your GitHub Pages will use:

```json
{
  "mode": "primary",  // or "fallback"
  "primary": {
    "url": "http://85.9.202.121",
    "requiresToken": true
  },
  "fallback": {
    "url": "https://raw.githubusercontent.com/silverliningresearch/aptdata/main",
    "requiresToken": false
  }
}
```

## How to Use

### Accessing GitHub Pages with Token

Your GitHub Pages should be accessed with the token in the URL:

```
https://silverliningresearch.github.io/BRU_PS/DailyPlan.html?token=YOUR_TOKEN_HERE
```

The token will be automatically parsed from the URL and used for authentication with the primary server.

## How to Switch Modes

### Using Primary Server (Normal Operation)

1. Edit `config.json`
2. Set `"mode": "primary"`
3. Commit and push changes to GitHub
4. Access your GitHub Pages with token: `DailyPlan.html?token=YOUR_TOKEN`
5. Data will be fetched from http://85.9.202.121 with token authentication

### Using Fallback (When Server is Down)

1. **Ensure aptdata repository is public** and contains the required files (see structure below)
2. Edit `config.json`
3. Set `"mode": "fallback"`
4. Commit and push changes to GitHub
5. Access your GitHub Pages (no token needed): `DailyPlan.html`
6. Data will be fetched from the GitHub repository without requiring tokens

## Required aptdata Repository Structure

For fallback mode to work, your `aptdata` repository must contain the following files:

```
aptdata/
├── BRU_PROF/
│   ├── interview_statistics.js
│   └── data/
│       └── BRU_flight_list_daily.js
└── data_V2/
    ├── Quota_Data.js
    └── invalid_cases.js
```

**Important**: Copy the `data_V2/` folder from your BRU_PS repository to the aptdata repository to ensure all required data files are available in fallback mode.

## Important Notes

- **Primary mode**: Requires token in URL, fetches from your deployed server
- **Fallback mode**: No token required, fetches from public GitHub repository
- The switch is **manual** - you control when to change modes
- Token is parsed from URL query parameter: `?token=xxx`
- If token is required but not provided in URL, system automatically switches to fallback mode
- Remember to make the aptdata repository public before switching to fallback mode

## Deployment

1. Create a new public repository on GitHub: `silverliningresearch/aptgateway`
2. Push this directory to that repository:
   ```bash
   git init
   git add .
   git commit -m "Initial configuration"
   git remote add origin git@github.com:silverliningresearch/aptgateway.git
   git branch -M main
   git push -u origin main
   ```
3. The config will be accessible at:
   `https://raw.githubusercontent.com/silverliningresearch/aptgateway/main/config.json`

## Usage in GitHub Pages

Your GitHub Pages HTML files will:
1. Fetch this config file first
2. Parse token from URL (?token=xxx)
3. Use the appropriate data source based on the mode setting
4. Automatically handle token authentication for primary mode
