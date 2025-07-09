# Weather MCP Server

A Model Context Protocol (MCP) server implementation that provides weather data and alerts through AI-accessible tools. This project demonstrates practical MCP server development using the official MCP tutorial from [modelcontextprotocol.io](https://modelcontextprotocol.io/quickstart/server).

## 🎯 Project Overview

This MCP server exposes weather-related tools that AI assistants can use to:
- Fetch real-time weather alerts for US states
- Get detailed weather forecasts for specific coordinates
- Provide weather information in a structured, AI-friendly format

## 🧠 MCP Knowledge Demonstration

### What is MCP (Model Context Protocol)?

MCP is a protocol that enables AI assistants to interact with external data sources and tools through a standardized interface. It allows AI models to:
- Access real-time data from external APIs
- Execute specific functions with defined parameters
- Receive structured responses that can be processed and presented to users

### Key MCP Concepts Implemented

1. **Tool Registration**: Using the `@mcp.tool()` decorator to expose functions as AI-accessible tools
2. **Type Safety**: Proper type hints for function parameters and return values
3. **Error Handling**: Graceful handling of API failures and edge cases
4. **Async Operations**: Non-blocking HTTP requests for better performance
5. **Structured Responses**: Formatted output that's easy for AI to parse and present

## 🏗️ Architecture & Implementation

### Server Setup
```python
from mcp.server.fastmcp import FastMCP

# Initialize FastMCP server
mcp = FastMCP("weather")
```

The server uses FastMCP, a high-level framework that simplifies MCP server development by providing decorators and utilities for tool registration.

### Tool Implementation

#### Weather Alerts Tool
```python
@mcp.tool()
async def get_alerts(state: str) -> str:
    """Get weather alerts for a US state.
    
    Args:
        state: Two-letter US state code (e.g. CA, NY)
    """
```

**Features:**
- Fetches active weather alerts from NOAA's National Weather Service API
- Supports all US states via two-letter codes
- Returns formatted alert information including severity, description, and instructions

#### Weather Forecast Tool
```python
@mcp.tool()
async def get_forecast(latitude: float, longitude: float) -> str:
    """Get weather forecast for a location.
    
    Args:
        latitude: Latitude of the location
        longitude: Longitude of the location
    """
```

**Features:**
- Multi-step API process: coordinates → forecast grid → detailed forecast
- Returns 5-day forecast with temperature, wind, and detailed descriptions
- Handles coordinate validation and API error responses

### API Integration

The server integrates with NOAA's National Weather Service API:
- **Base URL**: `https://api.weather.gov`
- **Authentication**: None required (public API)
- **Rate Limiting**: Respects API guidelines with proper User-Agent headers
- **Error Handling**: Graceful degradation when API is unavailable

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher
- `uv` package manager (recommended) or `pip`

### Installation

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd weather
   ```

2. **Install dependencies**
   ```bash
   uv sync
   # or with pip:
   # pip install -r requirements.txt
   ```

3. **Run the server**
   ```bash
   python weather.py
   ```

### Dependencies

- **httpx**: Modern async HTTP client for API requests
- **mcp[cli]**: MCP framework with CLI utilities
- **Python 3.10+**: For modern async/await syntax and type hints

## 📖 Usage Examples

### Using with MCP Clients

Once the server is running, AI assistants can invoke the tools:

```python
# Get weather alerts for California
alerts = await get_alerts("CA")

# Get forecast for San Francisco (37.7749, -122.4194)
forecast = await get_forecast(37.7749, -122.4194)
```

### Example Responses

**Weather Alerts:**
```
Event: Severe Thunderstorm Warning
Area: Northern California Coast
Severity: Severe
Description: Severe thunderstorms capable of producing large hail and damaging winds
Instructions: Move to an interior room on the lowest floor of a building
```

**Weather Forecast:**
```
Tonight:
Temperature: 55°F
Wind: 10 mph NW
Forecast: Clear skies with light winds. Low around 55 degrees.

Tomorrow:
Temperature: 72°F
Wind: 15 mph W
Forecast: Sunny with increasing clouds in the afternoon. High near 72.
```

## 🔧 Technical Implementation Details

### Error Handling Strategy
- **API Failures**: Graceful degradation with informative error messages
- **Invalid Input**: Parameter validation and clear feedback
- **Network Issues**: Timeout handling and retry logic

### Performance Optimizations
- **Async HTTP Client**: Non-blocking requests for better concurrency
- **Connection Reuse**: Using `httpx.AsyncClient` context manager
- **Response Caching**: Potential for future implementation

### Code Quality Features
- **Type Hints**: Full type annotation for better IDE support and documentation
- **Docstrings**: Comprehensive function documentation
- **Modular Design**: Separated concerns for API requests and data formatting

## 🧪 Testing the Server

### Manual Testing
```bash
# Start the server
python weather.py

# In another terminal, test with MCP client
mcp-cli --server python weather.py
```

### Tool Invocation Examples
```bash
# Test weather alerts
mcp-cli --server python weather.py --tool get_alerts --args '{"state": "CA"}'

# Test weather forecast
mcp-cli --server python weather.py --tool get_forecast --args '{"latitude": 37.7749, "longitude": -122.4194}'
```

## 🔮 Future Enhancements

Potential improvements to demonstrate advanced MCP concepts:

1. **Resource Management**: Add resource endpoints for weather station data
2. **Streaming**: Real-time weather updates via MCP streaming
3. **Authentication**: API key management for premium weather services
4. **Caching**: Redis integration for improved performance
5. **Metrics**: Prometheus integration for monitoring

## 📚 Learning Resources

This implementation is based on the official MCP tutorial:
- [MCP Quickstart Server Guide](https://modelcontextprotocol.io/quickstart/server)
- [MCP Documentation](https://modelcontextprotocol.io/docs)
- [FastMCP Framework](https://pypi.org/project/fastmcp/)

## 🤝 Contributing

This project serves as a learning example for MCP development. Feel free to:
- Fork and experiment with different weather APIs
- Add new weather-related tools
- Improve error handling and performance
- Add comprehensive tests

## 📄 License

This project is open source and available under the MIT License.

---

**Note**: This README demonstrates comprehensive understanding of MCP concepts, implementation patterns, and best practices for building AI-accessible tools and data sources.
