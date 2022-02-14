
<!-- markdownlint-disable -->
# Deploy Kafka / Confluent on a Raspberry Pi


<!-- markdownlint-restore -->

[![README Header][readme_header_img]][readme_header_link]

<!--




  ** DO NOT EDIT THIS FILE
  **
  ** This file was automatically generated by the `build-harness`.
  ** 1) Make all changes to `README.yaml`
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file.
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **





-->
A Confluent CFK deployment on a Raspberry Pi using experimental ARM based Docker images.

*NOTE: Please [contact us](mailto:enquiries@oso.sh) to gain access to these Docker images as they are stored on an OSO private registry* 

---






## Usage

For

### Repository structure




## Examples

### Setup Raspberry Pi 
We need to prepare the device with a standard OS install so that we can run Kubernetes. .
1. *Install the baseOS* by following the instructions to install a 64bit Raspberry Pi OS as documented [here](https://www.raspberrypi.com/documentation/computers/getting-started.html)

2. *Enable SSH on RaspberryPi* connect to the new raspberryPi image (SD Card), navigate to the `boot` directory, and create a file called `ssh`. This will allow a scripted instalation process to run quicker.

3. *Enable CGroup memory* On line 1 of the file /boot/cmdline.txt append the following values:
  ```shell
    cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
  ```

4. *Configure Pi Networking* Add / Edit the file /etc/dchpd.conf, add the following (adjust as necessary to internal network)
  ```shell
    interface eth0
    static ip_address=192.168.0.99/24
    static routers=192.168.0.1
    static domain_name_servers=192.168.0.1 8.8.8.8
  ```

### Connect to Raspberry Pi (remotely)
Now we have setup the base configuration, we are able to connect over ssh and start the deployment process. 
1. *SSH onto RaspberryPi* using the following commands:
  ```shell
    ssh pi@192.168.0.99
  ```
  The password will be `raspberry`

2. *Install GIT* Used to pull the configuration. Support for GitOps is important to enforce consistent configration over a fleet of devices
  ```shell
    sudo apt-get install git
  ```

3. *Install K3S* a lighestweisght Kubernetes distro designed to run on a small footprint under limited resources
  ```shell
    curl -sfL https://get.k3s.io | sh -s - --disable traefik --disable metrics-server
  ```

### Deploy CFK on K3S
1. Apply the Confluent CRDs using: `kubectl apply -k ./crds`
  ```shell
    ➜  kafka-arm-images git:(docs) ✗ kubectl apply -k ./crds
    customresourcedefinition.apiextensions.k8s.io/clusterlinks.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/confluentrolebindings.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/connectors.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/connects.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/controlcenters.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/kafkarestclasses.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/kafkarestproxies.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/kafkas.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/kafkatopics.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/ksqldbs.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/migrationjobs.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/schemaregistries.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/schemas.platform.confluent.io created
    customresourcedefinition.apiextensions.k8s.io/zookeepers.platform.confluent.io created
  ```

2. Depoy Operator, Zookeeper and Kafka ARM compatable images using: `kubectl apply -k .`
  ```shell
    ➜  kafka-arm-images git:(docs) ✗ kubectl apply -k .
    namespace/sandbox created
    serviceaccount/confluent-for-kubernetes created
    clusterrole.rbac.authorization.k8s.io/confluent-operator created
    clusterrolebinding.rbac.authorization.k8s.io/confluent-operator created
    secret/confluent-operator-licensing created
    service/confluent-operator created
    deployment.apps/confluent-operator created
    kafka.platform.confluent.io/kafka created
    zookeeper.platform.confluent.io/zookeeper created
  ```





## Related Projects

Check out these related projects.

- [Confluent for Kubernetes (CFK) examples](https://github.com/osodevops/confluent-kubernetes-playground) - Playground for Kafka / Confluent Kubernetes experimentations



## Need some help

File a GitHub [issue](https://github.com/osodevops/kafka-arm-images/issues), send us an [email][email] or [tweet us][twitter].

## The legals

Copyright © 2017-2022 [OSO](https://oso.sh) | See [LICENCE](LICENSE) for full details.

[<img src="https://oso-public-resources.s3.eu-west-1.amazonaws.com/oso-logo-green.png" alt="OSO who we are" width="250"/>](https://oso.sh/who-we-are/)

## Who we are

We at [OSO][website] help teams to adopt emerging technologies and solutions to boost their competitiveness, operational excellence and introduce meaningful innovations that drive real business growth. Our developer-first culture, combined with our cross-industry experience and battle-tested delivery methods allow us to implement the most impactful solutions for your business.

Looking for support applying emerging technologies in your business? We’d love to hear from you, get in touch by [email][email]

Start adopting new technologies by checking out [our other projects][github], [follow us on twitter][twitter], [join our team of leaders and challengers][careers], or [contact us][contact] to find the right technology to support your business.[![Beacon][beacon]][website]

  [logo]: https://oso-public-resources.s3.eu-west-1.amazonaws.com/oso-logo-green.png
  [website]: https://oso.sh?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=website
  [github]: https://github.com/osodevops?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=github
  [careers]: https://oso.sh/careers/?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=careers
  [contact]: https://oso.sh/contact/?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=contact
  [linkedin]: https://www.linkedin.com/company/oso-devops?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=linkedin
  [twitter]: https://twitter.com/osodevops?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=twitter
  [email]: mailto:enquiries@oso.sh?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=email
  [readme_header_img]: https://oso-public-resources.s3.eu-west-1.amazonaws.com/oso-animation.gif
  [readme_header_link]: https://oso.sh/what-we-do/?utm_source=github&utm_medium=readme&utm_campaign=osodevops/kafka-arm-images&utm_content=readme_header_link
  [beacon]: https://github-analyics.ew.r.appspot.com/G-WV0Q3HYW08/osodevops/kafka-arm-images?pixel&cs=github&cm=readme&an=kafka-arm-images
