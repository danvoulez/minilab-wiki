# LogLine Parser & Serializer Documentation

## Overview

LogLine is a whitespace-aware parser for structured logging and tracing data. It provides a human-readable format for defining operations, contexts, spans, and events with support for nested structures, references, function calls, and interpolations.

## Syntax Guide

### Basic Structure

```
OPERATION: operation_name
  id: unique_id
  template: description template
  param_name: value
END
```

### Block Types (Keywords)

- `OPERATION` - Represents an operational unit
- `CONTEXT` - Defines execution context
- `SPAN` - Tracing span
- `EVENT` - Discrete event
- `TRACE` - Trace definition
- `PACT` - Agreement/contract
- `AGENT` - Agent definition

### Special Parameters

- `id` - Unique identifier
- `trace_id` - Trace identifier for correlation
- `parent_id` - Parent span/operation reference
- `template` - String template for the block

### Value Types

#### 1. Strings

```
# Quoted strings (required for special characters)
name: "John Doe"
description: "Contains: special, characters"

# Unquoted strings (simple values)
status: active
type: user
```

#### 2. Numbers

```
count: 42
price: 19.99
temperature: -5.5
```

#### 3. Booleans

```
enabled: true
debug: false
```

#### 4. Null

```
optional_field: null
```

#### 5. Arrays

```
tags: [production, critical, verified]
scores: [95, 87, 92]
mixed: ["text", 123, true, null]
```

#### 6. References

References start with `@` and support dot-notation paths:

```
user: @current_user
profile: @user.profile
email: @user.contact.email
```

#### 7. Function Calls

```
timestamp: now()
formatted: format("Hello %s", @user.name)
calculated: sum(10, 20, 30)
```

#### 8. Interpolations

```
message: template{user_id}
path: resource{item_type}
```

## Complete Examples

### Example 1: Simple Operation

```
OPERATION: UserLogin
  id: op_12345
  user_id: @session.user
  ip_address: "192.168.1.100"
  successful: true
  timestamp: now()
END
```

**Parsed JSON:**

```json
{
  "type": "operation",
  "name": "UserLogin",
  "id": "op_12345",
  "params": {
    "user_id": {
      "type": "reference",
      "value": "@session.user",
      "path": ["session", "user"]
    },
    "ip_address": "192.168.1.100",
    "successful": true,
    "timestamp": {
      "type": "function_call",
      "name": "now",
      "args": []
    }
  }
}
```

### Example 2: Nested Structure

```
CONTEXT: RequestProcessing
  trace_id: trace_789
  request_id: req_456
  
  OPERATION: ValidateInput
    id: op_001
    schema: user_registration
    valid: true
  END
  
  OPERATION: ProcessData
    id: op_002
    parent_id: op_001
    records_processed: 150
    duration_ms: 245.7
  END
  
  EVENT: ProcessingComplete
    timestamp: now()
    status: success
  END
END
```

### Example 3: Complex Data Types

```
OPERATION: DataAnalysis
  id: analytics_001
  template: Analyzing data for {entity}
  
  # Array of tags
  tags: [analytics, ml, production]
  
  # Nested references
  dataset: @workspace.datasets.primary
  model: @models.trained.latest
  
  # Function calls with arguments
  started_at: timestamp()
  formatted_date: format_date(@analysis.start_time, "YYYY-MM-DD")
  
  # Interpolation
  report_path: reports{analysis_id}
  
  # Mixed array
  thresholds: [0.85, 0.90, 0.95]
  
  # Complex values
  config: { model: "gpt-4", temp: 0.7 }
END
```

### Example 4: Multi-level Nesting

```
TRACE: ServiceRequest
  trace_id: trace_2024_001
  service: api_gateway
  
  SPAN: IncomingRequest
    id: span_001
    endpoint: "/api/users"
    method: POST
    
    SPAN: Authentication
      id: span_002
      parent_id: span_001
      provider: oauth2
      validated: true
    END
    
    SPAN: DataValidation
      id: span_003
      parent_id: span_001
      schema_version: "2.1"
      errors: []
    END
    
    SPAN: DatabaseQuery
      id: span_004
      parent_id: span_001
      query_type: INSERT
      duration_ms: 23.4
      
      EVENT: QueryStart
        timestamp: 1234567890
      END
      
      EVENT: QueryComplete
        timestamp: 1234567913
        rows_affected: 1
      END
    END
  END
END
```

## API Reference

### Parsing Functions

#### `parseLogLineBlock(text: string): LogLineSpan`

Parses LogLine text and returns a span or throws an error.

```typescript
import { parseLogLineBlock } from './logline-parser';

try {
  const span = parseLogLineBlock(`
    OPERATION: TestOp
      status: active
    END
  `);
  console.log(span.type); // "operation"
} catch (error) {
  console.error('Parse failed:', error.message);
}
```

#### `parseLogLineBlockSafe(text: string): ParseResult`

Safely parses LogLine text and returns errors/warnings without throwing.

```typescript
import { parseLogLineBlockSafe } from './logline-parser';

const result = parseLogLineBlockSafe(loglineText);

if (result.errors.length > 0) {
  console.error('Errors:', result.errors);
}

if (result.warnings.length > 0) {
  console.warn('Warnings:', result.warnings);
}

if (result.span) {
  // Process parsed span
}
```

### Serialization Functions

#### `serializeToJsonAtomic(span: LogLineSpan, pretty?: boolean): string`

Converts a span to JSON format.

```typescript
import { serializeToJsonAtomic } from './logline-parser';

const json = serializeToJsonAtomic(span, true); // pretty-printed
console.log(json);
```

#### `serializeToLogLine(span: LogLineSpan, indent?: number): string`

Converts a span back to LogLine format.

```typescript
import { serializeToLogLine } from './logline-parser';

const logline = serializeToLogLine(span);
console.log(logline);
```

### Utility Functions

#### `walkSpans(span: LogLineSpan, visitor: Function, depth?: number): void`

Traverse all spans in the tree.

```typescript
import { walkSpans } from './logline-parser';

walkSpans(rootSpan, (span, depth) => {
  console.log(`${'  '.repeat(depth)}${span.type}: ${span.name}`);
});
```

#### `findSpan(span: LogLineSpan, predicate: Function): LogLineSpan | undefined`

Find a specific span matching criteria.

```typescript
import { findSpan } from './logline-parser';

const found = findSpan(rootSpan, s => s.id === 'op_123');
if (found) {
  console.log('Found:', found.name);
}
```

#### `transformSpan(span: LogLineSpan, transformer: Function): LogLineSpan`

Transform spans recursively.

```typescript
import { transformSpan } from './logline-parser';

const redacted = transformSpan(span, s => ({
  ...s,
  params: s.params ? redactSensitive(s.params) : undefined
}));
```

## Error Handling

The parser provides detailed error information:

```typescript
interface ParseError {
  line: number;
  column?: number;
  message: string;
  source?: string;
}
```

Example error output:

```
Parse error at line 5: Expected colon after block type, got IDENTIFIER "value"
```

## Advanced Features

### Comments

```
# This is a comment
OPERATION: MyOp  # inline comment
  # Comments are ignored
  status: active
END
```

### Whitespace Preservation

The parser preserves exact whitespace in raw string values, maintaining formatting for multi-word descriptions and special content.

### Escape Sequences

Strings support standard escape sequences:

- `\n` - newline
- `\t` - tab
- `\r` - carriage return
- `\"` - quote
- `\\` - backslash

```
message: "Line 1\nLine 2\tTabbed"
```

## Type Definitions

```typescript
type LogLineValue =
  | string
  | number
  | boolean
  | null
  | LogLineReference
  | LogLineFunctionCall
  | LogLineInterpolation
  | LogLineValue[];

interface LogLineSpan {
  type: string;
  name?: string;
  id?: string;
  params?: Record<string, LogLineValue>;
  template?: string;
  trace_id?: string;
  parent_id?: string;
  children?: LogLineSpan[];
  meta?: Record<string, unknown>;
}
```

## Best Practices

1. **Use meaningful IDs**: Always provide unique identifiers for operations and spans
1. **Leverage nesting**: Use hierarchical structures to represent parent-child relationships
1. **Reference data**: Use `@references` to link related data without duplication
1. **Quote special strings**: Always quote strings containing colons, commas, or brackets
1. **Consistent naming**: Use consistent naming conventions for parameters
1. **Comments for clarity**: Add comments to explain complex structures
1. **Error handling**: Always use `parseLogLineBlockSafe` in production code

## Performance Considerations

- Parser uses efficient tokenization with single-pass parsing
- Preserves source positions for accurate error reporting
- Handles large nested structures efficiently
- Memory-efficient token representation