# Dev Container with Cosmos DB Emulator

This dev container is configured with the Azure Cosmos DB Linux Emulator running alongside your development environment.

## What's Included

- **Cosmos DB Emulator**: Runs on `https://localhost:8081`
- **Auto-start**: The emulator starts automatically when you rebuild the container
- **Default Configuration**: Pre-configured in `CosmosDBConnector/appsettings.Development.json`

## Using the Emulator

### Connection Details
- **Endpoint**: `https://localhost:8081`
- **Account Key**: `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`

### Running the CosmosDB Connector

The connector is already configured to use the emulator by default:

```bash
cd CosmosDBConnector
dotnet run
```

### Testing Different Connection Methods

Edit `appsettings.Development.json` to test different methods:

```json
"ConnectionMethod": "Emulator"      // Test emulator only
"ConnectionMethod": "TestAll"       // Test all configured methods
"ConnectionMethod": "ManagedIdentity" // Test specific method
```

### Verifying the Emulator

Check if the emulator is running:

```bash
curl -k https://localhost:8081/_explorer/index.html
```

Or check container logs:

```bash
docker logs cosmosdb-emulator
```

## Troubleshooting

### Emulator Not Starting

If the emulator fails to start:

1. **Check memory**: The emulator requires at least 2GB of RAM
2. **View logs**: `docker logs cosmosdb-emulator`
3. **Restart**: Rebuild the dev container

### Connection Issues

If you can't connect to the emulator:

1. **Wait for startup**: The emulator can take 30-60 seconds to fully initialize
2. **Check status**: Visit `https://localhost:8081/_explorer/index.html` in your browser
3. **Verify configuration**: Ensure `ConnectionMethod` is set to `"Emulator"` in appsettings

### Port Conflicts

If port 8081 is already in use:

1. Stop any other Cosmos DB emulator instances
2. Check what's using the port: `lsof -i :8081`
3. Rebuild the dev container

## Switching to Azure Cosmos DB

To test against a real Azure Cosmos DB account:

1. Update `appsettings.Development.json` with your Azure Cosmos DB details
2. Set `ConnectionMethod` to `"ManagedIdentity"`, `"AccountKey"`, or `"ConnectionString"`
3. Configure the appropriate authentication values

## Performance Notes

- The emulator uses **Gateway mode** (slower) vs **Direct mode** on Azure
- Expect 2-3x slower performance compared to Azure Cosmos DB
- Perfect for development and unit testing
- Not suitable for performance benchmarking
