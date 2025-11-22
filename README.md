# Student Registration Webapp Operations Repository

This repository contains Ansible playbook and roles to automate the installation and configuration of a Java and Tomcat environment for the Student Registration web application. The playbook is designed to run on Linux servers (e.g., Amazon EC2 instances) and include all necessary steps to deploy and configure Tomcat with Java.

---

## Repository Structure

```
student-reg-webapp-ops/
├── hosts                  # Inventory file listing target servers
├── install_tomcat.yaml    # Main playbook for installing Tomcat and Java
├── tomcat/                # Ansible role for Tomcat installation
│   ├── tasks/
│   │   └── main.yaml      # Tasks for preparing environment, installing Java & Tomcat
│   ├── handlers/
│   │   └── main.yaml      # Handlers such as Restart Tomcat
│   ├── meta/
│   │   └── main.yaml      # Role metadata (dependencies, author, license)
│   ├── templates/
│   │   └── config.j2      # Systemd service template for Tomcat
│   └── vars/               # Optional variables for the role
└── README.md
```

---

## Requirements

* Ansible 2.15 or newer
* Python 3.x on the target hosts
* SSH access to target servers
* Sufficient privileges (sudo) to install packages, create directories, and configure systemd

---

## Variables

The role uses several variables to configure Java and Tomcat installation:

| Variable               | Description                          |
| ---------------------- | ------------------------------------ |
| `java_url`             | URL to download the Java JDK archive |
| `java_archive`         | Name of the Java archive file        |
| `java_dir`             | Directory to extract Java            |
| `tomcat_url`           | URL to download Tomcat archive       |
| `tomcat_archive`       | Name of the Tomcat archive file      |
| `tomcat_dir`           | Directory to extract Tomcat          |
| `tomcat_user_name`     | User who will own Tomcat and Java    |
| `tomcat_group_name`    | Group for Tomcat files               |
| `tomcat_users_file`    | Path to `tomcat-users.xml`           |
| `tomcat_user`          | Admin username for Tomcat            |
| `tomcat_user_password` | Password for admin user              |
| `tomcat_user_roles`    | Roles assigned to admin user         |

You can define these variables in `vars/main.yml` or pass them via extra-vars during playbook execution.

---

## Usage

1. **Clone the repository**

```bash
git clone https://github.com/onyekaoffodum/student-reg-webapp-ops.git
cd student-reg-webapp-ops
```

2. **Update Inventory**

Edit the `hosts` file to include the target servers for deployment:

```
[tomcatServers]
your_server_ip ansible_user=ec2-user
```

3. **Run the playbook**

```bash
ansible-playbook -i hosts install_tomcat.yaml --ask-become-pass
```

This will:

* Ensure `/opt` exists and required packages are installed
* Download and install Java JDK
* Download and install Apache Tomcat
* Configure environment variables
* Enable remote access to Manager and Host Manager
* Create a systemd service for Tomcat using the `config.j2` template
* Restart Tomcat automatically using handlers

---

## Notes

* The Tomcat should now be accessible via `http://<server_ip>:8080`.

---
