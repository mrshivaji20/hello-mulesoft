# Hello MuleSoft Application

A MuleSoft application with HTTP listener, data masking functionality, and automated CloudHub deployment via GitHub Actions.

## Features

- **HTTP Listener**: Accepts requests on `/hello` endpoint
- **Data Masking**: Masks sensitive data (password, SSN, email) using DataWeave
- **Logging**: Logs both original and masked data
- **Automated Deployment**: CI/CD pipeline for CloudHub deployment

## Application Structure

```
src/
├── main/
│   ├── mule/
│   │   ├── hello-mulesoft.xml    # Main flow configuration
│   │   └── global.xml            # Global configurations
│   └── resources/
│       ├── config.properties     # Application properties
│       └── log4j2.xml           # Logging configuration
└── test/
    └── resources/               # Test resources
```

## CI/CD Pipeline

### GitHub Actions Workflow

The repository includes a GitHub Actions workflow (`.github/workflows/deploy.yml`) that automatically:

1. **Triggers** on every push to the `master` branch
2. **Builds** the application using Maven
3. **Deploys** to CloudHub using the Mule Maven Plugin

### Required Repository Secrets

To enable automatic deployment, configure these repository secrets:

- `ANYPOINT_CLIENT_ID`: Your Anypoint Platform Client ID
- `ANYPOINT_CLIENT_SECRET`: Your Anypoint Platform Client Secret

### Deployment Configuration

- **Environment**: Sandbox
- **Region**: us-east-2
- **Workers**: 1
- **Worker Type**: MICRO
- **Mule Version**: 4.11.3

## Local Development

### Prerequisites

- Java 17
- Maven 3.6+
- Anypoint Studio (optional)

### Building the Application

```bash
mvn clean package
```

### Running Locally

```bash
mvn mule:run
```

The application will be available at `http://localhost:8081/hello`

## API Usage

### Endpoint

```
GET http://localhost:8081/hello
```

### Response

The application returns masked sample data:

```json
{
  "name": "John Doe",
  "email": "***@***.com",
  "password": "*********",
  "details": {
    "ssn": "***-**-****"
  }
}
```

## Dependencies

- **Mule Runtime**: 4.11.2
- **HTTP Connector**: 1.11.1
- **Common DataWeave Library**: 1.0.0 (for data masking)

## Deployment

### Automatic Deployment

Every push to the `master` branch triggers automatic deployment to CloudHub.

### Manual Deployment

To deploy manually:

```bash
mvn deploy -DmuleDeploy \
  -Danypoint.username=YOUR_CLIENT_ID \
  -Danypoint.password=YOUR_CLIENT_SECRET \
  -DapplicationName=hello-mulesoft-app \
  -Denvironment=Sandbox \
  -Dregion=us-east-2 \
  -DmuleVersion=4.11.3 \
  -Dworkers=1 \
  -DworkerType=MICRO
```

## Contributing

1. Create a feature branch
2. Make your changes
3. Test locally
4. Create a pull request
5. Once merged to master, the application will automatically deploy

## License

This project is licensed under the MIT License.