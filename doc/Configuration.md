# Configuration Guide <br/> Beacons Microservice

Configuration structure used by this module follows the 
[standard configuration] https://github.com/pip-services/pip-services/blob/master/usage/Configuration.md 
structure.

Example **config.yml** file:

```yaml
---
  # Container descriptor
  - descriptor: "pip-services:context-info:default:default:1.0"
    name: "beacons"
    description: "Beacons microservice"
  
  # Console logger
  - descriptor: "pip-services:logger:console:default:1.0"
    level: "trace"
  
  # Perfomance counter that post values to log
  - descriptor: "pip-services:counters:log:default:1.0"
  
  {{^if MONGO_ENABLED}}{{^if FILE_ENABLED}}
  # Memory persistence
  - descriptor: "beacons:persistence:memory:default:1.0"
  {{/if}}{{/if}}
  
  {{#if FILE_ENABLED}}
  # File persistence
  - descriptor: "beacons:persistence:file:default:1.0"
    path: {{FILE_PATH}}{{^if FILE_PATH}}"./data/beacons.json"{{/if}}
  {{/if}}
  
  {{#if MONGO_ENABLED}}
  # MongoDb persistence
  - descriptor: "beacons:persistence:mongodb:default:1.0"
    connection:
      uri: {{MONGO_SERVICE_URI}}
      host: {{MONGO_SERVICE_HOST}}{{^if MONGO_SERVICE_HOST}}"localhost"{{/if}}
      port: {{MONGO_SERVICE_PORT}}{{^if MONGO_SERVICE_PORT}}27017{{/if}}
      database: {{MONGO_DB}}{{^if MONGO_DB}}"test"{{/if}}
  {{/if}}
  
  {{#if COUCHBASE_ENABLED}}
    # Couchbase persistence
    - descriptor: "beacons:persistence:couchbase:default:1.0"
      connection:
        uri: {{COUCHBASE_SERVICE_URI}}
        host: {{COUCHBASE_SERVICE_HOST}}{{^if COUCHBASE_SERVICE_HOST}}"localhost"{{/if}}
        port: {{COUCHBASE_SERVICE_PORT}}{{^if COUCHBASE_SERVICE_PORT}}8091{{/if}}
        operation_timeout: 2
        detailed_errcodes: 1
      options:
        auto_create: false
        auto_index: true
      credential:
            username: {{COUCHBASE_SERVICE_USER}}{{^if COUCHBASE_SERVICE_USER}}"Administrator"{{/if}}
            password: {{COUCHBASE_SERVICE_PASSWD}}{{^if COUCHBASE_SERVICE_PASSWD}}"password"{{/if}}
    {{/if}}
    
  # Controller
  - descriptor: "beacons:controller:default:default:1.0"

  # Shared HTTP Endpoint
  - descriptor: "pip-services:endpoint:http:default:1.0"
    connection:
      protocol: http
      host: 0.0.0.0
      port: {{HTTP_PORT}}{{^if HTTP_PORT}}8080{{/if}}
  
  # HTTP Service V1
  - descriptor: "beacons:service:http:default:1.0"
  
  # Hearbeat service
  - descriptor: "pip-services:heartbeat-service:http:default:1.0"
  
  # Status service
  - descriptor: "pip-services:status-service:http:default:1.0"  

  # gRPC Service V1
  - descriptor: "beacons:service:grpc:default:1.0"
    connection:
      protocol: http
      host: 0.0.0.0
      port: {{GRPC_PORT}}{{^if GRPC_PORT}}8081{{/if}}
```
