# cert-manager-issuers

![Test Workflow](https://github.com/kadras-io/cert-manager-issuers/actions/workflows/test.yml/badge.svg)
![Release Workflow](https://github.com/kadras-io/cert-manager-issuers/actions/workflows/release.yml/badge.svg)
[![The SLSA Level 3 badge](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev/spec/v1.0/levels)
[![The Apache 2.0 license badge](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Follow us on Twitter](https://img.shields.io/static/v1?label=Twitter&message=Follow&color=1DA1F2)](https://twitter.com/kadrasIO)

> **Warning**
> This package has been archived. Issuers are now included in the [cert-manager](https://github.com/kadras-io/package-for-cert-manager) package.

A Carvel package providing a collection of issuers for [cert-manager](https://github.com/kadras-io/package-for-cert-manager), used by the Kadras platform to support TLS via a private CA or Let's Encrypt.

## 🚀&nbsp; Getting Started

### Prerequisites

* Kubernetes 1.28+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
  ```

### Dependencies

cert-manager-issuers requires the [cert-manager](https://github.com/kadras-io/package-for-cert-manager) package. You can install it from the [Kadras package repository](https://github.com/kadras-io/kadras-packages).

### Installation

Add the Kadras [package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster:

  ```shell
  kctrl package repository add -r kadras-packages \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-system --create-namespace
  ```

<details><summary>Installation without package repository</summary>
The recommended way of installing the cert-manager-issuers package is via the Kadras <a href="https://github.com/kadras-io/kadras-packages">package repository</a>. If you prefer not using the repository, you can add the package definition directly using <a href="https://carvel.dev/kapp/docs/latest/install"><code>kapp</code></a> or <code>kubectl</code>.

  ```shell
  kubectl create namespace kadras-system
  kapp deploy -a cert-manager-issuers-package -n kadras-system -y \
    -f https://github.com/kadras-io/cert-manager-issuers/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/cert-manager-issuers/releases/latest/download/package.yml
  ```
</details>

Install the cert-manager-issuers package:

  ```shell
  kctrl package install -i cert-manager-issuers \
    -p cert-manager-issuers.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-system
  ```

> **Note**
> You can find the `${VERSION}` value by retrieving the list of package versions available in the Kadras package repository installed on your cluster.
> 
>   ```shell
>   kctrl package available list -p cert-manager-issuers.packages.kadras.io -n kadras-system
>   ```

Verify the installed packages and their status:

  ```shell
  kctrl package installed list -n kadras-system
  ```

## 📙&nbsp; Documentation

Documentation, tutorials and examples for this package are available in the [docs](docs) folder.
For documentation specific to cert-manager, check out [cert-manager.io](https://cert-manager.io).

## 🎯&nbsp; Configuration

The cert-manager-issuers package can be customized via a `values.yml` file.

  ```yaml
  letsencrypt:
    include: true
    email: cert.info@example.com
  ```

Reference the `values.yml` file from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i cert-manager-issuers \
    -p cert-manager-issuers.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-system \
    --values-file values.yml
  ```

### Values

The cert-manager-issuers package has the following configurable properties.

<details><summary>Configurable properties</summary>

| Config | Default | Description |
|-------|-------------------|-------------|
| `namespace` | `cert-manager` | The namespace where cert-manager is deployed. |
| `letsencrypt.include` | `false` | Whether to include a ClusterIssuer for Let's Encrypt. |
| `letsencrypt.staging` | `true` | Whether to use Let's Encrypt staging, recommended for non-production environments. |
| `letsencrypt.email` | `""` | The email address that Let's Encrypt will use to send info on expiring certificates or other issues. |

</details>

## 🛡️&nbsp; Security

The security process for reporting vulnerabilities is described in [SECURITY.md](SECURITY.md).

## 🖊️&nbsp; License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for more information.
