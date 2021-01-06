Molecule tests for the role.

# Test Matrix

Test options in "Other" column flow down through table unless changed.

Test   | # Partitions | Groups in partitions?   | Other
---    | ---          | ---                     | ---
test1  | 1            | N                       | 2x compute node, sequential names (default test), config on all nodes
test1b | 1            | N                       | 1x compute node
test1c | 1            | N                       | 2x compute nodes, nonsequential names
test2  | 2            | N                       | 4x compute node, sequential names
test3  | 1            | Y                       | -
test4  | 1            | N                       | 2x compute node, accounting enabled
test5  | 1            | N                       | As for #1 but configless
test6  | 1            | N                       | 0x compute nodes, configless
test7  | 1            | N                       | 1x compute node, no login node, configless

# Local Installation & Running

Local installation on a Centos7 machine looks like:

    sudo yum install -y gcc python3-pip python3-devel openssl-devel python3-libselinux
    sudo yum install -y yum-utils
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum install -y docker-ce docker-ce-cli containerd.io
    pip3 install -r molecule/requirements.txt --user

    sudo systemctl start docker
    sudo usermod -aG docker ${USER}
    newgrp docker
    docker run hello-world # test docker works without sudo

    sudo yum install -y git
    git clone git@github.com:stackhpc/ansible-role-openhpc.git
    cd ansible-role-openhpc/

Then to run all tests:

    cd ansible-role-openhpc/
    MOLECULE_IMAGE=centos:7 molecule test --all
    MOLECULE_IMAGE=centos:8.2.2004 molecule test --all

Note that to see some debugging information you may want to prepend:

    MOLECULE_NO_LOG="false" ...
