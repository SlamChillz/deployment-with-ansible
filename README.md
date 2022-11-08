# Deploy Laravel and Set up Postgresql on a Virtual Machine in the Cloud

You are required to deploy this [Laravel application](https://github.com/f1amy/laravel-realworld-example-app) from your mini project.
This time, the entire deployment steps including installation of packages and dependencies,
configuring your apache webserver etc, will be defined in an ansible playbook and deployed to atleast one ansible slave.

## Solution Requirements

- You should also write a bash script that would install and set up postgresql
- This bash script would be run on your ansible slaves using an ansible playbook.
- Should be able to access your deployment using a domain name of your choice(not an IP address).
- Should be able to test all the endpoints without errors
- Your base url may or may not display the default Laravel page
- These must be done on virtual machines on any cloud provider of your choice(any Linux distro of your choice).
- Your application must be encrypted with TLS/SSL.
- You may or may not define a logical network on the cloud, but extra efforts would be rewarded

## Prerequistes

- An ansible control node, where ansible is install
- At least one remote server with `Debian 11` linux operating system
- Clone this repository `git clone https://github.com/Mendyslam/altschool-second-semester-project-cloud-engineering.git`

## Make these modifications

- variables: rename [main.yaml.sample](vars/main.yaml.sample) to `main.yaml` and provide your values to the variables.
    - using the `mv` command do: `mv main.yaml.sample main.yaml`
    - Replace the `{ ... }` with the actual values. If your remote server user is `altschool`, then line:
      `user: { your host username }` will be `user: altschool`
- postgresql script: rename [postgres.sh.sample] to `postgres.sh` and provide the appropriate values as outlined above.

## 1. The Server Initial Setup

[initial-setup.yaml](./initial-setup.yaml) playbook does the following:

- Installs aptitude package manager
- Updates existing packages
- Installs essential server packages
- Sets up ssh configuration

#### To run the initial-setup playbook
- `ansible-playbook initial-setup.yaml`

## 2. Set up Packages and Softwares for the Application and Database Server

[setup.yaml](./setup.yaml) playbook requires several roles that does the following:

- Installs and configures `ufw`
- Installs and configures `git`
- Installs and configures `php` and its dependencies
- Installs and configures `apache2` and its dependencies
- Installs and configures `SSL/TLS` for the `apache2` web server
- Installs and configures `postgresql` server

#### To run the entire setup playbook
- `ansible-playbook setup.yaml`

#### To run a specific(s) configuration, e.g. php and psql
- `ansible-playbook setup.yaml --tags=php,psql`

## 3. The Deployment

[deploy.yaml](./deploy.yaml) playbook executes the tasks listed below:

- Clones the application repository
- Sets appropriate ownership and permissions to the project directory
- Ensures the application storage cache is writable
- Installs composer and runs `cmposer install` to install the application dependencies
- Sets up environmental variables and generates application key
- Optimizes the application build
- Runs migrations and seeds the database
- Restarts apache2 webserver to sync deployment

#### To run the deployment playbook
- `ansible-playbook deploy.yaml`

## Application URL
- domain: [https://altcloud.me](https://altcloud.me)
- To view the `tags` and `articles`
    - [https://altcloud.me/api/tags](https://altcloud.me/api/tags)
    - [https://altcloud.me/api/articles](https://altcloud.me/api/articles)
