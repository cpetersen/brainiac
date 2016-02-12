# Brainiac - UNDER DEVELOPMENT, NOT QUITE WORKING YET

[Ansible](http://www.ansible.com) configuration for [GPU optimized](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cluster_computing.html) Deep Learning with [TensorFlow](https://www.tensorflow.org) on [EC2](http://aws.amazon.com/ec2).

## Getting Started

 > ### Important
 > Before you can properly provision an instance, you need to [download the cuDNN library from Nvidia's site](https://developer.nvidia.com/rdp/assets/cudnn-65-linux-v2-asset). You will need to become part of their developer program which requires an application and can take 1-2 days to process. So if you want to get started, do that first.

### Launch an instance

```sh
 > ansible-playbook -i inventory plays/launch.yml
```

### Provision an instance

*Note:* A note about cuDNN. As stated above, you will need to download the file `cudnn-6.5-linux-x64-v2.tgz` from [Nvidia's site](https://developer.nvidia.com/rdp/assets/cudnn-65-linux-v2-asset). Place it in the `plays/roles/cuda/files` directory before proceeding.

This will take a while, approximately 40 minutes on my setup.

```sh
> time ansible-playbook -i inventory plays/provision.yml
 PLAY [Brainiac] ***************************************************************

 GATHERING FACTS ***************************************************************
 ok: [54.151.16.56]

 TASK: [cuda | Update and Upgrade] *********************************************
 changed: [54.151.16.56]

 TASK: [cuda | Install development dependencies] *******************************
 changed: [54.151.16.56] => (item=build-essential,g++,gfortran,git,libatlas-base-dev,libblas-dev,libfreetype6-dev,liblapack-dev,libncurses-dev,libopenblas-dev,libxft-dev,linux-headers-generic,linux-image-extra-virtual,pkg-config,python-dev,python-matplotlib,python-numpy,python-pandas,python-pip,python-pydot,python-sklearn,swig,unzip,unzip,wget,zip,zlib1g-dev)

...

 TASK: [tensorflow | Configure Tensorflow] *************************************
 changed: [54.151.16.56]

 TASK: [tensorflow | command /mnt/brainiac/bin/bazel build -c opt --config=cuda //tensorflow/cc:tutorials_example_trainer] ***
 changed: [54.151.16.56]

 TASK: [tensorflow | command /mnt/brainiac/bin/bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package] ***
 changed: [54.151.16.56]

 PLAY RECAP ********************************************************************
 54.151.16.56               : ok=28   changed=24   unreachable=0    failed=0

 ansible-playbook -i inventory plays/provision.yml  1.75s user 1.38s system 0% cpu 38:36.03 total
```

You should now have TensorFlow installed with GPU support.

### Terminate an instance

```sh
 > ansible-playbook -i inventory plays/terminate.yml
```

# Acknowledgements

Thank you to John Ramey and his post [Installing TensorFlow on an AWS EC2 Instance with GPU Support](http://ramhiser.com/2016/01/05/installing-tensorflow-on-an-aws-ec2-instance-with-gpu-support/). This project largely coverts that post into Ansible plays.
