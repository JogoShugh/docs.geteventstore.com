---
title: "Projections Management"
section: ".NET API"
version: "4.0.2"
---

The Event Store Client API includes helper methods that use the HTTP API to allow you to manage projections. This document will describe the methods found in the `ProjectionsManager` class. All methods in this class are asynchronous.

## Methods

### Enable a Projection

Enables an existing projection by its name. You must have access to a projection to enable it.

```csharp
public Task EnableAsync(string name, UserCredentials userCredentials = null)
```

### Disable a Projection

Disables an existing projection by its name. You must have access to a projection to disable it.

```csharp
public Task DisableAsync(string name, UserCredentials userCredentials = null)
```

### Abort a Projection

Aborts an existing projection by its name. You must have access to a projection to abort it.

```csharp
public Task AbortAsync(string name, UserCredentials userCredentials = null)
```

### Create a One-Time Projection

Creates a projection that will run until the end of the log and then stop. The query parameter contains the javascript you want created as a one time projection.

```csharp
public Task CreateOneTimeAsync(string query, UserCredentials userCredentials = null)
```

### Create a Continuous Projection

Creates a projection that will run until the end of the log and then continue running. The query parameter contains the javascript you want created as a one time projection. Continuous projections have explicit names and you can enable or disable them via this name.

```csharp
public Task CreateContinuousAsync(string name, string query, UserCredentials userCredentials = null)
```

### List all Projections

Returns a list of all the projections.

```csharp
public Task<List<ProjectionDetails>> ListAllAsync(UserCredentials userCredentials = null)
```

### List One-Time Projections

Returns a list of all One-Time Projections.

```csharp
public Task<List<ProjectionDetails>> ListOneTimeAsync(UserCredentials userCredentials = null)
```

### Get Statistics on Projection

Returns the statistics associated with a named projection.

```csharp
public Task<string> GetStatisticsAsync(string name, UserCredentials userCredentials = null)
```

### Delete Projection

Deletes a named projection. You must have access to a projection to delete it.

```csharp
public Task DeleteAsync(string name, UserCredentials userCredentials = null)
```

### Get State

Retrieves the state of the projection.

```csharp
public Task<string> GetState(string name, UserCredentials userCredentials = null)
```

### Get Partition State

Retrieves the state of the projection via the given partition.

```csharp
public Task<string> GetPartitionStateAsync(string name, string partition, UserCredentials userCredentials = null)
```

### Get Result

Retrieves the result of the projection.

```csharp
public Task<string> GetResult(string name, UserCredentials userCredentials = null)
```

### Get Partition Result

Retrieves the result of the projection via the given partition.

```csharp
public Task<string> GetPartitionResultAsync(string name, string partition, UserCredentials userCredentials = null)
```
