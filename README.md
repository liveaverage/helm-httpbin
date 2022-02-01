# helm-httpbin

Quick example of using helm to deploy httpbin on OpenShift Service Mesh. This assumes your SMCP and member projects have been configured.

# Usage

  1. Login to OpenShift via CLI, navigate to the appropriate tenant project, and Clone this project
  ```
  oc project tenant-1
  git clone https://github.com/liveaverage/helm-httpbin.git
  cd helm-httpbin
  ```
  2. Ensure you have a TLS secret present for your Service Mesh Gateway. **This must existing the project hosting your SMCP**
  ```
  oc create secret tls secret-cert --cert=/home/liveaverage/.acme.sh/api.vmx.int.shifti.us/fullchain.cer --key=/home/liveaverage/.acme.sh/api.vmx.int.shifti.us/api.vmx.int.shifti.us.key -n PROJECT_SMCP
  ```
  3. Update the `values.yaml` with your domain/subdomain, desired names, and appropriate Service Mesh Annotations.
  ```
  ...
  # Enable or Disable service Mesh integration
  sm:
    enabled: true
    # Leave an empty dictionary if injection should be disabled: {}
    annotations:
      sidecar.istio.io/inject: 'true'
      sidecar.istio.io/rewriteAppHTTPProbers: 'true'
  ...
  ```
  4. Install your helm chart
  ```
  helm install helm-httpbin .
  ```
  5. Adjust values.yaml as needed and reinstall/upgrade to make changes:
  ```
  helm upgrade helm-httpbin .
  ```
