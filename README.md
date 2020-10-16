# wcm_io_devops.conga_bundle_files

This role copies bundle specific files into bundle folders based upon
the configuration generated by CONGA. This role is for example used by
[wcm_io_devops.conga_aem_cms](https://github.com/wcm-io-devops/ansible-conga-aem-cms) to deploy the `hmac` and `master` files for the
`com.adobe.granite.crypto.file` bundle.

> This role was developed as part of the
> [wcm.io DevOps Ansible Automation for AEM](http://devops.wcm.io/ansible-aem/)
> to integrate Ansible with
> [CONGA](http://devops.wcm.io/conga/).

## Requirements

This role requires Ansible 2.7 or higher.

## Role Variables

Available variables are listed below, along with default values:

    conga_bundle_files_aem_user: "{{ conga_config.quickstart.adminUser.username | default('admin')}}"

The username to use for the Bundle API call.

    conga_bundle_files_aem_password: "{{ conga_config.quickstart.adminUser.password | default('admin')}}"

The password to use for the Bundle API call.

    conga_bundle_files_aem_port: "{{ conga_config.quickstart.port | default(4502) }}"

The port of the target AEM instance. This can be set
explicitly, but the idea is to reuse it from a CONGA role in which the
port is defined (e.g.
[`aem-cms`](https://github.com/wcm-io-devops/conga-aem-definitions/blob/develop/conga-aem-definitions/src/main/roles/aem-cms.yaml))
to make it automatically work for both author and publisher instances.

    conga_bundle_files_aem_api_prefix: "http://localhost:{{ conga_bundle_files_aem_port }}/system/console/bundles/"

The prefix of the bundle API call made to retrieve the bundle id and
thus the target path where to deploy the file.

    conga_bundle_files_aem_api_timeout: 60

Timeout in seconds for the AEM api call

    conga_bundle_files_target_dir_prefix: "crx-quickstart/launchpad/felix/bundle"
    
The prefix of the bundle target path. This value is concatenated with
the bundle id retrieved from the bundle API call.
    
    conga_bundle_files_target_data_dir: "data"

The subdirectory inside the bundle directory.

    conga_bundle_files_standalone: true

Enables/disables standalone mode. When set to true the
[wcm_io_devops.aem_service](https://github.com/wcm-io-devops/ansible-aem-service)
dependency is enabled. Set this value to false when you have several
aem-service dependencies in your play to avoid multiple AEM restarts.

## Dependencies

This role depends on the
[wcm_io_devops.conga_facts](https://github.com/wcm-io-devops/ansible-conga-facts) role
for supplying the list of bundle files to deploy. In the standalone mode
it also depends on the
[wcm_io_devops.aem_service](https://github.com/wcm-io-devops/ansible-aem-service) role
for ensuring that the target AEM instance is started before calling the
API.

## Example

Includes the role to copy bundle files to
crx-quickstart/launchpad/felix/bundle<id>/data and restart aem
afterwards when the files have changed.

    - include_role: 
        name: wcm_io_devops.conga_bundle_files
      vars: 
        conga_bundle_files_base_path: "{{ conga_config.quickstart.rootPath }}",
        conga_bundle_files_owner: "{{ aem_cms_user }}",
        conga_bundle_files_group: "{{ aem_cms_group }}",
        conga_bundle_files_mode: "0600",
        conga_bundle_files_handlers: [ restart aem ]

## License

Apache 2.0
