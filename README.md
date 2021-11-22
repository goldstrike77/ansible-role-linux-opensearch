![](https://img.shields.io/badge/Ansible-opensearch-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments. The author does not guarantee the accuracy, completeness, reliability, and availability of the role content. Under no circumstances will the author be held responsible or liable in any way for any claims, damages, losses, expenses, costs or liabilities whatsoever, including, without limitation, any direct or indirect damages for loss of profits, business interruption or loss of information.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。作者不对角色内容之准确性、完整性、可靠性、可用性做保证。在任何情况下，作者均不对任何索赔，损害，损失，费用，成本或负债承担任何责任，包括但不限于因利润损失，业务中断或信息丢失而造成的任何直接或间接损害。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_opensearch.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Opensearch Versions](#opensearch-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Donations](#Donations)

## Overview
OpenSearch is a family of software consisting of a search engine, and OpenSearch Dashboards, a data visualization dashboard for that search engine. The software started in 2021 as a fork of Elasticsearch and Kibana, with development led by Amazon Web Services

## Requirements
### Operating systems
This Ansible role installs opensearch on Linux operating system, including establishing a filesystem structure and server configuration with some common operational features, Will works on the following operating systems:

  * CentOS 7

### Opensearch versions

The following list of supported the Opensearch releases:

  * 1.1.0

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `os_cluster`: Specify name for your Opensearch cluster.
* `os_admin_pass`: The password for administrator.
* `os_rotate_day`: Specify the logs retention days.
* `os_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine on Opensearch.
* `os_logstash_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine on LogStash.
* `os_memory_lock`: A boolean to determine whether or not lock the process address space into memory on startup.
* `os_dashboards_proxy`: A boolean to determine whether or not to mount Kibana at if you are running behind a proxy.
* `os_version`: Specify the Opensearch version.
* `os_logstash_version`: Specify the Logstash version.
* `os_data_path`: Specify the Opensearch storage directory.
* `os_soft_path`: Specify the Opensearch installation directory.

##### Role dependencies
* `os_ngx_dept`: A boolean to determine whether or not to proxy web interface and API traffic using NGinx.

##### Listen port
* `os_port.rest`: TCP port for Opensearch REST.
* `os_port.transport`: TCP port for Opensearch transport port ragne.
* `os_port.exporter`: TCP port for Prometheus Exporter.
* `os_port.dashboards`: TCP port for Kibana server.
* `os_port.dashboards_exporter`: TCP port for Kibana exporter.
* `os_port.analyzer_web`: TCP port for performance analyzer WebService exposed.
* `os_port.analyzer_rpc`: TCP port for performance analyzer RPC Communication.
* `os_port.logstash_exporter`: TCP Port for Logstash exporter.
* `os_port.logstash_api`: TCP Port for the Logstash metrics REST endpoint.

##### Logstash parameters
* `os_logstash_arg.index_refresh_interval`: How often to perform a index refresh operation.
* `os_logstash_arg.pipeline_workers`: The number of workers.
* `os_logstash_arg.pipeline_batch_size`: How many events to retrieve from inputs before sending to workers.
* `os_logstash_arg.pipeline_batch_delay`: How long to wait in milliseconds while polling for the next event.
* `os_logstash_inputs_arg`: Define the global common inputs parameters.

##### NGinx parameters
* `os_ngx_domain`: Defines domain name.
* `os_ngx_port_http`: Defines NGinx HTTP listen port.
* `os_ngx_port_https`: Defines NGinx HTTPs listen port.

##### Opensearch parameters
* `os_arg.user`: Opensearch running user.
* `os_arg.action_destructive_requires_name`: Restricts deletions to specific names, instead of allowing the special _all or wildcard options.
* `os_arg.cluster_max_shards_per_node`: Controls the number of shards allowed in the cluster per data node.
* `os_arg.http_compression`: A boolean to determine whether or not to compression when possible with Accept-Encoding.
* `os_arg.http_cors_enabled`: Enable or disable cross-origin resource sharing.
* `os_arg.http_cors_allow_methods`: Which methods to allow.

##### Service Mesh
* `environments`: Define the service environment.
* `datacenter`: Define the DataCenter.
* `domain`: Define the Domain.
* `customer`: Define the customer name.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
- Ansible versions >= 2.8
- Python >= 2.7.5
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

    [siem:vars]
    os_cluster='siem'

    [siem]
    node01 ansible_host='192.168.1.10'
    node02 ansible_host='192.168.1.11'
    node03 ansible_host='192.168.1.12'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: all
  roles:
     - role: ansible-role-linux-opensearch
       os_cluster: 'siem'
```

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`.

```yaml
os_cluster: 'siem'
os_admin_pass: 'changeme'
os_rotate_day: '180'
os_heap_size: '3g'
os_logstash_heap_size: '2g'
os_memory_lock: false
os_dashboards_proxy: false
os_version: '1.1.0'
os_logstash_version: '7.13.2'
os_data_path: '/data'
os_soft_path: '/usr/share'
os_ngx_dept: true
os_port:
  rest: '9200'
  transport: '9300'
  exporter: '9108'
  dashboards: '5601'
  dashboards_exporter: '9684'
  analyzer_web: '9601'
  analyzer_rpc: '9650'
  logstash_exporter: '9198'
  logstash_api: '9600'
os_logstash_arg:
  index_refresh_interval: '30s'
  pipeline_workers: '2'
  pipeline_batch_size: '1000'
  pipeline_batch_delay: '100'
os_logstash_inputs_arg:
  common:
    - name: 'syslog-gelf'
      protocol: 'udp'
      port: '12201'
  azure:
    eventhub:
      storage: "DefaultEndpointsProtocol=https;AccountName=stlearnp001;AccountKey=UqqSKWXPrl07eTwbh6aTPC0qXnW6SZbE/XkGqVe/AmpExkwSeLW634CcgIKqpZ/bDbLg5lBKqN6ERkFTR8M8gQ==;EndpointSuffix=core.chinacloudapi.cn"
      eventhub: "Endpoint=sb://evhns-learn-prd-001.servicebus.chinacloudapi.cn/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=8k7bSp1lH5neIJZcdCnQkg5ZLjNWtLRNSxaa/P9ZLrU="
      hubs:
        - 'activity-logs'
        - 'logs-cluster-autoscaler'
        - 'logs-guard'
        - 'logs-kube-apiserver'
        - 'logs-kube-audit'
        - 'logs-kube-audit-admin'
        - 'logs-kube-controller-manager'
        - 'logs-kube-scheduler'
os_ngx_domain: 'siem.example.com'
os_ngx_port_http: '80'
os_ngx_port_https: '443'
os_arg:
  user: 'opensearch'
  action_destructive_requires_name: true
  cluster_max_shards_per_node: '10000'
  http_compression: true
  http_cors_enabled: true
  http_cors_allow_methods: 'HEAD, GET, POST, PUT, DELETE'
environments: 'prd'
datacenter: 'dc01'
domain: 'local'
customer: 'demo'
tags:
  subscription: 'default'
  owner: 'nobody'
  department: 'Infrastructure'
  organization: 'The Company'
  region: 'China'
exporter_is_install: false
consul_public_register: false
consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
consul_public_http_prot: 'https'
consul_public_http_port: '8500'
consul_public_clients:
  - '127.0.0.1'
```

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Donations
Please donate to the following monero address.

    46CHVMbb6wQV2PJYEbahb353SYGqXhcdFQVEWdCnHb6JaR5125h3kNQ6bcqLye5G7UF7qz6xL9qHLDSAY3baagfmLZABz75