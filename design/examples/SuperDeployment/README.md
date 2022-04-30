# SuperDeployment
This folder contains the manifests that the operator should create. For this example we are using an Abstracted Resource of kind `SuperDeployment`

## ARD is created
You can see what the ARD would look like in `./definition/ard.yaml`.

When an ARD is created, the operator should:
- Create a CRD for the AR based on the ARD (exactly same schema but removing `spec.versions[*].templates`). This should look like `./crd.yaml`
- Create a Helm chart based on the `spec.versions[*].templates`. This should look like `./chart`
- Listen for instances of that CRD being created

## AR is created
You can see what an AR for SuperDeployment would look like in `./example-nginx.yaml`

When an AR is created, the operator should:
- Take the whole resource and dump it in a values.yaml file
- Render the Helm chart with those values
- kubectl apply the rendered files