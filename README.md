# Trino Community Helm Chart

This Helm chart installs Trino, an open-source distributed SQL query engine.

## Prerequisites

- Kubernetes 1.10+ with Beta APIs enabled
- Helm 3.x installed

## Installation

To install Trino using this Helm chart, follow these steps:

Install the Helm repository in your cluster:
   ```bash
    helm upgrade -i trino-community https://raw.githubusercontent.com/dview-io/trino-community/main/charts/trino-community-0.1.0.tgz -f values.yaml
   ```
## Configurations
A simple Values.yaml which setups Trino with mysql connector and glue catalog connector and Ranger-Admin with no openldap and ranger-usersync
MySQL Catalog Configuration:

### Configures a MySQL connector for Trino.
Uses environment variables or default values for the MySQL host, username, and password.
Example:
```yaml
trino:
  additionalCatalogs:
    mysql.properties: |-
        connector.name=mysql
        connection-url=jdbc:mysql://${env:MYSQL_HOST:-"localhost"}:3306
        connection-user=${env:MYSQL_USER:-"root"}
        connection-password=${env:MYSQL_PASSWORD:-"password"}
```

### AWS Glue Catalog Configuration:
Sets up a Hive metastore connector using AWS Glue.
Configures parameters for AWS Glue such as region, catalog ID, access key, and secret key.
Uses environment variables or default values.

```yaml
trino:
  additionalCatalogs:
    glue.properties: |-
      connector.name=hive
      hive.metastore=glue
      hive.metastore.glue.region=${env:GLUE_REGION:-"us-east-1"}
      hive.metastore.glue.catalogid=${env:GLUE_CATALOG_ID:-"default-glue-catalog-id"}
      hive.metastore.glue.access-key=${env:GLUE_ACCESS_KEY:-"default-access-key"}
      hive.metastore.glue.secret-key=${env:GLUE_SECRET_KEY:-"default-secret-key"}
      # Optional properties
      # hive.metastore.glue.endpoint-url=custom-glue-endpoint-url
```

## Environment Variables
Defines environment variables to be used in the Trino deployment.Utilizes Kubernetes secrets for secure storage of sensitive information.
```yaml
trino:
  env:
    - name: MYSQL_HOST
      valueFrom:
        secretKeyRef:
          key: MYSQL_HOST
          name: mysql-secret
    - name: GLUE_ACCESS_KEY
      valueFrom:
        secretKeyRef:
          key: GLUE_ACCESS_KEY
          name: glue-secrets 
      # ... (other environment variables omitted for brevity)
```
You can view a sample `values.yaml` file [here](values.yaml).
