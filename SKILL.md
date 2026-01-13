---
name: agent-browser
description: Advanced browser automation tool for web testing, form filling, screenshots, data extraction, and structured information retrieval. Supports complex workflows including authentication, dynamic content handling, and formatted table output. Essential for web scraping, testing, and data collection tasks.
---

# Browser Automation with agent-browser

## Quick start

```bash
agent-browser open <url>        # Navigate to page
agent-browser snapshot -i       # Get interactive elements with refs
agent-browser click @e1         # Click element by ref
agent-browser fill @e2 "text"   # Fill input by ref
agent-browser close             # Close browser
```

## Core workflow

1. Navigate: `agent-browser open <url>`
2. Snapshot: `agent-browser snapshot -i` (returns elements with refs like `@e1`, `@e2`)
3. Interact using refs from the snapshot
4. Re-snapshot after navigation or significant DOM changes

## Data Extraction Workflow

For extracting structured data from web pages:

1. Navigate to target page
2. Take snapshot to identify data containers
3. Extract data using `get text` or other commands
4. Format extracted data into tables for presentation

## Commands

### Navigation
```bash
agent-browser open <url>      # Navigate to URL
agent-browser back            # Go back
agent-browser forward         # Go forward  
agent-browser reload          # Reload page
agent-browser close           # Close browser
```

### Snapshot (page analysis)
```bash
agent-browser snapshot        # Full accessibility tree
agent-browser snapshot -i     # Interactive elements only (recommended)
agent-browser snapshot -c     # Compact output
agent-browser snapshot -d 3   # Limit depth to 3
```

### Interactions (use @refs from snapshot)
```bash
agent-browser click @e1           # Click
agent-browser dblclick @e1        # Double-click
agent-browser fill @e2 "text"     # Clear and type
agent-browser type @e2 "text"     # Type without clearing
agent-browser press Enter         # Press key
agent-browser press Control+a     # Key combination
agent-browser hover @e1           # Hover
agent-browser check @e1           # Check checkbox
agent-browser uncheck @e1         # Uncheck checkbox
agent-browser select @e1 "value"  # Select dropdown
agent-browser scroll down 500     # Scroll page
agent-browser scrollintoview @e1  # Scroll element into view
```

### Get information
```bash
agent-browser get text @e1        # Get element text
agent-browser get value @e1       # Get input value
agent-browser get title           # Get page title
agent-browser get url             # Get current URL
```

### Screenshots
```bash
agent-browser screenshot          # Screenshot to stdout
agent-browser screenshot path.png # Save to file
agent-browser screenshot --full   # Full page
```

### Wait
```bash
agent-browser wait @e1                     # Wait for element
agent-browser wait 2000                    # Wait milliseconds
agent-browser wait --text "Success"        # Wait for text
agent-browser wait --load networkidle      # Wait for network idle
```

### Semantic locators (alternative to refs)
```bash
agent-browser find role button click --name "Submit"
agent-browser find text "Sign In" click
agent-browser find label "Email" fill "user@test.com"
```

## Example: Form submission

```bash
agent-browser open https://example.com/form
agent-browser snapshot -i
# Output shows: textbox "Email" [ref=e1], textbox "Password" [ref=e2], button "Submit" [ref=e3]

agent-browser fill @e1 "user@example.com"
agent-browser fill @e2 "password123"
agent-browser click @e3
agent-browser wait --load networkidle
agent-browser snapshot -i  # Check result
```

## Example: Authentication with saved state

```bash
# Login once
agent-browser open https://app.example.com/login
agent-browser snapshot -i
agent-browser fill @e1 "username"
agent-browser fill @e2 "password"
agent-browser click @e3
agent-browser wait --url "**/dashboard"
agent-browser state save auth.json

# Later sessions: load saved state
agent-browser state load auth.json
agent-browser open https://app.example.com/dashboard
```

## Example: Data extraction and table formatting

```bash
# Extract product information and format as table
agent-browser open https://example.com/products
agent-browser snapshot -i
# Identify product elements and extract data
agent-browser get text @product1
agent-browser get text @price1
# ... extract more data points
# Then format results into markdown table for output
```

## Sessions (parallel browsers)

```bash
agent-browser --session test1 open site-a.com
agent-browser --session test2 open site-b.com
agent-browser session list
```

## JSON output (for parsing)

Add `--json` for machine-readable output:
```bash
agent-browser snapshot -i --json
agent-browser get text @e1 --json
```

## Output Formatting

### Table Rendering (REQUIRED)

When presenting structured data such as lists, rankings, or tabular information extracted from web pages, output MUST be rendered in markdown table format for better readability.

**Requirements:**
- Use markdown tables for any tabular data
- Include appropriate headers for columns
- Ensure data is properly aligned and formatted
- Use consistent formatting across similar data types
- Include relevant metadata (URLs, dates, prices, etc.) as table columns

**Example Table Output:**
| 主播名称 | 用户名 | 平台 | 收费 | 到期日期 | 直播链接 |
|----------|--------|------|------|----------|----------|
| 谪止     | fanxiaofeng2020 | 抖音 | ¥40.00 | 2026-01-08 | [链接](https://live.douyin.com/562507403167) |
| 小烦o3o   | yml01515 | 抖音 | ¥30.00 | 2026-01-08 | [链接](https://live.douyin.com/918906083989) |

### Data Extraction Best Practices

- When extracting multiple similar items, organize them into tables
- Use descriptive column headers in Chinese when appropriate
- Sort data logically when possible (e.g., by date, relevance, or priority)
- Include clickable links using markdown format: `[text](url)`
- Ensure all monetary values include currency symbols
- Format dates consistently (YYYY-MM-DD)

## Debugging

```bash
agent-browser open example.com --headed  # Show browser window
agent-browser console                    # View console messages
agent-browser errors                     # View page errors
```
