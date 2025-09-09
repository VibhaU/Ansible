# Ansible
Ansible practices in EC2 instances with following commands

 1  apt update
    2  apt upgrade -1
    3  apt upgrade -y
    4  apt install -y software-properties-common
    5  sudo add-apt-repository --yes --update ppa:ansible/ansible
    6  apt install -y ansible
    7  apt install unzip -y
    8  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o
    9  "awscliv2.zip"
   10  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   11  ls
   12  unzip awscliv2.zip
   13  ls
   14  sudo ./aws/install
   15  aws --version
   16  aws configure
   17  aws sts get-caller-identity
   18  apt install -y python3-pip
   19  apt install -y python3-venv
   20  python3 -m venv ~/ansible-venv
   21  source ~/ansible-venv/bin/activate
   22  pip install --upgrade pip
   23  pip install boto3 botocore
   24  ansible-galaxy collection install amazon.aws
   25  ansible galaxy collection list | grep amzon.aws
   26  ansible-galaxy collection list | grep amzon.aws
   27  mkdir ec2
   28  cd ec2
   29  nano inventory.ini
   30  nono ec2.yml
   31  nano ec2.yml
   32  ls 
   33  # ec2.yml
   34  ---
   35  - name: Launch two EC2 instances
   36  hosts: localhost
   37  connection: local
   38  gather_facts: no
   39  vars:
   40  aws_region: us-east-1
   41  instance_type: t2.micro
   42  key_name: my-ansible-key
   43  security_group: my-ansible-sg
   44  image_id: ami-id # Update id
   45  tasks:
   46  - name: Create key pair
   47  amazon.aws.ec2_key:
   48  name: "{{ key_name }}"
   49  region: "{{ aws_region }}"
   50  state: present
   51  register: keypair
   52  - name: Save private key locally
   53  copy:
   54  content: "{{ keypair.key.private_key }}"
   55  dest: "~/.ssh/{{ key_name }}.pem"
   56  mode: '0600'
   57  when: keypair.changed
   58  - name: Create security group
   59  amazon.aws.ec2_security_group:
   60  name: "{{ security_group }}"
   61  description: "Allow SSH and HTTP"
   62  region: "{{ aws_region }}"
   63  rules:
   64  - proto: tcp
   65  from_port: 22
   66  to_port: 22
   67  cidr_ip: 0.0.0.0/0
   68  - proto: tcp
   69  from_port: 80
   70  to_port: 80
   71  cidr_ip: 0.0.0.0/0
   72  rules_egress:
   73  - proto: -1
   74  from_port: 0
   75  to_port: 0
   76  cidr_ip: 0.0.0.0/0
   77  - name: Launch EC2 instances
   78  amazon.aws.ec2_instance:
   79  name: "ansible-demo"
   80  key_name: "{{ key_name }}"
   81  instance_type: "{{ instance_type }}"
   82  image_id: "{{ image_id }}"
   83  region: "{{ aws_region }}"
   84  count: 2
   85  wait: yes
   86  security_group: "{{ security_group }}"
   87  register: ec2
   88  - name: Show public IPs
   89  debug:
   90  var: ec2.instances | map(attribute='public_ip_address
   91  ansible-playbook -i inventory.ini ec2.yml
   92  nano ec2.yml
   93  ansible-playbook -i inventory.ini ec2.yml
   94  nano ec2.yml
   95  ansible-playbook -i inventory.ini ec2.yml
   96  nano inventory.ini
   97  nano createS3.yml
   98  ansible-playbook -i inventory.ini createS3.yml
   99  nano deleteS3.yml
  100  ansible-playbook -i inventory.ini deleteS3.yml
  101  nano deleteterra.yml
  102  ls
  103* 
  104  nano deleteterra.yml
  105  ansible-playbook -i inventory.ini deleteterra.yml
  106  ls
  107  git init
  108  git remote add origin https://github.com/VibhaU/Ansible.git
  109  git remote -v
  110  git config --global user.name "Vibha"
  111  git config --global user.email "urankar.vibha@gmail.com"
  112  git add .
  113  git status
  114  git commit -m "adding ansible file"
  115  git branch
  116  git push origin master
  117  mkdir git
  118  cd git
  119  git clone https://github.com/VibhaU/Ansible.git
  120  ls
  121  cd ..
  122  ls
  123  cp -r ec2.yml git/Ansible
  124  cd git/Ansible
  125  ls
  126  git
  127  git status
  128  git add .
  129  git commit -m "added ec2.yml"
  130  git branch
  131  git push origin main
  132  git add .
  133  ls
  134  cd..
  135  cd ..
  136  ls
  137  cp -r createS3.yml deleteS3.yml deleteterra.yml inventory.ini git/Ansible
  138  cd Ansible
  139  cd git/Ansible
  140  ls
  141  git add .
  142  git commit -m "adding create and dlet"
  143  git push origin main
  144  history  
