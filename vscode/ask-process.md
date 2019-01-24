Note the "processId" attribute

```
{
  "type": "node",
  "request": "attach",
  "name": "Node: Nodemon",
  "processId": "${command:PickProcess}",
  "restart": true,
  "protocol": "inspector",
  "port": 8001,
  "address": "localhost",
  "sourceMaps": false,
  "localRoot": "${workspaceRoot}/api",
  "remoteRoot": "/app"
}
```
