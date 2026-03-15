# ☸ Kubernetes Deployment

We have developed MaiBot's Helm Chart to help you elegantly deploy MaiBot to Kubernetes.

This document assumes you are familiar with Kubernetes and Helm, so it won't explain the role of each part in detail.

This Helm Chart uses [MaiBot-Napcat-Adapter](https://github.com/Mai-with-u/MaiBot-Napcat-Adapter) as the sole adapter.

## 📋 Environment Requirements

- ✅ Kubernetes deployed (can be single-node cluster)
- ⚙️ Helm v3.8.0 or higher installed
- 💾 Available Storage Class deployed in Kubernetes cluster
- 🌎 (Optional) Domain name resolved to cluster ingress, and available Ingress Class deployed in cluster

## 🛠️ Deployment Steps

### 🔍 1. View Helm Chart Information

MaiBot's Helm Chart is published at `oci://reg.mikumikumi.xyz/maibot/maibot`.

Before deploying MaiBot, please confirm the version you want to deploy.

You can view all available Helm Chart versions and corresponding MaiBot versions in the [helm-chart branch](https://github.com/Mai-with-u/MaiBot/tree/helm-chart/helm-chart) of the MaiBot code repository.

This document uses `<CHART_VERSION>` and similar placeholders for Helm Chart versions. Please replace them with the actual version you need to install. If omitting the `--version` flag, the latest official version will be installed by default.

If you want to view Chart information:
```shell
helm show chart oci://reg.mikumikumi.xyz/maibot/maibot --version <CHART_VERSION>
```

### 📝 2. Get and Modify Chart Values File

Output Chart values file to `maibot.yaml`:
```shell
helm show values oci://reg.mikumikumi.xyz/maibot/maibot --version <CHART_VERSION> > maibot.yaml
```

Edit `maibot.yaml` file, configure options as needed.

#### Values Item Description

`values.yaml` is divided into several main sections.

1. `EULA` & `PRIVACY`: Users must agree to the agreements here to successfully deploy MaiBot.

2. `pre_processor`: Configuration for pre-processing Job before deployment.

3. `adapter`: Deployment configuration for MaiBot's Adapter.

4. `core`: Deployment configuration for MaiBot core.

5. `statistics_dashboard`: Deployment configuration for MaiBot runtime statistics dashboard.

   MaiBot automatically outputs html format runtime statistics reports periodically. This statistics report can be deployed as a dashboard.

   Disabled by default for privacy considerations.

6. `napcat`: Napcat deployment configuration.

   Considering reuse of external Napcat instances, Napcat deployment has been decoupled. Users can choose whether to deploy Napcat.

   Napcat will be bundled for deployment by default.

7. `sqlite_web`: sqlite-web deployment configuration.

   Through sqlite-web, you can operate MaiBot's database on web page, convenient for debugging. Not deploying has no impact on MaiBot operation.

   This service is very dangerous if exposed to public network, not deployed by default.

8. `config`: Fill in runtime configuration for various components of MaiBot here.

   Configuration filled here will only overwrite actual configuration files during initial deployment or when user specifies, and needs to strictly follow yaml file indentation format.

   **If you find filling configuration here too troublesome, you don't need to fill this part, configure through WebUI graphical interface after successful deployment.**

   - `override_*_config`: Specify whether this deployment/upgrade uses following configuration to overwrite actual configuration files. Default not overwrite.

   - `adapter_config`: Corresponds to adapter's `config.toml`. See [Adapter Documentation](https://docs.mai-mai.org/manual/adapters/napcat.html).

   - `core_model_config`: Corresponds to core's `model_config.toml`. See [Model Configuration Guide](https://docs.mai-mai.org/manual/configuration/configuration_model_standard).

   - `core_bot_config`: Corresponds to core's `bot_config.toml`. See [Configuration Guide](https://docs.mai-mai.org/manual/configuration/configuration_standard).

::: tip
After editing, please properly save this values file.
:::

### 🏠️ 3. Create Kubernetes Namespace

Create a namespace for MaiBot in Kubernetes, for example `bot`:
```shell
kubectl create ns bot
```

### 📥 4. Deploy MaiBot Instance

According to the edited `maibot.yaml`, deploy MaiBot to `bot` namespace. Give this installation instance a name, for example `maimai`.

```shell
helm install maimai oci://reg.mikumikumi.xyz/maibot/maibot --namespace bot --version <CHART_VERSION> --values maibot.yaml
```

During first deployment, to adapt to k8s architecture, pre-processing task will overwrite some key configurations. This takes some time, so deployment process may be slow, and some Pods may fail to start, wait about one minute.

::: tip
Pre-processing task is implemented through Helm Chart's Post-Install Hook, only triggered during each `helm install/upgrade/rollback`.
:::

::: tip
You can deploy multiple MaiBot installation instances within cluster, as long as these instance names are different.
:::

### ⚡ 5. Configure napcat to Connect to MaiBot

Open napcat console.

- If bundled deployment of napcat, and configured Ingress, then can open URL like `https://napcat.example.com/` in browser (domain configured in values), reach console.

- If bundled deployment of napcat, but not configured Ingress, then need to view napcat's Service, access console port through port forwarding (default 6099) or NodePort.

- If not bundled deployment, decide to use external napcat instance, please open external napcat console.

After entering console, login to QQ used by MaiBot, then immediately modify console password.

Steps to connect to MaiBot:

1. Enter `Network Configuration`, create new `Websocket Client`.

2. Enable configuration, give this connection a name, for example `MaiMai`.

3. Fill in MaiBot adapter's Websocket address.

   - If bundled deployment of napcat, URL to fill here should be:
     ```text
     ws://<RELEASE_NAME>-maibot-adapter:<ADAPTER_SVC_PORT>/
     
     # RELEASE_NAME is MaiBot installation instance name, e.g., maimai
     # ADAPTER_SVC_PORT is adapter service port, default 8095
     
     # An example:
     ws://maimai-maibot-adapter:8095/
     ```

     ::: tip
     k8s cluster internal DNS name rules: [Service and Pod DNS](https://kubernetes.io/zh-cn/docs/concepts/services-networking/dns-pod-service/).
     :::
     
   - If not bundled deployment of napcat, decide to use external napcat instance, should fill adapter's Websocket service URL here.

     You can fill according to adapter's Service ClusterIP + Port, or according to node IP + NodePort, or implement port forwarding yourself.

4. Heartbeat interval consistent with `config.adapter_config.napcat_server.heartbeat_interval` in values (default consistent, no need to modify).

5. To enhance security, can set Token for connection between adapter and Napcat. Token needs to be consistent with `config.adapter_config.napcat_server.token` in values. Token not enabled by default.

6. Click save, observe adapter and core logs, check if successfully connected.

### 🎉 6. Test MaiBot

Now can send messages to MaiBot, test if available.

## ⏫ Upgrade MaiBot

Helm Chart development usually lags behind main version for some time. When MaiBot has new Release, corresponding Helm Chart may be released a few days later, please wait patiently during this period.

When new version Helm Chart available, please update MaiBot installation instance according to these steps.

1. If you don't manage configuration through Helm Chart, first need to backup your actual configuration files.
   ```shell
   kubectl cp -n bot maimai-maibot-adapter-0:/adapters/config.toml adapter_config.toml
   kubectl cp -n bot maimai-maibot-core-0:/MaiMBot/config/bot_config.toml core_bot_config.toml
   kubectl cp -n bot maimai-maibot-core-0:/MaiMBot/config/model_config.toml core_model_config.toml
   ```

2. Backup storage volumes of various MaiBot components.

   This is not mandatory, but recommended practice, used for rollback when upgrade has problems. According to different implementations of your storage backend, there will be different backup methods.

   Before backup, need to first close `core` and `adapter` components:
   ```shell
   kubectl scale -n bot statefulset maimai-maibot-adapter --replicas 0
   kubectl scale -n bot statefulset maimai-maibot-core --replicas 0
   # Wait for Pods to completely terminate before backup
   ```

3. Rename old version values file:
   ```shell
   mv maibot.yaml maibot-old.yaml
   ```

4. Get new version values file:
   ```shell
   helm show values oci://reg.mikumikumi.xyz/maibot/maibot --version <NEW_VERSION> > maibot.yaml
   ```

5. Need special explanation: configuration files cannot be automatically upgraded to new version, so must manually handle configuration file upgrade.

   Refer to old version values file (if you don't manage configuration through Helm Chart, then also need to refer to actual configuration files backed up just now), fill new version values file as needed.

   Usually values file main body won't have big changes, while config part will have more changes, need special attention.

   **If you find filling `config` part too troublesome, also can not fill, but after upgrade complete, configure from scratch through WebUI graphical interface. But such workload may be relatively large.**

6. Upgrade MaiBot instance:
   ```shell
   helm upgrade maimai oci://reg.mikumikumi.xyz/maibot/maibot \
       --namespace bot \
       --version <NEW_VERSION> \
       --values maibot.yaml \
       --set config.override_adapter_config=true \
       --set config.override_core_bot_config=true \
       --set config.override_core_model_config=true
   ```
   
   Note need to use `--set config.override_*_config=true` option, specify using new version configuration files to forcibly overwrite original old version configuration files, otherwise new version components cannot recognize old version configuration.

## ✏️ Modify MaiBot Configuration

MaiBot configuration files stored in storage volumes. Specifically:

- `adapter` configuration files stored in `<RELEASE_NAME>-maibot-adapter-config` PVC bound storage volume, corresponding to `/adapters/` path in container.
- `core` configuration files stored in `<RELEASE_NAME>-maibot-core-config` PVC bound storage volume, corresponding to `/MaiMBot/config/` path in container.

Generally speaking, there are three ways to modify MaiBot configuration files:

1. Configure through WebUI (recommended).
2. Enter container or storage volume, directly modify configuration files.
3. Completely not use above two solutions, only manage configuration files through Helm Chart `config` item (this requires configuring `config.override_*_config` always `true`).
   Then update instance through `helm upgrade` command, simultaneously update configuration files:
   ```shell
   helm upgrade maimai oci://reg.mikumikumi.xyz/maibot/maibot --namespace bot --version <CURRENT_VERSION> --values maibot.yaml
   ```

## ↩ Rollback MaiBot

If want to roll back to previously deployed version, can use `helm rollback` command to roll back deployment configuration:
```shell
helm history maimai --namespace bot  # View all deployment version history of MaiBot
helm rollback maimai --namespace bot  # Return to MaiBot previous version
helm rollback maimai <HISTORY_INDEX> --namespace bot  # Return to MaiBot specified version
```

**Note, this method only rolls back MaiBot's k8s deployment configuration (like image version), various components' configuration files and actual data cannot be directly rolled back (need to manually restore previous backups), please operate carefully.**

## 🗑 Uninstall MaiBot

Use following command can remove MaiBot installation instance:
```shell
helm uninstall maimai -n bot
```

After uninstallation, values required to deploy MaiBot cannot be retrieved, recommend saving values file properly.

After uninstallation, whether MaiBot storage volume data will be deleted depends on storage class configuration, may need cluster administrator to manually handle.

## ❓ Other Notes

### 📥 Backup MaiBot Data Before Operations

Upgrade/downgrade operations between different versions of MaiBot may cause MaiBot data abnormalities.

Rolling back MaiBot deployment configuration may also cause MaiBot data abnormalities.

Before performing these operations, recommend backing up MaiBot storage volume data in advance.

### ⚡ Old Version Configuration Migration

Versions before `0.11.6-beta` stored configuration in k8s ConfigMap resources. With version iteration, MaiBot's operation complexity on configuration files increased, k8s adaptation complexity also increased synchronously, and WebUI can directly modify configuration files, therefore starting from `0.11.6-beta` version, various components' configuration no longer stored in k8s ConfigMap, but directly stored in actual files in storage volume.

Users upgrading from old version, old ConfigMap configuration will be automatically migrated to new storage volume configuration files by pre-processing task.

### 🔄 Configuration Automatically Reset During Deployment

adapter configuration `napcat_server` and `maibot_server` `host` and `port` fields will be automatically reset during each deployment/update of Helm installation instance.
core configuration `webui` and `maim_message` some fields will also be automatically reset during each deployment/update of Helm installation instance.

Reasons for automatic reset:

- core Service DNS name is dynamic (concatenated by installation instance name), cannot be determined in advance in adapter configuration file.
- To make adapter listen to all addresses and maintain port number configured in Helm Chart, need to overwrite these configurations in adapter configuration file.
- core WebUI start/stop needs to be controlled by Helm Chart, to properly create Service and Ingress resources.
- core maim_message api server can now be exposed as k8s service. Listening IP and port need to be controlled by Helm Chart, for Service to correctly map.

During first deployment, pre-processing task will be responsible for resetting these configurations. This takes some time, so deployment process may be slow, and some Pods may fail to start, wait about one minute.

### 🗙 Cross-node PVC Mounting Issues

Some MaiBot components will mount same PVC, mainly for synchronizing data or modifying configuration.

If k8s cluster has multiple nodes, and Pods sharing same PVC not scheduled to same node, then this PVC access mode needs to have `ReadWriteMany` access mode.

Not all storage controllers support `ReadWriteMany` access mode.

If your storage controller cannot support `ReadWriteMany` access mode, you can avoid problems by configuring `nodeSelector` to schedule Pods sharing same PVC to same node.

Component list that will share PVC:
- `core` and `adapter`: Share `adapter-config`, for providing `core` WebUI ability to modify adapter configuration files.
- `core` and `statistics-dashboard`: Share `statistics-dashboard`, for synchronizing statistics data html files.
- `core` and `sqlite-web`: Share `maibot-core`, for providing `sqlite-web` ability to operate MaiBot database.
- Deployment pre-processing task `preprocessor` and `adapter`, `core`: Share `adapter-config` and `core-config`, for initializing `core` and `adapter` configuration files.