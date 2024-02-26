
- [Ansible中文权威指南](www.ansible.com.cn/docs/playbooks_roles.html)

- [ansible-playbook定义变量与使用](vars.md)

- [ansible使用教程（4W字长文，保姆级别教程，建议收藏）](https://blog.csdn.net/A_art_xiang/article/details/120524817)

- [ansible.builtin.find module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/find_module.html)

- [Ansible全套详细教程](https://blog.csdn.net/qq_43355223/article/details/88111875?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-88111875-blog-120524817.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-88111875-blog-120524817.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=2)

## 

### group_vars
The group_vars in Ansible are a convenient way to apply variables to multiple hosts at once. Group_vars is an Ansible-specific folder as part of the repository structure. This folder contains YAML files created to have data models, and these data models apply to all the devices listed in the hosts.ini file. We can apply group variables to logical functions and hardware platforms. The variables should include a common identifier as the naming convention, such as platform_ and group_. A few examples of group_vars candidates include Netflow, Domain name, Multicast, Access control lists, Banners, and QoS policies.

### host_vars
The host_vars is a similar folder to group_vars in the repository structure. It contains data models that apply to individual hosts/devices in the hosts.ini file. Hence, there is a YAML file created per device containing specific information about that device.

The variables should be prepended with hosts_ for the naming convention, as well as to ensure better readability.

## 主机自动建立ssh信任
大数据环境下，为了安装、配置和维护的方便，一般会设置管理机（安装ansible的机器）和每个集群节点之间的无密码登录（单向信任），而无密码登录最简单的方式是通过设置ssh公私钥认证机制，下面playbook脚本可以完成管理机到远程主机组hostall的无密码登录，脚本内容如下：

- hosts: hostall
  gather_facts: no
  roles:
   - roles
  tasks:
   - name: close ssh yes/no check
     lineinfile: path=/etc/ssh/ssh_config regexp='(.*)StrictHostKeyChecking(.*)' line="StrictHostKeyChecking no"
   - name: delete /root/.ssh/
     file: path=/root/.ssh/ state=absent
   - name: create .ssh directory
     file: dest=/root/.ssh mode=0600 state=directory
   - name: generating local public/private rsa key pair
     local_action: shell ssh-keygen -t rsa -b 2048 -N '' -f /root/.ssh/id_rsa
   - name: view id_rsa.pub
     local_action: shell cat /root/.ssh/id_rsa.pub
     register: sshinfo
   - set_fact: sshpub={{sshinfo.stdout}}
   - name: add sshkey
     local_action: shell echo {{sshpub}} > {{AnsibleDir}}/roles/templates/authorized_keys.j2
   - name: copy authorized_keys.j2 to all hosts
     template: src={{AnsibleDir}}/roles/templates/authorized_keys.j2 dest=/root/.ssh/authorized_keys mode=0600
     tags:
     - copy sshkey

这个playbook稍微复杂一些，它仍然用到了角色变量，所以此脚本要放在/etc/ansible目录下，脚本一开始通过lineinfile模块对远程主机上的sshd配置文件ssh_config进行文件内容替换，这个替换是关闭ssh第一次登陆时给出的“yes/no”提示，接着在远程主机上删除/root/.ssh目录，并重新创建此目录，这个操作的目的是确保远程主机/root/.ssh目录是干净的、权限正确的。然后通过local_action模块在管理机上生成一对公私钥，同时将生成的公钥文件内容作为变量sshinfo的值，并通过set_fact模块重新定义一个变量sshpub，此变量引用sshinfo变量的stdout输出，也就是最终的公钥值，紧接着，将变量sshpub的内容写入管理机authorized_keys.j2模板文件中，最后，使用template模块将authorized_keys.j2模板文件拷贝到每个远程主机的/root/.ssh目录下，并重命名为authorized_keys，同时给文件授于属主读、写权限。

将此脚本放到/etc/ansible目录下，并命名为ssh.yml，然后执行如下命令：

``
[root@server239 ansible]# ansible-playbook  ssh.yml
``
## Handlers
handlers=tasks，处理程序可以做任务可以完成的任何事，但是只有当另一个task调用它的时候才会执行。
我们添加notify指令到上面的playbook中：
```
- hosts: remote
  become: yes
  become_user: root
  tasks:
	  - name: Install Nginx
		apt:
			name: nginx
			state: present
			update_cache: true
		notify:
			- Start Nginx
	handlers:
		- name: Start Nginx
		service:
			name: nginx
			state: started
```

## roles（最终使用方法，比playbook更好配置）
roles角色用于组织多个task并封装成完成这些任务需要的数据，但是真实的配置场景里面，我们需要很多变量，文件，模板之类的东西，虽然可以配合playbook使用，但是roles更好，因为它有一个目录结构来规范

roles
  rolename
   - files
   - handlers
   - meta
   - templates
   - tasks
   - vars

我们下面创建一个新的role例子，role的任务有：

1. Add Nginx Repository- 使用apt_repository模块添加Nginx稳定PPA以获取最新的稳定版本的Nginx 。
2. Install Nginx - 使用Apt模块安装Nginx。
3. Create Web Root - 最后创建一个Web根目录。

在每一个子目录中，ansible都会自动找到并读取那个叫main.yml的文件。

创建角色
进入本地ansible-test文件夹，激活虚拟环境：

mkdir nginx  # 这里最好就使用roles这个名字
cd nginx  
ansible-galaxy init nginx
然后进入nginx，会发现整个role目录已经建立好了。

1. files
里面放置我们需要复制到服务器中的文件


2. handlers
记得放在main.yml文件中：

---
- name: Start Nginx
  service:
  	name: nginx
  	state: started
  
 - name: Reload Nginx
   service:
   	name: nginx
   	state: reloaded

前面提到了handler需要被task调用才起作用，我们可以在其他的YAML文件中调用它们


3. meta
这个目录里面的main.yml用于描述角色之间的依赖关系，比如nginx role依赖于ssl role，那么：

---
dependencies:
	- {role: ssl}
这样调用nginx角色时，自动会先调用ssl角色
如果无任何依赖：
dependencies: []


4. Template
这里面不需要有main.yml文件，这里的文件使用的是Jinja2模板引擎的.j2文件
比如我们新建一个文件为serversforhackers.com.conf.j2：

server {
    # Enforce the use of HTTPS
    listen 80 default_server;
    server_name {{ domain }};
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl default_server;

    root /var/www/{{ domain }}/public;
    index index.html index.htm index.php;

    access_log /var/log/nginx/{{ domain }}.log;
    error_log  /var/log/nginx/{{ domain }}-error.log error;

    server_name {{ domain }};

    charset utf-8;

    include h5bp/basic.conf;

    ssl_certificate           {{ ssl_crt }};
    ssl_certificate_key       {{ ssl_key }};
    include h5bp/directive-only/ssl.conf;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location = /favicon.ico { log_not_found off; access_log off; }
    location = /robots.txt  { log_not_found off; access_log off; }

    location ~ \.php$ {
        include snippets/fastcgi.conf;
        fastcgi_pass unix:/var/run/php7.1-fpm.sock;
    }
}
这是一个标准的php应用的Nginx配置文件，里面有一些变量

## 语法检查
nsible-playbook -v --syntax-check fsgui.yml

## debug
tasks:
          - name: Display Host Variable from hostfile
            debug: msg="The {{ inventory_hostname }} Vaule is {{ key }}"

## 在playbook文件内使用vars_files
我们还可以在playbook文件内通过vars_files字段引用变量，首先把所有的变量定义到某个文件内，然后在playbook文件内使用vars_files参数引用这个变量文件，我们再回到之前的variabled.yaml文件：

---
  - hosts: all
    gather_facts: False
    vars_files:
        - var.yaml
    tasks:
          - name: Display Host Variable from hostfile
            debug: msg="The {{ inventory_hostname }} Vaule is {{ key }}"

## 另一种copy

--- 
- hosts: test
  remote_user: root
  gather_facts: false
  become: yes
  tasks: 
    - name: "copy from target cpu"
      copy: 
        src: "{{item.src}}"
        dest: "{{item.dest}}"
        owner: root
        group: root
        mode: 775
      with_items: 
        - {src: "/home/dist.zip",dest: "/home/www/admin/"}
        - {src: "/home/www/dist.zip",dest: "/home/www/www/"}


## 显示详细信息 -v
 ansible-playbook -i hosts -v fsgui.yml