---
title: "Impala ODBC, Dotnet Core, and Docker"
date: 2020-07-31T20:44:16Z
draft: false
---

This post will show how to query an Impala database from a DotNet Core application running in Docker.

## Prerequisites

Knowledge of Apache Hadoop, Docker, and DotNet Core

## Impala

[Apache Impala](https://impala.apache.org/index.html) is the open source, native analytic database for Apache Hadoop.

[Cloudera](https://www.cloudera.com/) provides an ODBC driver for Impala that can be [downloaded from their website](https://www.cloudera.com/downloads.html) (requires an account).

## Installing the Impala ODBC driver

Assuming you have downloaded the Cloudera Impala ODBC driver (in this case, I have downloaded it to: `clouderaimpalaodbc_2.6.10.1010-2_amd64.deb`), here is a script to install and configure it on a Linux system (for example, on a Docker image as shown below).

```bash
#!/usr/bin/env bash

apt-get update && apt-get install -y unixodbc krb5-user

dpkg -i clouderaimpalaodbc_2.6.10.1010-2_amd64.deb

echo "[ODBC Drivers]" > /etc/odbcinst.ini \
    && echo "Cloudera ODBC Driver for Impala=Installed" >> /etc/odbcinst.ini \
    && echo "[Cloudera ODBC Driver for Impala]" >> /etc/odbcinst.ini \
    && echo "Driver=/opt/cloudera/impalaodbc/lib/64/libclouderaimpalaodbc64.so" >> /etc/odbcinst.ini

echo "[Driver]" > /opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini \
    && echo "DriverManagerEncoding=UTF-16" >> /opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini \
    && echo "ErrorMessagesPath=/opt/cloudera/impalaodbc/ErrorMessages/" >> /opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini \
    && echo "LogLevel=0" >> /opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini \
    && echo "LogPath=/app/odbc_logs" >> /opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini \
    && echo "SwapFilePath=/tmp" >> /opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini

export ODBCSYSINI=/etc
export ODBCINI=/etc/odbc.ini
export CLOUDERAIMPALAODBCINI=/opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini
export LD_LIBRARY_PATH=/opt/cloudera/impalaodbc/lib/64
```

## Using the Impala ODBC driver

Add the odbc package

```shell
> dotnet add package System.Data.Odbc
```

Use it just like any other ODBC connection

```csharp
// notice that the Driver name matches the /etc/odbcinst.ini file
var connectionString = $"Driver=Cloudera ODBC Driver for Impala;Host={Host};Port=21050;UID={UserId};";
// we can either a kerberos keytab file or a password for auth
if (!string.IsNullOrWhiteSpace(KeyFile)) 
    connectionString += $"AuthMech=1;UseKeytab=1;DefaultKeytabFile={KeyFile};";
else 
    connectionString += $"AuthMech=3;UseSASL=1;PWD={Password};";

var sql = "select first_name from names;";

using (var connection = new OdbcConnection(connectionString))
{
    connection.Open();
    using (var command = new OdbcCommand(sql, connection))
    {
        using (var reader = command.ExecuteReader())
        {
            while(reader.Read()) 
                Console.WriteLine($"first name: {reader.GetString(0)}");
        }
    }
}
```

## Building a docker image

Next, we'll build a docker image for our application with the Impala ODBC driver installed.

Contents of `Dockerfile`:

```dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY MyApi.csproj ./
RUN dotnet restore

# copy everything else and build app
COPY /. ./
WORKDIR /app
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS runtime
WORKDIR /app
COPY --from=build /app/out ./
# install the ODBC driver using the script from above
COPY clouderaimpalaodbc_2.6.10.1010-2_amd64.deb ./
COPY install_odbc.sh ./

# Configure apt
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get -y upgrade && apt-get -y install --no-install-recommends apt-utils 2>&1

# set up odbc
RUN ./install_odbc.sh

# Clean up
RUN apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*
ENV DEBIAN_FRONTEND=dialog

ENTRYPOINT ["dotnet", "MyApi.dll"]
```

Build the image:

```bash
docker build --rm -t myapi .
```

## Running the docker container

Running the application is simple

```bash
docker run \
    --name myapi \
    --rm -d -p 5000:80 myapi
```
