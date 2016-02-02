# Brainiac

[Ansible](http://www.ansible.com) configuration for [GPU optimized](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cluster_computing.html) Deep Learning with [TensorFlow](https://www.tensorflow.org) on [EC2](http://aws.amazon.com/ec2).

## Getting Started

 > ### Important
 > Before you can properly provision an instance, you need to [download the cuDNN library from Nvidia's site](https://developer.nvidia.com/rdp/assets/cudnn-65-linux-v2-asset). You will need to become part of their developer program which requires an application and can take 1-2 days to process. So if you want to get started, do that first.

### Launch an instance

```sh
 > ansible-playbook -i inventory plays/launch.yml
```

### Provision an instance

*Note:* A note about cuDNN. As stated above, you will need to download the file `cudnn-6.5-linux-x64-v2.tgz` from [Nvidia's site](https://developer.nvidia.com/rdp/assets/cudnn-65-linux-v2-asset). Place it in the `files` directory before proceeding.

```sh
 > ansible-playbook -i inventory plays/provision.yml
```

You should now have TensorFlow installed with GPU support.

### Terminate an instance

Termination is still manual. Log into your [AWS console](http://console.aws.amazon.com), terminate the instance, then remove it from your `inventory` file.
