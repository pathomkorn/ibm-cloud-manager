#IBM PowerVC Keypair Setup
* Cloud consumer
  * Login IBM Cloud Manager with OpenStack dashboard
  * Navigate `PROJECT` > `Compute` > `Access & Security`
  * Click `Key Pairs` tab
  * Click `Create Key Pair` button
  * Download key pair
  * Navigate `PROJECT` > `Compute` > `Access & Security`
  * Click `Key Pairs` tab
  * Select newly created key pair
  * Copy `Public Key` value and save to public key file
  * Send `public key file` and `public key name` to cloud provider
* Cloud provider
 * Login IBM PowerVC SSH session with root
 * Create [powervcrc](/pathomkorn/ibm-powervc/powervcrc) file
```bash
export OS_USERNAME=root
export OS_PASSWORD=${powervc_password}
export OS_TENANT_NAME=ibm-default
export OS_AUTH_URL=https://${powervc_server}:5000/v3/
export OS_IDENTITY_API_VERSION=3
export OS_CACERT=/etc/pki/tls/certs/powervc.crt
export OS_REGION_NAME=RegionOne
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
```
 * Sourcing powervcrc file
```bash
# source powervcrc
 ```
 * Add cloud consumer key pair to PowerVC
```bash
# nova keypair-add --pub_key ${public_key_file} ${public_key_name}
 ```
 * Verify key pair added in PowerVC
```bash
# nova keypair-list
 ```
 * Cloud consumer
  * Login IBM Cloud Manager with OpenStack dashboard
  * Launch PowerVC instance using key pair
