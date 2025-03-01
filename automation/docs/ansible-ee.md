Certainly! Here's the updated documentation, excluding references to `ansible-navigator.yml`. The focus is solely on setting up an **Ansible Execution Environment (EE)** using the required files: `execution-environment.yml`, `requirements.yml`, and `requirements.txt`.

---

# Ansible Execution Environment Documentation

This guide provides a detailed walkthrough of setting up an **Ansible Execution Environment (EE)**. An EE is a containerized environment that includes all necessary tools, dependencies, and configurations to run Ansible reliably and consistently across systems.

---

## What is an Execution Environment?

An **Execution Environment (EE)** is a portable, containerized environment for running Ansible workflows. It encapsulates:
- The Ansible runtime.
- Required Python libraries.
- Ansible Galaxy collections (modules, plugins, and roles).
- Custom configurations for automation workflows.

This ensures consistent and reproducible automation, regardless of the underlying infrastructure.

---

## Files Required to Setup an Execution Environment

To create an Ansible EE, you need the following files:

1. **`execution-environment.yml`:** Defines how the EE container is built, including dependencies and base image.
2. **`requirements.yml`:** Lists the Ansible Galaxy collections to install in the EE.
3. **`requirements.txt`:** Lists Python libraries to include in the EE.

Each file's purpose, role, and creation steps are detailed below.

---

## Step 1: Create `execution-environment.yml`

This is the primary configuration file for building the EE container. It specifies the base image and lists the dependency files.

### **Purpose of This File**:
- Defines how the EE is built.
- Specifies the base container to use (e.g., `quay.io/ansible/creator-ee:latest`).
- Links to the files (`requirements.yml` and `requirements.txt`) for dependencies.
- Allows additional customization steps during the build process.

### **File Content**:
```yaml
---
version: 3  # Schema version for EE
dependencies:
  galaxy: requirements.yml  # File for Ansible Galaxy collections (see Step 2)
  python: requirements.txt  # File for Python library dependencies (see Step 3)
additional_build_steps:
  prepend_base:
    - RUN ln -s /usr/bin/microdnf /usr/bin/dnf  # Compatibility fix for the base image
images:
  base_image:
    name: quay.io/ansible/creator-ee:latest  # Base image to build the EE
```

### **Create the File**:
Run the following command to create the file:
```bash
cat << EOF > execution-environment.yml
---
version: 3
dependencies:
  galaxy: requirements.yml
  python: requirements.txt
additional_build_steps:
  prepend_base:
    - RUN ln -s /usr/bin/microdnf /usr/bin/dnf
images:
  base_image:
    name: quay.io/ansible/creator-ee:latest
EOF
```

---

## Step 2: Create `requirements.yml`

This file lists the **Ansible Galaxy collections** that must be included in the EE. Collections extend Ansible functionality by bundling modules, plugins, and roles.

### **Purpose of This File**:
- Ensures specific Ansible collections are available in the EE.
- Enables a modular approach to automation by reusing pre-built components from Galaxy.

### **File Content**:
```yaml
---
collections:
  - name: ansible.netcommon         # Collection for networking support
  - name: f5networks.f5_modules     # F5 BIG-IP modules for managing network devices
  - name: f5networks.f5_bigip       # Collection for BIG-IP-specific automation
  - name: kubernetes.core           # Kubernetes modules for managing clusters
  - name: ansible.utils             # Useful utilities (e.g., string manipulation)
  - name: ansible.posix             # Modules for Linux/Unix-based systems
  - name: community.general         # Community-supported modules for various tasks
```

### **Create the File**:
Run the following command to create the file:
```bash
cat << EOF > requirements.yml
---
collections:
  - name: ansible.netcommon
  - name: f5networks.f5_modules
  - name: f5networks.f5_bigip
  - name: kubernetes.core
  - name: ansible.utils
  - name: ansible.posix
  - name: community.general
EOF
```

---

## Step 3: Create `requirements.txt`

This file lists the **Python libraries** required inside the EE. These libraries support Ansible modules and custom automation workflows.

### **Purpose of This File**:
- Makes Python dependencies available within the EE container for Ansible modules and scripting.
- Ensures compatibility with specific Ansible-related Python integrations.

### **File Content**:
```plaintext
ansible
ansible-runner
cryptography
objectpath
ordereddict
simplejson
paramiko
jinja2
netaddr
packaging
kubernetes
kubernetes-validate
PyYAML
jsonpatch
```

### **Create the File**:
Run the following command to create the file:
```bash
cat << EOF > requirements.txt
ansible
ansible-runner
cryptography
objectpath
ordereddict
simplejson
paramiko
jinja2
netaddr
packaging
kubernetes
kubernetes-validate
PyYAML
jsonpatch
EOF
```

---

## Building the Execution Environment

Once you've created `execution-environment.yml`, `requirements.yml`, and `requirements.txt`, use `ansible-builder` to build the execution environment container.

### **Prerequisites**:
Ensure the following tools are installed on your system:
1. **`ansible-builder`**: For building the EE.
2. A container engine (e.g., **`podman`** or **`docker`**).

Install `ansible-builder` if needed:
```bash
pip install ansible-builder
```

### **Command to Build the EE**:
Run the following command:
```bash
ansible-builder build --tag ansible-f5-ee:latest --container-engine podman
```

- **`--tag ansible-f5-ee:latest`**: Tags the resulting EE image for reference.
- **`--container-engine podman`**: Uses Podman as the container engine (use `docker` if available).

---

## Verifying and Using the Execution Environment

### ** Verify the EE Image**:
After the build completes, check if the container image has been created:
```bash
podman images | grep ansible-f5-ee  # If using Podman
docker images | grep ansible-f5-ee  # If using Docker
```

You should see the `ansible-f5-ee:latest` image listed.

---

## Summary of Files and Their Roles

1. **`execution-environment.yml`**:  
   Defines the EE build process, linking to the dependency files and setting the base image.

2. **`requirements.yml`**:  
   Specifies the required Ansible Galaxy collections that extend Ansible automation capabilities.

3. **`requirements.txt`**:  
   Lists Python dependencies needed for Ansible modules, plugins, and custom tasks.

---

## Benefits of Using an Execution Environment:

- **Reproducibility:** Ensures all dependencies are consistent across environments.
- **Portability:** Encapsulates your Ansible runtime in a container, making it reusable anywhere.
- **Simplifies Collaboration:** Teams only need to pull and run the EE container with no manual dependency management.

---

These steps ensure you are equipped to build and manage a robust Ansible Execution Environment that standardizes your automation processes!