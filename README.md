# Trino ODBC Driver for .NET

A production-ready ODBC driver for connecting .NET applications to Trino (formerly PrestoSQL) clusters.

## Features

- Full ODBC 3.8 specification compliance
- Support for .NET Framework 4.8 and .NET 6.0+
- Multiple authentication methods (LDAP, JWT, Kerberos, OAuth)
- Connection pooling with configurable settings
- Complete ADO.NET implementation with Entity Framework Core and Dapper support
- Support for complex Trino data types (ARRAY, MAP, ROW)
- High-performance query execution with server-side cursors
- Comprehensive error handling and diagnostics

## Installation

```bash
Install-Package TrinoOdbc
```

## Quick Start

```csharp
// Create a connection
using var connection = new TrinoConnection("Host=trino-coordinator;Port=8080;Catalog=hive;Schema=default;User=admin;AuthType=None");
connection.Open();

// Execute a query
using var command = connection.CreateCommand();
command.CommandText = "SELECT * FROM orders WHERE order_date > @orderDate";

// Add parameters
var parameter = command.CreateParameter();
parameter.ParameterName = "@orderDate";
parameter.Value = new DateTime(2023, 1, 1);
command.Parameters.Add(parameter);

// Read results
using var reader = command.ExecuteReader();
while (reader.Read())
{
    int orderId = reader.GetInt32(0);
    decimal amount = reader.GetDecimal(1);
    // Process data...
}
```

## Authentication Examples

### LDAP Authentication

```csharp
var connString = "Host=trino-coordinator;Port=8080;Catalog=hive;User=username;Password=pwd;AuthType=LDAP";
```

### JWT Authentication

```csharp
var connString = "Host=trino-coordinator;Port=8080;Catalog=hive;User=username;AccessToken=your_jwt_token;AuthType=JWT";
```

## Performance Optimization

- Connection pooling is enabled by default
- For large result sets, consider setting `UseResultCaching=true`
- Set appropriate `CommandTimeout` values for long-running queries

## License

MIT License
