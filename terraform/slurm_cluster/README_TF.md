# slurm_cluster

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
Copyright (C) SchedMD LLC.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | ~> 1.0 |
| <a name="requirement_random"></a> [random](#requirement\_random) | ~> 3.0 |

## Providers

No providers.

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_slurm_controller_hybrid"></a> [slurm\_controller\_hybrid](#module\_slurm\_controller\_hybrid) | ./modules/slurm_controller_hybrid | n/a |
| <a name="module_slurm_controller_instance"></a> [slurm\_controller\_instance](#module\_slurm\_controller\_instance) | ./modules/slurm_controller_instance | n/a |
| <a name="module_slurm_controller_template"></a> [slurm\_controller\_template](#module\_slurm\_controller\_template) | ./modules/slurm_instance_template | n/a |
| <a name="module_slurm_login_instance"></a> [slurm\_login\_instance](#module\_slurm\_login\_instance) | ./modules/slurm_login_instance | n/a |
| <a name="module_slurm_login_template"></a> [slurm\_login\_template](#module\_slurm\_login\_template) | ./modules/slurm_instance_template | n/a |
| <a name="module_slurm_partition"></a> [slurm\_partition](#module\_slurm\_partition) | ./modules/slurm_partition | n/a |

## Resources

No resources.

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_cgroup_conf_tpl"></a> [cgroup\_conf\_tpl](#input\_cgroup\_conf\_tpl) | Slurm cgroup.conf template file path. | `string` | `null` | no |
| <a name="input_cloud_parameters"></a> [cloud\_parameters](#input\_cloud\_parameters) | cloud.conf options. | <pre>object({<br>    no_comma_params = bool<br>    resume_rate     = number<br>    resume_timeout  = number<br>    suspend_rate    = number<br>    suspend_timeout = number<br>  })</pre> | <pre>{<br>  "no_comma_params": false,<br>  "resume_rate": 0,<br>  "resume_timeout": 300,<br>  "suspend_rate": 0,<br>  "suspend_timeout": 300<br>}</pre> | no |
| <a name="input_cloudsql"></a> [cloudsql](#input\_cloudsql) | Use this database instead of the one on the controller.<br>* server\_ip : Address of the database server.<br>* user      : The user to access the database as.<br>* password  : The password, given the user, to access the given database. (sensitive)<br>* db\_name   : The database to access. | <pre>object({<br>    server_ip = string<br>    user      = string<br>    password  = string # sensitive<br>    db_name   = string<br>  })</pre> | `null` | no |
| <a name="input_compute_startup_scripts"></a> [compute\_startup\_scripts](#input\_compute\_startup\_scripts) | List of scripts to be ran on compute VM startup. | <pre>list(object({<br>    filename = string<br>    content  = string<br>  }))</pre> | `[]` | no |
| <a name="input_compute_startup_scripts_timeout"></a> [compute\_startup\_scripts\_timeout](#input\_compute\_startup\_scripts\_timeout) | The timeout (seconds) applied to each script in compute\_startup\_scripts. If<br>any script exceeds this timeout, then the instance setup process is considered<br>failed and handled accordingly.<br><br>NOTE: When set to 0, the timeout is considered infinite and thus disabled. | `number` | `300` | no |
| <a name="input_controller_hybrid_config"></a> [controller\_hybrid\_config](#input\_controller\_hybrid\_config) | Creates a hybrid controller with given configuration.<br>See 'main.tf' for valid keys. | <pre>object({<br>    google_app_cred_path    = string<br>    slurm_control_host      = string<br>    slurm_control_host_port = string<br>    slurm_control_addr      = string<br>    slurm_bin_dir           = string<br>    slurm_log_dir           = string<br>    output_dir              = string<br>    install_dir             = string<br>    munge_mount = object({<br>      server_ip     = string<br>      remote_mount  = string<br>      fs_type       = string<br>      mount_options = string<br>    })<br>  })</pre> | <pre>{<br>  "google_app_cred_path": null,<br>  "install_dir": null,<br>  "munge_mount": {<br>    "fs_type": "nfs",<br>    "mount_options": null,<br>    "remote_mount": "/etc/munge",<br>    "server_ip": null<br>  },<br>  "output_dir": "/etc/slurm",<br>  "slurm_bin_dir": "/usr/local/bin",<br>  "slurm_control_addr": null,<br>  "slurm_control_host": null,<br>  "slurm_control_host_port": null,<br>  "slurm_log_dir": "/var/log/slurm"<br>}</pre> | no |
| <a name="input_controller_instance_config"></a> [controller\_instance\_config](#input\_controller\_instance\_config) | Creates a controller instance with given configuration. | <pre>object({<br>    access_config = list(object({<br>      nat_ip       = string<br>      network_tier = string<br>    }))<br>    additional_disks = list(object({<br>      disk_name    = string<br>      device_name  = string<br>      disk_size_gb = number<br>      disk_type    = string<br>      disk_labels  = map(string)<br>      auto_delete  = bool<br>      boot         = bool<br>    }))<br>    can_ip_forward         = bool<br>    disable_smt            = bool<br>    disk_auto_delete       = bool<br>    disk_labels            = map(string)<br>    disk_size_gb           = number<br>    disk_type              = string<br>    enable_confidential_vm = bool<br>    enable_oslogin         = bool<br>    enable_shielded_vm     = bool<br>    gpu = object({<br>      count = number<br>      type  = string<br>    })<br>    instance_template   = string<br>    labels              = map(string)<br>    machine_type        = string<br>    metadata            = map(string)<br>    min_cpu_platform    = string<br>    network_ip          = string<br>    on_host_maintenance = string<br>    preemptible         = bool<br>    region              = string<br>    service_account = object({<br>      email  = string<br>      scopes = list(string)<br>    })<br>    shielded_instance_config = object({<br>      enable_integrity_monitoring = bool<br>      enable_secure_boot          = bool<br>      enable_vtpm                 = bool<br>    })<br>    source_image_family  = string<br>    source_image_project = string<br>    source_image         = string<br>    static_ip            = string<br>    subnetwork_project   = string<br>    subnetwork           = string<br>    tags                 = list(string)<br>    zone                 = string<br>  })</pre> | <pre>{<br>  "access_config": [],<br>  "additional_disks": [],<br>  "can_ip_forward": false,<br>  "disable_smt": false,<br>  "disk_auto_delete": true,<br>  "disk_labels": {},<br>  "disk_size_gb": null,<br>  "disk_type": null,<br>  "enable_confidential_vm": false,<br>  "enable_oslogin": true,<br>  "enable_shielded_vm": false,<br>  "gpu": null,<br>  "instance_template": null,<br>  "labels": {},<br>  "machine_type": "n1-standard-1",<br>  "metadata": {},<br>  "min_cpu_platform": null,<br>  "network_ip": null,<br>  "on_host_maintenance": null,<br>  "preemptible": false,<br>  "region": null,<br>  "service_account": null,<br>  "shielded_instance_config": null,<br>  "source_image": null,<br>  "source_image_family": null,<br>  "source_image_project": null,<br>  "static_ip": null,<br>  "subnetwork": "default",<br>  "subnetwork_project": null,<br>  "tags": [],<br>  "zone": null<br>}</pre> | no |
| <a name="input_controller_startup_scripts"></a> [controller\_startup\_scripts](#input\_controller\_startup\_scripts) | List of scripts to be ran on controller VM startup. | <pre>list(object({<br>    filename = string<br>    content  = string<br>  }))</pre> | `[]` | no |
| <a name="input_controller_startup_scripts_timeout"></a> [controller\_startup\_scripts\_timeout](#input\_controller\_startup\_scripts\_timeout) | The timeout (seconds) applied to each script in controller\_startup\_scripts. If<br>any script exceeds this timeout, then the instance setup process is considered<br>failed and handled accordingly.<br><br>NOTE: When set to 0, the timeout is considered infinite and thus disabled. | `number` | `300` | no |
| <a name="input_disable_default_mounts"></a> [disable\_default\_mounts](#input\_disable\_default\_mounts) | Disable default global network storage from the controller<br>* /usr/local/etc/slurm<br>* /etc/munge<br>* /home<br>* /apps<br>If these are disabled, the slurm etc and munge dirs must be added manually,<br>or some other mechanism must be used to synchronize the slurm conf files<br>and the munge key across the cluster. | `bool` | `false` | no |
| <a name="input_enable_bigquery_load"></a> [enable\_bigquery\_load](#input\_enable\_bigquery\_load) | Enables loading of cluster job usage into big query.<br><br>NOTE: Requires Google Bigquery API. | `bool` | `false` | no |
| <a name="input_enable_cleanup_compute"></a> [enable\_cleanup\_compute](#input\_enable\_cleanup\_compute) | Enables automatic cleanup of compute nodes and resource policies (e.g.<br>placement groups) managed by this module, when cluster is destroyed.<br><br>NOTE: Requires Python and script dependencies.<br><br>*WARNING*: Toggling this may impact the running workload. Deployed compute nodes<br>may be destroyed and their jobs will be requeued. | `bool` | `false` | no |
| <a name="input_enable_cleanup_subscriptions"></a> [enable\_cleanup\_subscriptions](#input\_enable\_cleanup\_subscriptions) | Enables automatic cleanup of pub/sub subscriptions managed by this module, when<br>cluster is destroyed.<br><br>NOTE: Requires Python and script dependencies.<br><br>*WARNING*: Toggling this may temporarily impact var.enable\_reconfigure behavior. | `bool` | `false` | no |
| <a name="input_enable_devel"></a> [enable\_devel](#input\_enable\_devel) | Enables development mode. Not for production use. | `bool` | `false` | no |
| <a name="input_enable_hybrid"></a> [enable\_hybrid](#input\_enable\_hybrid) | Enables use of hybrid controller mode. When true, controller\_hybrid\_config will<br>be used instead of controller\_instance\_config and will disable login instances. | `bool` | `false` | no |
| <a name="input_enable_reconfigure"></a> [enable\_reconfigure](#input\_enable\_reconfigure) | Enables automatic Slurm reconfigure on when Slurm configuration changes (e.g.<br>slurm.conf.tpl, partition details). Compute instances and resource policies<br>(e.g. placement groups) will be destroyed to align with new configuration.<br><br>NOTE: Requires Python and Google Pub/Sub API.<br><br>*WARNING*: Toggling this will impact the running workload. Deployed compute nodes<br>will be destroyed and their jobs will be requeued. | `bool` | `false` | no |
| <a name="input_epilog_scripts"></a> [epilog\_scripts](#input\_epilog\_scripts) | List of scripts to be used for Epilog. Programs for the slurmd to execute<br>on every node when a user's job completes.<br>See https://slurm.schedmd.com/slurm.conf.html#OPT_Epilog. | <pre>list(object({<br>    filename = string<br>    content  = string<br>  }))</pre> | `[]` | no |
| <a name="input_login_network_storage"></a> [login\_network\_storage](#input\_login\_network\_storage) | Storage to mounted on login and controller instances<br>* server\_ip     : Address of the storage server.<br>* remote\_mount  : The location in the remote instance filesystem to mount from.<br>* local\_mount   : The location on the instance filesystem to mount to.<br>* fs\_type       : Filesystem type (e.g. "nfs").<br>* mount\_options : Options to mount with. | <pre>list(object({<br>    server_ip     = string<br>    remote_mount  = string<br>    local_mount   = string<br>    fs_type       = string<br>    mount_options = string<br>  }))</pre> | `[]` | no |
| <a name="input_login_nodes"></a> [login\_nodes](#input\_login\_nodes) | List of slurm login instance definitions. | <pre>list(object({<br>    access_config = list(object({<br>      nat_ip       = string<br>      network_tier = string<br>    }))<br>    additional_disks = list(object({<br>      disk_name    = string<br>      device_name  = string<br>      disk_size_gb = number<br>      disk_type    = string<br>      disk_labels  = map(string)<br>      auto_delete  = bool<br>      boot         = bool<br>    }))<br>    can_ip_forward         = bool<br>    disable_smt            = bool<br>    disk_auto_delete       = bool<br>    disk_labels            = map(string)<br>    disk_size_gb           = number<br>    disk_type              = string<br>    enable_confidential_vm = bool<br>    enable_oslogin         = bool<br>    enable_shielded_vm     = bool<br>    gpu = object({<br>      count = number<br>      type  = string<br>    })<br>    group_name          = string<br>    instance_template   = string<br>    labels              = map(string)<br>    machine_type        = string<br>    metadata            = map(string)<br>    min_cpu_platform    = string<br>    network_ips         = list(string)<br>    num_instances       = number<br>    on_host_maintenance = string<br>    preemptible         = bool<br>    region              = string<br>    service_account = object({<br>      email  = string<br>      scopes = list(string)<br>    })<br>    shielded_instance_config = object({<br>      enable_integrity_monitoring = bool<br>      enable_secure_boot          = bool<br>      enable_vtpm                 = bool<br>    })<br>    source_image_family  = string<br>    source_image_project = string<br>    source_image         = string<br>    static_ips           = list(string)<br>    subnetwork_project   = string<br>    subnetwork           = string<br>    tags                 = list(string)<br>    zone                 = string<br>  }))</pre> | `[]` | no |
| <a name="input_login_startup_scripts"></a> [login\_startup\_scripts](#input\_login\_startup\_scripts) | List of scripts to be ran on login VM startup. | <pre>list(object({<br>    filename = string<br>    content  = string<br>  }))</pre> | `[]` | no |
| <a name="input_login_startup_scripts_timeout"></a> [login\_startup\_scripts\_timeout](#input\_login\_startup\_scripts\_timeout) | The timeout (seconds) applied to each script in login\_startup\_scripts. If<br>any script exceeds this timeout, then the instance setup process is considered<br>failed and handled accordingly.<br><br>NOTE: When set to 0, the timeout is considered infinite and thus disabled. | `number` | `300` | no |
| <a name="input_network_storage"></a> [network\_storage](#input\_network\_storage) | Storage to mounted on all instances.<br>* server\_ip     : Address of the storage server.<br>* remote\_mount  : The location in the remote instance filesystem to mount from.<br>* local\_mount   : The location on the instance filesystem to mount to.<br>* fs\_type       : Filesystem type (e.g. "nfs").<br>* mount\_options : Options to mount with. | <pre>list(object({<br>    server_ip     = string<br>    remote_mount  = string<br>    local_mount   = string<br>    fs_type       = string<br>    mount_options = string<br>  }))</pre> | `[]` | no |
| <a name="input_partitions"></a> [partitions](#input\_partitions) | Cluster partitions as a list. See module slurm\_partition. | <pre>list(object({<br>    enable_job_exclusive              = bool<br>    enable_placement_groups           = bool<br>    partition_conf                    = map(string)<br>    partition_startup_scripts_timeout = number<br>    partition_startup_scripts = list(object({<br>      filename = string<br>      content  = string<br>    }))<br>    partition_name = string<br>    partition_nodes = list(object({<br>      node_count_static      = number<br>      node_count_dynamic_max = number<br>      group_name             = string<br>      node_conf              = map(string)<br>      additional_disks = list(object({<br>        disk_name    = string<br>        device_name  = string<br>        disk_size_gb = number<br>        disk_type    = string<br>        disk_labels  = map(string)<br>        auto_delete  = bool<br>        boot         = bool<br>      }))<br>      access_config = list(object({<br>        network_tier = string<br>      }))<br>      bandwidth_tier         = string<br>      can_ip_forward         = bool<br>      disable_smt            = bool<br>      disk_auto_delete       = bool<br>      disk_labels            = map(string)<br>      disk_size_gb           = number<br>      disk_type              = string<br>      enable_confidential_vm = bool<br>      enable_oslogin         = bool<br>      enable_shielded_vm     = bool<br>      enable_spot_vm         = bool<br>      gpu = object({<br>        count = number<br>        type  = string<br>      })<br>      instance_template   = string<br>      labels              = map(string)<br>      machine_type        = string<br>      metadata            = map(string)<br>      min_cpu_platform    = string<br>      on_host_maintenance = string<br>      preemptible         = bool<br>      service_account = object({<br>        email  = string<br>        scopes = list(string)<br>      })<br>      shielded_instance_config = object({<br>        enable_integrity_monitoring = bool<br>        enable_secure_boot          = bool<br>        enable_vtpm                 = bool<br>      })<br>      spot_instance_config = object({<br>        termination_action = string<br>      })<br>      source_image_family  = string<br>      source_image_project = string<br>      source_image         = string<br>      tags                 = list(string)<br>    }))<br>    network_storage = list(object({<br>      local_mount   = string<br>      fs_type       = string<br>      server_ip     = string<br>      remote_mount  = string<br>      mount_options = string<br>    }))<br>    region             = string<br>    subnetwork_project = string<br>    subnetwork         = string<br>    zone_policy_allow  = list(string)<br>    zone_policy_deny   = list(string)<br>  }))</pre> | <pre>[<br>  {<br>    "enable_job_exclusive": false,<br>    "enable_placement_groups": false,<br>    "network_storage": [],<br>    "partition_conf": {<br>      "Default": "YES"<br>    },<br>    "partition_name": "debug",<br>    "partition_nodes": [<br>      {<br>        "access_config": [],<br>        "additional_disks": [],<br>        "bandwidth_tier": "platform_default",<br>        "can_ip_forward": false,<br>        "disable_smt": false,<br>        "disk_auto_delete": true,<br>        "disk_labels": {},<br>        "disk_size_gb": null,<br>        "disk_type": null,<br>        "enable_confidential_vm": false,<br>        "enable_oslogin": true,<br>        "enable_shielded_vm": false,<br>        "enable_spot_vm": false,<br>        "gpu": null,<br>        "group_name": "test",<br>        "instance_template": null,<br>        "labels": {},<br>        "machine_type": "n1-standard-1",<br>        "metadata": {},<br>        "min_cpu_platform": null,<br>        "node_conf": {},<br>        "node_count_dynamic_max": 0,<br>        "node_count_static": 20,<br>        "on_host_maintenance": null,<br>        "preemptible": false,<br>        "service_account": null,<br>        "shielded_instance_config": null,<br>        "source_image": null,<br>        "source_image_family": null,<br>        "source_image_project": null,<br>        "spot_instance_config": null,<br>        "tags": []<br>      }<br>    ],<br>    "partition_startup_scripts": [],<br>    "partition_startup_scripts_timeout": 300,<br>    "region": null,<br>    "subnetwork": "default",<br>    "subnetwork_project": null,<br>    "zone_policy_allow": [],<br>    "zone_policy_deny": []<br>  }<br>]</pre> | no |
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | Project ID to create resources in. | `string` | n/a | yes |
| <a name="input_prolog_scripts"></a> [prolog\_scripts](#input\_prolog\_scripts) | List of scripts to be used for Prolog. Programs for the slurmd to execute<br>whenever it is asked to run a job step from a new job allocation.<br>See https://slurm.schedmd.com/slurm.conf.html#OPT_Prolog. | <pre>list(object({<br>    filename = string<br>    content  = string<br>  }))</pre> | `[]` | no |
| <a name="input_slurm_cluster_name"></a> [slurm\_cluster\_name](#input\_slurm\_cluster\_name) | Cluster name, used for resource naming and slurm accounting. | `string` | n/a | yes |
| <a name="input_slurm_conf_tpl"></a> [slurm\_conf\_tpl](#input\_slurm\_conf\_tpl) | Slurm slurm.conf template file path. | `string` | `null` | no |
| <a name="input_slurmdbd_conf_tpl"></a> [slurmdbd\_conf\_tpl](#input\_slurmdbd\_conf\_tpl) | Slurm slurmdbd.conf template file path. | `string` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_slurm_cluster_name"></a> [slurm\_cluster\_name](#output\_slurm\_cluster\_name) | Slurm cluster name. |
| <a name="output_slurm_controller_instance_details"></a> [slurm\_controller\_instance\_details](#output\_slurm\_controller\_instance\_details) | Slurm controller instance details. |
| <a name="output_slurm_controller_instance_self_links"></a> [slurm\_controller\_instance\_self\_links](#output\_slurm\_controller\_instance\_self\_links) | Slurm controller instance self\_link. |
| <a name="output_slurm_controller_instances"></a> [slurm\_controller\_instances](#output\_slurm\_controller\_instances) | Slurm controller instance object details. |
| <a name="output_slurm_login_instance_details"></a> [slurm\_login\_instance\_details](#output\_slurm\_login\_instance\_details) | Slurm login instance details. |
| <a name="output_slurm_login_instance_self_links"></a> [slurm\_login\_instance\_self\_links](#output\_slurm\_login\_instance\_self\_links) | Slurm login instance self\_link. |
| <a name="output_slurm_partition"></a> [slurm\_partition](#output\_slurm\_partition) | Slurm partition details. |
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->