Role Name
=========


Requirements
------------

A collection kubernetes.core should be installed. To accomplish this run the following command:

ansible-galaxy collection install kubernetes.core

See https://docs.ansible.com/ansible/latest/collections/kubernetes/core/docsite/kubernetes_scenarios/k8s_intro.html for more information

Role Variables
--------------

This role reads and stores data from ansible folder. Each cluster to backup should have it's own folder within ansible folder with a required data. 'cluster-name' should have a file with  kubeadmin-password and  kubeconfig files. Example:

├── cluster_name

│   └── auth

│       ├── kubeadmin-password

│       └── kubeconfig



Dependencies
------------

-

Example Playbook
----------------

---
- hosts: localhost
  vars:
    cluster_name: my_cluster
    domain_name: example.com
    manifests:

    - output_file: manifest1.yaml
      api_version_str: myapiVesrion/v1
      entity_kind: myKind1
      entity_name: myName1

    - output_file: manifest2.yaml
      api_version_str: myapiVesrion/v1
      entity_kind: myKind2
      entity_name: myName2
      entity_namespace: NameSpace2

  tasks:
    - name: Set output_file env w/o namespace param
      vars:
        output_file: "{{ item.output_file }}"
        api_version_str: "{{ item.api_version_str }}"
        entity_kind: "{{ item.entity_kind }}"
        entity_name: "{{ item.entity_name }}"
      include_role:
        name: clear_metadata
      when: item.entity_namespace is not defined
      loop: "{{ manifests }}"

    - name: Set output_file env with namespace param
      vars:
        output_file: "{{ item.output_file }}"
        api_version_str: "{{ item.api_version_str }}"
        entity_kind: "{{ item.entity_kind }}"
        entity_name: "{{ item.entity_name }}"
        entity_namespace: "{{ item.entity_namespace }}"
      include_role:
        name: clear_metadata
      when: item.entity_namespace is defined
      loop: "{{ manifests }}"


License
-------

BSD

Author Information
------------------

Mikhail Klimov
git: git@github.com:mikhailklimov1/clear_metadata.git
