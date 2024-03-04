# MFA access to CSCS (ela, daint) with modifications

```sh
git clone https://github.com/pesieber/sshservice-cli.git CSCS_sshservice-cli
git clone git@github.com:pesieber/sshservice-cli.git CSCS_sshservice-cli
```

Modifications: the original generates and authenticates the ssh key, my modifications start the ssh-agent and add the key

Add in .ssh/config:
```sh
Host ela
  Hostname ela.cscs.ch
  IdentityFile ~/.ssh/cscs-key
  User psieber

Host daint
  Hostname daint.cscs.ch
  IdentityFile ~/.ssh/cscs-key
  User psieber
  ProxyJump ela
  ForwardAgent yes
```

For each new ssh key (daily), execute the shell script to generate, authorize, start ssh-agent, add key to ssh-agent (user = cscs_user_name, cscs pw, GoogleAuthenticator OTP)
```sh
bash ~/CSCS_sshservice-cli/cscs-keygen.sh                               
```

For convencience, put this in your .bashrc and use "todaint" as the command instead:
```sh
alias todaint='bash ~/CSCS_sshservice-cli/cscs-keygen.sh'
```

After generating and adding the key, you can login directly:
```sh
ssh daint
```


# mfa-cscs-access (original)

The repository contains a simple Python script [cscs-keygen.py] and a shell script [cscs-keygen.sh] which can be used as a command line tool for fetching public and private keys signed by CSCS'S CA after authenticating using MFA. You can then use those keys to ssh to CSCS'login nodes.

For using the python script, these are the steps:

```sh
git clone git@github.com:eth-cscs/sshservice-cli.git
cd sshservice-cli
pip install virtualenv # (if you don't already have virtualenv installed)
virtualenv venv # to create your new environment (called 'venv' here)
source venv/bin/activate # to enter the virtual environment
pip install -r requirements.txt # to install the requirements in the current environment
python cscs-keygen.py
```

For using the shell script, these are the steps:
```bash
git clone git@github.com:eth-cscs/sshservice-cli.git
cd sshservice-cli
bash cscs-keygen.sh
```
