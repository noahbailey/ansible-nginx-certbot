# ansible-nginx-certbot
Setup a simple reverse proxy with Certbot TLS 

Components: 
* Nginx web server and front end proxy
* Certbot TLS certificate authority
* CloudFlare DNS service

This role takes data from ansible variables and uses it to configure a basic Nginx reverse proxy server. 

## Usage

Add this role to the `roles` directory in your ansible project.

Then, include the role using a top level playbook:

```yaml
- name: Proxy servers
  hosts: proxy-servers
  become: true 
  roles: 
  - ansible-nginx-certbot
```

## Variables

This role requires these variables to exist in inventory:

#### 1. Sites 

This is a list of sites or web applications that will run behind the proxy server. It is formatted as a list of mappings. 

Multiple backend servers can be defined. By default, it will configure src address load balancing when there is more than one backend server defined. 

```yaml
sites: 
- name: app1.gablogianartcollection.org
  backends: 
    - host: 10.10.10.11
    - host: 10.10.10.12
```

#### 2. Cloudflare Account

To create DNS records a CloudFlare global API key is required. 

```yaml
cloudflare: 
  api_key: aaaaaazzzzzz
  email:  ongo@gablogianartcollection.org
  domain: gablogianartcollection.org
```

Since the API key essentially grants full access to the account it should be stored securely in Ansible Vault before it is added to a git repository. 