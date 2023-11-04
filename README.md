# OVH Deploy Hosting Action

Patched by WouterC

A GitHub Action to deploy the contents of a repository to an OVH Hosting Plan (with SSH support)

>This GitHub Action works best with the OVH Pro Hosting plan. It might work with other providers if their hosting product meet the reguirements listed below.

## Requirements

* OVH Account with an active [Pro Hosting](https://www.ovh.de/hosting/hosting-pro.xml) service (which has SSH-support)

  * A new user with SSH access to your hosting account (Web -> Hosting plans -> `<yourPlan>` -> FTP - SSH -> Create a FTP user)

  * Grant the user SSH access after you created the account

  * Ensure you configured the users home directory as the websites root directory

* A repository with your websites code

* Needed environment variables are configured and the SSH users name + password are available as GitHub secrets (`<yourRepository>` -> Settings -> Secrets -> New secret)

## Example usage

Inside your code base/repository create the directories `.github/workflows`. In the `workflows` folder create a file `main.yml` with the following content:

```yaml
on:
  push:
    branches:
      - name-of-branch

jobs:
  deploy:
    runs-on: ubuntu-latest
    name: Deploy website
    steps:
    - name: Deploy to OVH hosting
      uses: woutercall/ovh-deploy
      env:
        OVH_HOSTING_USER: ${{ secrets.OVH_HOSTING_USER }}
        OVH_HOSTING_PASSWORD: ${{ secrets.OVH_HOSTING_PASSWORD }}
        OVH_HOSTING_DOMAIN: ssh.cluster20.hosting.ovh.net
        REPOSITORY_DIR: directory on OVH
        REPOSITORY_BRANCH: branch        
```

Now simply adjust the last three listed environment variables. Visit your OVH hosting plans control panel to get the correct SSH domain.
Don't forget to create two secrets at your repository with the names `OVH_HOSTING_USER` + `OVH_HOSTING_PASSWORD` and the appropriate credentials as the value. Use a strong password!

With this GitHub Actions configuration everytime you push new changes to your repository, the ovh-deploy-hosting-action will login to your hosting account and get the newest version of your repo/code. Additionally delete the `README.md`.
