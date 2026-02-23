# Spring Boot API Documentation Extraction Prompt

Please analyze this Spring Boot project and generate comprehensive API documentation.

## Output Requirements

**IMPORTANT:** Write the analysis results directly to the following file:
```
{RESULT_DIR}/api.md
```

## Analysis Steps

Follow these steps for comprehensive analysis:

### 1. Project Basic Information Extraction

Extract project basic information from:
- `pom.xml` or `build.gradle` - Get project name, version, dependencies
- `application.yml` / `application.properties` - Get server port, base configuration
- `application.yml` / `application.properties` - Get context-path and other path configurations

### 2. Controller Scanning and Parsing

Scan all Controller classes (typically in `*Controller.java` files):

- Identify classes with `@RestController` or `@Controller` annotations
- Extract `@RequestMapping` class-level path prefix
- Parse mapping annotations on each method:
  - `@GetMapping`
  - `@PostMapping`
  - `@PutMapping`
  - `@DeleteMapping`
  - `@PatchMapping`
  - `@RequestMapping`

### 3. Endpoint Details Extraction

For each API endpoint, extract the following information:

- **HTTP Method**: GET/POST/PUT/DELETE/PATCH, etc.
- **Full Path**: Class-level path + Method-level path
- **Method Description**: Infer functionality from JavaDoc or method name
- **Request Parameters**:
  - `@RequestParam` - Query parameters
  - `@PathVariable` - Path parameters
  - `@RequestBody` - Request body
  - `@RequestHeader` - Request headers
- **Return Type**: Method return value type

### 4. Data Structure Analysis

Analyze the following data models:
- DTO classes (typically named `*DTO.java`, `*Request.java`, `*Response.java`)
- Entity classes (classes with `@Entity` annotation)
- Nested data structures

Generate JSON examples for each data structure.

### 5. Spring Boot Actuator Endpoints

If the project integrates Spring Boot Actuator, list standard endpoints:
- `/actuator/health` - Health check
- `/actuator/info` - Application info
- `/actuator/metrics` - Metrics information
- Other enabled endpoints

### 6. Security and Configuration

Identify and document:
- CORS configuration (`@CrossOrigin` or `CorsConfig` class)
- Authentication/Authorization mechanisms (Spring Security configuration)
- Global exception handlers

## Document Format Template

Generate the document in the following format:

```markdown
# {Service Name} API Documentation

## Overview

{Service Name} is {project description}, built with Spring Boot {version}.

**Service Information:**
- Service Name: {artifactId}
- Version: {version}
- Port: {server.port}
- Base URL: http://localhost:{port}{context-path}

## API Endpoints

### {ControllerName}

**File Location:** [relative path](relative path)

{Controller description}

| Method | Endpoint | Description | Parameters | Request Body | Response |
|--------|----------|-------------|------------|--------------|----------|
| {HTTP Method} | `{path}` | {description} | {parameter list} | {request body type} | {response type} |

#### Data Structure Examples

**{DataStructureName} ({description})**
```json
{
  "field1": "example value",
  "field2": "example value"
}
```

## Spring Boot Actuator Endpoints

{If applicable, list Actuator endpoints}

## CORS Configuration

{Document CORS related configuration}

## Security Configuration

{Document authentication and authorization configuration}

## Error Response Format

{Standard error response format}

## Usage Examples

{Provide curl examples for key APIs}

## Notes

{Important usage notes}

## Version History

- **{version}**: {version description}
```

## Important Notes

1. **Path Concatenation**: Properly concatenate class-level and method-level paths
2. **Parameter Details**: Distinguish between required and optional parameters, annotate parameter types
3. **Generic Handling**: For generic return types like `ResponseEntity<T>` or `Result<T>`, extract the actual data type
4. **Inheritance**: Handle cases where Controllers inherit from base classes
5. **Swagger/OpenAPI**: If the project uses annotations like `@ApiOperation`, prefer the descriptions from them
6. **Conditional Endpoints**: Specially mark endpoints that may be enabled based on configuration or profiles
