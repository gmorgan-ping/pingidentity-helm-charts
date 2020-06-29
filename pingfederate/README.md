# PingFederate

## Use Devops Key
In a development or sandbox environment, the helm chart can be run with a valid registered devops license key (https://pingidentity-devops.gitbook.io/devops/getstarted/devopsregistration).  Make sure the `PING_IDENTITY_DEVOPS_USER` and `PING_IDENTITY_DEVOPS_KEY` environment variables are set

```shell
helm install \
--generate-name --debug \
--set license.useDevOpsKey=true \
--set license.devOpsKey.user=${PING_IDENTITY_DEVOPS_USER} \
--set license.devOpsKey.key=${PING_IDENTITY_DEVOPS_KEY} \
--set license.acceptEULA=yes \
 ./
```

## Use provided License file
In a controlled environment, the helm chart can be run with a valid license file.  Add the license file as a secret as shown:
```shell

kubectl create secret generic pingfederate-license --from-file ./pingfederate.lic

helm install \
--generate-name --debug \
--set license.licenseSecretName=pingfederate-license
--set license.acceptEULA=yes \
 ./
```

## Chart Configuration

| Parameter | Description | Default |
|--|--|--|
| TODO | TODO | TODO |


## PingFederate Configuration Profile

By default this chart loads sample PingFederate configuration on startup.  These are layered as:

| Configuration Layer | Server Profile |
|--|--|
| 1 | https://github.com/pingidentity/pingidentity-server-profiles/tree/master/pf-dns-ping-clustering |
| 2 | https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingfederate |

You can add additional layers or change to your own configuration repository by modifying the `SERVER_PROFILE_*` environment variables in `pingfederate.envs` of the chart configuration

## Clustering Example

### Admin Console
```shell
helm install \
pingfederate-admin --debug \
--set pingfederate.logging=DEBUG \
--set pingfederate.clustering.enabled=true \
--set pingfederate.clustering.adminConsoleRole=true \
--set license.useDevOpsKey=true \
--set license.devOpsKey.user=${PING_IDENTITY_DEVOPS_USER} \
--set license.devOpsKey.key=${PING_IDENTITY_DEVOPS_KEY} \
--set license.acceptEULA=yes \
 ./
```

### Engines:
```shell
helm install \
pingfederate-runtime --debug \
--set pingfederate.logging=DEBUG \
--set pingfederate.clustering.enabled=true \
--set pingfederate.clustering.adminConsoleRole=false \
--set replicaCount=3 \
--set license.useDevOpsKey=true \
--set license.devOpsKey.user=${PING_IDENTITY_DEVOPS_USER} \
--set license.devOpsKey.key=${PING_IDENTITY_DEVOPS_KEY} \
--set license.acceptEULA=yes \
 ./
```

## Testing

Tested with **PingFederate 10.1**