## Sample Git Repo for [`platform-automation-pipelines`](https://github.com/brightzheng100/platform-automation-pipelines)

### Overview

The recommended folder structure, based on the real-world practices, is as below:

```
├── README.md
├── <FOUNDATION-CODE, e.g. qa>
│    ├── config
│    │   ├── auth.yml
│    │   └── global.yml
│    ├── env
│    │   └── env.yml
│    ├── generated-config
│    │   └── <PRODUCT-SLUG>.yml
│    ├── products
│    │   └── <PRODUCT-SLUG>.yml
│    ├── state
│    │   └── state.yml
│    ├── vars
│    │   └── <PRODUCT-SLUG>-vars.yml
│    └── products.yml
└── <ANOTHER FOUNDATION-CODE, e.g. prod>
```

So here is a sample folder structure that has 2 fundations, namely `dev` (on GCP) and `pez` (on vSphere).

```
$ tree .
.
├── README.md
├── dev
│   ├── config
│   │   ├── auth.yml
│   │   └── global.yml
│   ├── env
│   │   └── env.yml
│   ├── errands
│   │   └── errands.yml
│   ├── generated-config
│   │   ├── credhub-service-broker.yml
│   │   ├── director.yml
│   │   ├── elastic-runtime.yml
│   │   ├── harbor-container-registry.yml
│   │   ├── p-redis.yml
│   │   ├── p-scheduler.yml
│   │   └── pivotal-container-service.yml
│   ├── products
│   │   ├── credhub-service-broker.yml
│   │   ├── director.yml
│   │   ├── elastic-runtime.yml
│   │   ├── harbor-container-registry.yml
│   │   ├── ops-manager.yml
│   │   ├── p-redis.yml
│   │   └── pivotal-container-service.yml
│   ├── products.yml
│   ├── state
│   │   └── state.yml
│   └── vars
│       ├── credhub-service-broker-vars.yml
│       ├── director-vars.yml
│       ├── elastic-runtime-vars.yml
│       ├── harbor-container-registry-vars.yml
│       ├── ops-manager-vars.yml
│       ├── p-redis-vars.yml
│       └── pivotal-container-service-vars.yml
└── pez
...
```

### Detailed Description

| Folder/File | Purposes | Samples  |
| --- | --- | --- |
| `<FOUNDATION_CODE>`  | As the root folder for a dedicated foundation | This can be something like `dev`, `qa`, `sit`, `prod` etc. which is up to the team's convention to name the foundations. |
| config | A folder for global configs. Currently there are two files: `auth.yml` to configure the OpsMan authentication; `global.yml` for some common configuration which may require CredHub interpolation | The default `auth.yml` is for basic authentication. Please refer to sample of `auth-ldap.yml` and `auth-saml.yml` for other mechanisms |
| env | A folder to contain properties for targeting and logging into the Ops Manager API.  | Currently there is only one `env.yml` to configure the properties so that the `platform-automation` tasks can use `om --env the-path-to/env.yml <COMMAND> ...` to interact with OpsMan APIs |
| errands | A folder to contain errand config files to control errands while applying changes. **For experiment only**  | A naming convention of `<PRODUCT_SLUG>.yml` will be applied to all the files within this folder, e.g. `cf.yml`. For the config file format please refer to `om` Doc [here](https://github.com/pivotal-cf/om/tree/master/docs/apply-changes).  |
| generated-config | A folder to contain automatically generated product config files during operations, it should start with no files  | A naming convention of `<PRODUCT_SLUG>.yml` will be applied to all the files within this folder, e.g. `director.yml`, `pivotal-container-service.yml` |
| products | A folder to contain templatized product configs  | A couple of samples have been provided but please remove all of them to start with |
| state | A folder to contain the meta information named `state.yml` to manage the Ops Manager VM. This content of this file will be managed by pipelines during operations.  | A sample has been provided for GCP. Please change the IaaS code to meet your context |
| vars | A folder to contain vars for templatized product configs in folder of `/products` | A couple of samples have been provided but please remove all of them to start with |


### Evolving Practices

**1. The naming of the files**
   
Previously I used `<PRODUCT_NAME>` but found that it's confusing because we can't easily know what the real `<PRODUCT_NAME>` is until you stage it on PCF.
Now all are based on `<PRODUCT_SLUG>`, that we can easily get it back by simply issuing `$ pivnet products`.

**2. The semver-based config for all products**

A `<FOUNDATION_CODE/products.yml` file is added to have one centralized semantic based config for all products.
Now you can easily embrace GitOps by mantaining this file for the whole foundation.
Thanks to the newly built [Semver Config Concourse Resource](https://github.com/brightzheng100/semver-config-concourse-resource), the version changes can be properly handled by different pipelines: `upgrade`s go to `install-upgrade-products` and `patch`s go to `patch-products`.
Check it out and share your feedback!

**3. Added a `global.yaml` config file under `<FOUNDATION_CODE>/config/`**

Sometimes we need some more global configs, especially for those require CredHub interpolation, e.g. `pivnet_token`. This is the right place to host them.
