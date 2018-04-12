
===========
Ansible Playbook for setting up the ELK Stack in TDRIVE2 TEST Enviorment

![ELK](/image/ansible-elk.png?raw=true)

## What does it do?
   - Automated deployment of a full ELK stack (Elasticsearch, Logstash/Fluentd, Kibana)
     * 6.x ELK versions are maintained.
     * Uses Nginx as a reverse proxy for Kibana
     * Adds either iptables or firewalld rules if firewall is active
     * Tunes Elasticsearch heapsize to half your memory, to a max of 2G
     * All service ports can be modified in ```install/group_vars/all.yml```
     * Optionally install [curator](https://www.elastic.co/guide/en/elasticsearch/client/curator/current/index.html)


## Requirements
   - Ubuntu14.04 LTS or RHEL server/client with no modifications
   - ELK server with at least 8G of memory but in TDRIVE On0demand basis we are going with 4G.


## Notes
   - Current ELK version is 6.x series
   - Sets the nginx htpasswd to admin/admin initially
   - nginx ports default to 80/8080 for Kibana and SSL cert retrieval (configurable)
   - Uses OpenJDK for Java8
   - It's fairly quick, takes around 3minutes on test VM
     - ```install_elasticsearch: true```
     - ```install_kibana: true```
     - ```install_logstash: true```
     - Note: Deploying X-Pack will wrap your ES with additional authentication and security, Kibana for example will have it's own credentials now - the default is username: ```elastic``` and password: ```changeme```
       But we are not going with X-Pack solution.
## ELK Server Instructions
   - Clone repo and setup your hosts file
```
git clone Url
cd tdrive-ansible-elk
sed -i 's/host-01/es1/' hosts
sed -i 's/host-02/es2/' hosts
```
   - If you're using a non-root user for Ansible, e.g. AWS EC2 likes to use ec2-user then set the follow below, default is root.

```
ansible_system_user: ec2-user
```

   - Run the playbook
```
ansible-playbook -i hosts elk.yml
```
   - (see playbook messages)
   - Navigate to the ELK at http://host-01:80
   - Default login is:
      - username: ```admin```
      - password: ```admin```

![ELK](/image/elk-index-5.x-1.png?raw=true "Select @timestamp from drop-down.")

![ELK](/image/elk-index-5.x-2.png?raw=true "Click the blue create button.")

![ELK](/image/elk-index-5.x-3.png?raw=true "Click Discover")

## ELK Client Instructions
   - Run the client playbook against the generated ``elk_server`` variable
```
   - Once this completes return to your ELK and you'll see log results come in from ELK/EFK clients via filebeat
![ELK](/image/elk-index-5.x-4.png?raw=true "watch the magic")





## File Hierarchy
```
.

