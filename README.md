# helm-charts
Repository to hold all Helm charts used during deployment of Lit Republic IT infrastructure.

## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

  helm repo add <alias> https://<orgname>.github.io/helm-charts
  **E.G:** helm repo add litrepublic-charts https://shawngerrard.github.io/helm-charts

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
<alias>` to see the charts.

To install the <chart-name> (**E.G:** lr-wordpress) chart:

    helm install wordpress litrepublic-charts/wordpress

To uninstall the chart:

    helm delete wordpress