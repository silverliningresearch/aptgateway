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
