# DAPS Server


Notice: Tractus-x relies on an IDS-compatible architecture. In that context, a DAPS is needed as an identity server. In this project, we use Fraunhofer's DAPS implementation (https://github.com/Fraunhofer-AISEC/omejdn-server#readme). We recommend choosing your favorite identity server. The provided helm charts here need to be adapted accordingly.

![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.0.0](https://img.shields.io/badge/AppVersion-1.0.0-informational?style=flat-square)

[https://github.com/Fraunhofer-AISEC/omejdn-server#readme ](https://github.com/Fraunhofer-AISEC/omejdn-server#readme) 

DAPS is a minimal but extensible OAuth 2.0/OpenID connect server used for ..

IoT devices which use their private keys to request OAuth2 access tokens in order to access protected resources
Websites or apps which retrieve user attributes
It is used as the Dynamic Attribute Provisioning Service (DAPS) prototype of the Industrial Data Space.

Some of DAPS's core features include:

Database-free easy-to-read configuration files
Integration of existing LDAP directory services
Fully configurable through the Admin API Plugin
A User Selfservice API Plugin
Standard Compliance (see below)
IMPORTANT: DAPS is meant to be a research sandbox in which we can (re)implement standard protocols and potentially extend and modify functionality under the hood to support research projects. Use at your own risk! At a minimum, take a look at the documentation for production setups.


### Software Version
```shell
Helm version is v1.7.8
Application version is v1.7.1
```


## Directory structure of an DAPS server
By default, daps uses the following directory structure for configurations and keys:  <br />

```
\_ omejdn.rb                 (Omejdn Source code)
\_ lib/                      (Additional Source code)
\_ plugins/
    \_ api/                  (API Plugins)
    \_ claim_mapper/         (Claim Mapper Plugins)
    \_ user_db/              (User Database Plugins)
\_ config/
    \_ omejdn.yml            (The main configuration file)
    \_ clients.yml           (Client configuration file)
    \_ webfinger.yml         (Webfinger configuration)
    \_ oauth_providers.yml   (To configure external OpenID Providers)
    \_ scope_description.yml (Human-readable strings for Scopes)
    \_ scope_mapping.yml     (Mapping Scopes to Attributes)
\_ keys/
    \_ omejdn/               (Keys and Certificates to be JWKS-advertised)
        \_ omejdn.key        (The OAuth2 server private key)
    \_ clients/              (The public key certificates for clients)
\_ views/                    (Web-Pages)
\_ public/                   (Additional frontend resources (CSS+Images))
\_ docs/                     (Documentation)
\_ tests/
    \_ test_*.rb             (Unit and E2E tests for Omejdn)
    \_ test_resources/       (Test vectors)
\_ scripts/                  (Convenience Scripts)
```

## Signing keys
The server public/private key pair used to sign tokens is configured in config/omejdn.yml through the signing_key property for both token types (ID Token and Access Token). The keys will be generated if they do not exist, but you can also provide your own. The keys must be in PEM format.

In order to generate your own key pair with a self-signed pulic key for testing, your can execute:

```
$ openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem
$ mv key.pem keys/signing_key.pem
```

## Configuring the server
***
## Environment variables
```
- APP_ENV: May be set to 'production' to prevent debug output such as logging sensitive information to stdout.
- HOST: May be set to modify the host config variable (useful for docker-compose deployments)
- OMEJDN_PATH_PREFIX: May be set to modify the path_prefix config variable
- BIND_TO: May be set to modify the bind_to config variable
- ALLOW_ORIGIN: May be set to modify the allow_origin config variable
- OMEJDN_ADMIN: May be set to username:password to create an admin user
- OMEJDN_JWT_AUD_OVERRIDE: May be set to modify the expected 'aud' claim in a JWT assertion in a client_credentials flow. The standard usually expects the claim to contain the host, hence use this only if necessary.
```

Setting the environment variables depends on how you run the service. If you are not using docker, you can set the variables as follows:
```
$ export APP_ENV="production"
$ export HOST="https://my-omejdn.example.tld"
$ ruby omejdn.rb
```

## Token verification
All tokens are signed by the server. To retrieve the respecting public key you can use the JSON Web Key Set (https://tools.ietf.org/html/rfc7517, https://auth0.com/docs/jwks)

```
    $ curl <omejdn URL>/.well-known/jwks.json
```


## OpenID Connect
***
By default, OpenID Connect is disabled. In order to enable it, you need to edit omejdn.yml and set openid to true.

### Discovery

You may retrieve the server configuration under

```
    $ curl <omejdn URL>/.well-known/openid-configuration
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Pod affinity configuration |
| autoscaling | object | `{"enabled":false, "maxReplicas":100, "minReplicas":1, "targetCPUUtilizationPercentage":80}` | DAPS autoscaling configuration |
| env.config | object | `{}` | Additional env variables |
| env.secret | object | `{}` | Additional env variables that should be stored in encrypted way |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.repository | string | `"nginx"` | DAPS docker image |
| image.tag | string | `""` | Image tag. Overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` | Secret which contains dockerconfig.json from private container registry with daps image |
| ingress.annotations | object | `{}` | Additional ingress annotations |
| ingress.enabled | bool | `false` | If set to `true`, DAPS will be exposed with ingress controller at http(s)://(ingress.host)/(ingress.pathPrefix) |
| ingress.host | string | `"chart-example.local"` |  |
| ingress.pathPrefix | string | `"/"` | Path prefix to be added to DAPS URI. Regex can be used |
| ingress.rootPath | string | `"/"` | Root prefix without regex rules that used to configure daps host name in configuration |
| ingress.tls.certMgr.enabled | bool | `false` | If `true` cert-manager will be used to issue a certificate with ingress.host CN name |
| ingress.tls.certMgr.issuer | string | `""` | Cert-manager issuer name |
| ingress.tls.enabled | bool | `false` | If `true` daps will be exposed with https |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` | Node selection configuration |
| omejdn.createDefaultAdmin | bool | `false` | Default user and client will be created if set to `true`. User credentials set in `omejdn.defaultAdminUser` section |
| omejdn.defaultAdminUser | string | `"admin:admin"` | Default user credentials in format `user:password` |
| omejdn.serverKey | string | `""` | Server key content. DAPS will generate key if it's not provided at startup |
| omejdn.serverKeyFolderPath | string | `"/opt/server-key"` | Path to directory with private server key |
| persistence.enabled | bool | `true` | If `true` persistent volume will be used to store clients and users configuration |
| persistence.storageClass | string | `"azurefile"` | Storage class to claim a volume. |
| persistence.storageSize | string | `"1Gi"` | Volume size |
| podAnnotations | object | `{}` |  |
| podSecurityContext | object | `{}` | Pod security context configuration |
| replicaCount | int | `1` | DAPS instances count |
| resources | object | `{}` | Pod resources requests and limits configuration |
| securityContext | object | `{}` | Pod security context configuration |
| service.port | int | `4567` | Service port |
| service.type | string | `"ClusterIP"` | Service type |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. -- If not set and create is true, a name is generated using the fullname template |
| tolerations | list | `[]` | Pod toleration settings |


# Installation Steps

https://github.com/eclipse-tractusx/daps-helm-chart/blob/main/INSTALL.md



