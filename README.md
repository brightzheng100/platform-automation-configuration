## Sample Git Repo for `platform-automation`

### Overview

Below is just as a sample folder structure which has 2 fundations, namely `dev` and `pez`.

```
.
├── README.md
├── dev
│   ├── config
│   │   ├── auth-ldap.yml
│   │   ├── auth-saml.yml
│   │   └── auth.yml
│   ├── env
│   │   └── env.yml
│   ├── generated-config
│   ├── products
│   │   ├── cf.yml
│   │   ├── credhub-service-broker.yml
│   │   ├── director-1.x.yml
│   │   ├── director.yml
│   │   ├── ops-manager.yml
│   │   └── pivotal-container-service.yml
│   ├── state
│   │   └── state.yml
│   └── vars
│       ├── cf-vars.yml
│       ├── credhub-service-broker-vars.yml
│       ├── director-vars.yml
│       ├── ops-manager-vars.yml
│       └── pivotal-container-service-vars.yml
└── pez
    ├── config
    │   └── auth.yml
    ├── env
    │   └── env.yml
    ├── generated-config
    ├── products
    │   ├── director.yml
    │   ├── ops-manager.yml
    │   └── pivotal-container-service.yml
    ├── state
    │   └── state.yml
    └── vars
        ├── director-vars.yml
        ├── ops-manager-vars.yml
        └── pivotal-container-service-vars.yml
```

### Detailed Description

| Folder/File | Purposes | Samples  |
| --- | --- | --- |
| `<FOUNDATION-CODE>`  | As the root folder for a dedicated foundation | This can be something like `dev`, `qa`, `sit`, `prod` etc. which is up to the team's convention to name the foundations. |
| config | A folder for global configs. Currently there is only one `auth.yml` to configure the OpsMan authentication. | The default `auth.yml` is for basic authentication. Please refer to sample of `auth-ldap.yml` and `auth-saml.yml` for other mechanisms |
| env | A folder to contain properties for targeting and logging into the Ops Manager API.  | Currently there is only one `env.yml` to configure the properties so that the `platform-automation` tasks can use `om --env the-path-to/env.yml <COMMAND> ...` to interact with OpsMan APIs |
| generated-config | A folder to contain automatically generated product config files during operations, it should start with no files  | A naming convention of `<PRODUCT-NAME>-<PRODUCT-VERSION>.yml` will be applied to all the files within this folder, e.g. `cf-2.2.11.yml`, `director-2.4.1.yml` |
| products | A folder to contain templatized product configs  | A couple of samples have been provided but please remove all of them to start with |
| state | A folder to contain the meta information named `state.yml` to manage the Ops Manager VM. This content of this file will be managed by pipelines during operations.  | A sample has been provided for GCP. Please change the IaaS code to meet your context |
| vars | A folder to contain vars for templatized product configs in folder of `/products` | A couple of samples have been provided but please remove all of them to start with |
