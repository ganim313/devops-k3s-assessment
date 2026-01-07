# Debugging – OIDC Failure

## Observed Behavior
After enabling OIDC, the application starts successfully, but authentication fails at runtime.

## Root Cause
The OIDC issuer uses a self-signed TLS certificate that is not trusted by the container’s default CA bundle.

## Diagnosis
- Verified pod health using `kubectl get pods`
- Inspected logs using `kubectl logs open-webui-0`
- Identified that OIDC validation occurs only during authentication

## Fix
Trust the custom CA by mounting it via a ConfigMap and updating the container trust store.

This preserves TLS security and is suitable for production use.
