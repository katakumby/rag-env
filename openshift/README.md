## Prep namespaces
```
oc new-project ailab

oc new-project qdrant
```
# MCP
```oc apply -f yaml/atlassian-mcp/mcp-atlassian-deployment.yaml -n ailab```

## CONFIG
```
oc apply -f .\yaml\analyst-agent\analyst-agent-config.yaml -n ailab
```
## Qdrant
```
helm install qdrant qdrant/qdrant --namespace qdrant --create-namespace
helm upgrade -i qdrant qdrant/qdrant -n qdrant  -f .\yaml\qdrant\values.yam
```

OR

```
oc apply -f .\yaml\qdrant\qdrant-deployment.yaml -n qdrant
oc apply -f .\yaml\qdrant\route-qdrant.yaml -n qdrant
```

? oc label namespace ailab netpol-allow=ailab --overwrite



# Seed Qdrant
```
oc apply -f .\yaml\analyst-agent\rag-chunk-job-runtime-install.yaml -n ailab
oc delete job rag-chunk-job-runtime-install -n ailab --ignore-not-found
```
# Agent
```
oc apply -f .\yaml\analyst-agent\analyst-agent-deployment.yaml -n ailab
 
oc apply -f .\yaml\analyst-agent\analyst-agent-service-route.yaml -n ailab
```