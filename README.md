# Ansible

- [WebApp 1 - Basic](WebApplicaitonV1)
- [WebApp 2 - Seperate Servers](WebApplicaitonV2)
- [WebApp 3 - Roles](WebApplicaitonV3)
- [WebApp 4 - Async, Poll and Little Handler](WebApplicaitonV4)
- [WebApp 5 - Strategies (plugin)](WebApplicaitonV5)
- [WebApp 6 - Error Handling](WebApplicaitonV6)
- [WebApp 7 - Jinja2 Templating](WebApplicaitonV7)
- [WebApp 8 - Lookups (plugin)](WebApplicaitonV8)
- [WebApp 9 - Vault](WebApplicaitonV9)
- [WebApp 10 - Dynamic Inventry](WebApplicaitonV10)
- [WebApp 11 - Custom Module and Plugin](WebApplicaitonV11)
- [WebApp 12 - Meta](WebApplicaitonV13)
- [WebApp 13 - VirtualBox, VmWare and Vagrant](WebApplicaitonV14)
- [WebApp 14 - Azure and Digital Ocean](WebApplicaitonV15)


## Ansible Tutorial

- https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html

- [LAMP Example](/LAMP/readme.md)

- https://github.com/devopsschool-sample-programs/ansible-roles-example-collections

- https://github.com/Asymmetrik/ansible-roles


## Comparison

- https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c

- https://phoenixnap.com/blog/ansible-vs-terraform-vs-puppet

- https://codefol.io/posts/deployment-versus-provisioning-versus-orchestration/

- https://www.youtube.com/watch?v=II4PFe9BbmE

- https://www.youtube.com/watch?v=vh1YtWjfcyk

## Prvision vs Configuration

Not by a long shot. You’ve provisioned a load balancing service, yes. But is it configured? Probably not. In addition to the basic networking needed (IP addresses, VLANs, etc…) there are also a whole bunch of very application specific things that must be configured. There’s no “golden” load balancing service that can be generically applied to your application. Period. At a minimum the virtual server (that represents your application to the rest of the world) needs two things: an IP address and a pool of app instances to select from.

That means there’s some configuration that must occur after the provisioning. 

# Configuration Drift

Configuration drift is caused by inconsistent configuration items (CIs) across computers or devices. Configuration drift occurs naturally in data center environments when changes to software and hardware are made ad hoc and are not recorded or tracked in a comprehensive and systematic fashion.

Configuration drift accounts for many high availability and disaster recovery system failures. To prevent configuration drift, administrators should maintain detailed information about the network addresses of hardware devices as well as what software versions are running on them and which updates have been applied.

Special configuration management database (CMDB) software is available to help administrators prevent configuration drift.

- Mutable vs Immutable Infra
  
Configuration management tools such as Chef, Puppet, Ansible, and SaltStack typically default to a mutable infrastructure paradigm. For example, if you tell Chef to install a new version of OpenSSL, it’ll run the software update on your existing servers and the changes will happen in-place. Over time, as you apply more and more updates, each server builds up a unique history of changes. This often leads to a phenomenon known as configuration drift, where each server becomes slightly different than all the others, leading to subtle configuration bugs that are difficult to diagnose and nearly impossible to reproduce.

If you’re using a provisioning tool such as Terraform to deploy machine images created by Docker or Packer, then every “change” is actually a deployment of a new server (just like every “change” to a variable in functional programming actually returns a new variable). For example, to deploy a new version of OpenSSL, you would create a new image using Packer or Docker with the new version of OpenSSL already installed, deploy that image across a set of totally new servers, and then undeploy the old servers. This approach reduces the likelihood of configuration drift bugs, makes it easier to know exactly what software is running on a server, and allows you to trivially deploy any previous version of the software at any time. Of course, it’s possible to force configuration management tools to do immutable deployments too, but it’s not the idiomatic approach for those tools, whereas it’s a natural way to use provisioning tools.

https://www.youtube.com/watch?v=II4PFe9BbmE

## Procedural vs Declerative

Chef and Ansible encourage a procedural style where you write code that specifies, step-by-step, how to to achieve some desired end state. Terraform, CloudFormation, SaltStack, and Puppet all encourage a more declarative style where you write code that specifies your desired end state, and the IAC tool itself is responsible for figuring out how to achieve that state.

For example, let’s say you wanted to deploy 10 servers (“EC2 Instances” in AWS lingo) to run v1 of an app. Here is a simplified example of an Ansible template that does this with a procedural approach:


```
- ec2:
    count: 10
    image: ami-v1    
    instance_type: t2.micro
```

And here is a simplified example of a Terraform template that does the same thing using a declarative approach:

```
resource "aws_instance" "example" {
  count         = 10
  ami           = "ami-v1"
  instance_type = "t2.micro"
}
```

Now at the surface, these two approaches may look similar, and when you initially execute them with Ansible or Terraform, they will produce similar results. The interesting thing is what happens when you want to make a change.

For example, imagine traffic has gone up and you want to increase the number of servers to 15. With Ansible, the procedural code you wrote earlier is no longer useful; if you just updated the number of servers to 15 and reran that code, it would deploy 15 new servers, giving you 25 total! So instead, you have to be aware of what is already deployed and write a totally new procedural script to add the 5 new servers: