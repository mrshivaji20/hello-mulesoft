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
2. **Authenticates** to Anypoint Exchange for private dependencies
3. **Builds** the application using Maven
4. **Deploys** to CloudHub using the Mule Maven Plugin

### Required Repository Secrets

To enable automatic deployment and private Exchange access, configure these repository secrets:

- `ANYPOINT_USERNAME`: Your Anypoint Platform username (for Exchange authentication)
- `ANYPOINT_PASSWORD`: Your Anypoint Platform password (for Exchange authentication)
- `ANYPOINT_CLIENT_ID`: Your Anypoint Platform Client ID (for CloudHub deployment)
- `ANYPOINT_CLIENT_SECRET`: Your Anypoint Platform Client Secret (for CloudHub deployment)

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
- Access to Anypoint Exchange for private dependencies

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
- **Common DataWeave Library**: 1.0.0 (private Exchange dependency for data masking)

## Authentication Setup

### For CI/CD Pipeline

The GitHub Actions workflow creates a Maven `settings.xml` file with authentication for:
- Anypoint Exchange (for downloading private dependencies)
- MuleSoft releases repository

### For Local Development

Create a `~/.m2/settings.xml` file with your Anypoint credentials:

```xml
<settings>
  <servers>
    <server>
      <id>anypoint-exchange-v3</id>
      <username>YOUR_ANYPOINT_USERNAME</username>
      <password>YOUR_ANYPOINT_PASSWORD</password>
    </server>
  </servers>
</settings>
```

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
3. Test locally (ensure you have Exchange access)
4. Create a pull request
5. Once merged to master, the application will automatically deploy

## Troubleshooting

### Build Failures

- **401 Unauthorized**: Check your Anypoint Exchange credentials in repository secrets
- **Dependency Resolution**: Ensure `ANYPOINT_USERNAME` and `ANYPOINT_PASSWORD` are correctly set
- **Deployment Failures**: Verify `ANYPOINT_CLIENT_ID` and `ANYPOINT_CLIENT_SECRET` are valid

## License

This project is licensed under the MIT License.