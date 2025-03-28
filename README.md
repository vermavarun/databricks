# databricks

## Access types with ADLS

- Using access keys
  ```
    spark.conf.set(
    "fs.azure.account.key.<storage-account-name>.dfs.core.windows.net",
    "")

    display(dbutils.fs.ls("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net"))

    display(spark.read.csv("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/data.csv"))

  ```
- Using SAS tokens
    ```
    spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "SAS")

    spark.conf.set("fs.azure.sas.token.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.sas.FixedSASTokenProvider")
    spark.conf.set("fs.azure.sas.fixed.token.<storage-account-name>.dfs.core.windows.net", "<sas-token>")


    display(dbutils.fs.ls("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net"))

    display(spark.read.csv("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/data.csv"))

    ```


- Using Service Principal
  ```
    client_id = ""
    tenant_id = ""
    client_secret = ""

    spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
    spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
    spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", client_id)
    spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", client_secret)
    spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", f"https://login.microsoftonline.com/{tenant_id}/oauth2/token")

    display(dbutils.fs.ls("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net"))

    display(spark.read.csv("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/data.csv"))

    ```
- Using Cluster Scoped Credentials
    ```
    display(dbutils.fs.ls("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net"))


    display(spark.read.csv("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/data.csv"))

    ```
- Pass-through (Azure Active Directory) or no credentials
    ```
    Just check the check box in cluster configuration and give your user the required permissions to the storage account using iam roles.
    ```


## Secrets Management

### Databricks backed secrets

- Go to home page of databricks workspace
- visit url {adb*****.net/?o=****#}secrets/createScope
- Create a new secret scope


### Azure Key Vault backed secrets (⚙️Recommended)

#### Add secrets to Azure Key Vault
#### Create databricks secret scope
#### Get secrets using ``` dbutils.secret.get```

### Usage

```
dbutils.secrets.help()


dbutils.secrets.listScopes()


dbutils.secrets.list(scope = '<secret-scope-name>')


dbutils.secrets.get(scope = '<secret-scope-name>', key = '<azure-keyvault-secret-name>')

````