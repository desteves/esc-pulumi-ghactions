# esc-pulumi-ghactions

A Hello World webapp continuously deployed in AWS using Pulumi IaC + Pulumi ESC in a GitHub Actions Workflow.

## step by step

```bash

# create repo with lic + readme, and clone it locally
$ gh repo clone desteves/esc-pulumi-ghactions
$ cd esc-pulumi-ghactions
$ tree              
.
# ├── LICENSE
# └── README.md

# add structure
$ mkdir app
$ mkdir infra


$ pulumi new container-aws-typescript  --dir infra --name esc-pulumi-ghactions  --yes
# wait for deps to download...

# re-org to my preferred dir structure
$ mv infra/app/Dockerfile app 
$ vi infra/index.ts 
#   update the app source directory, should now read `../app`
$ vi app/Dockerfile
#   use alpine


$ rmdir infra/app
$ tree -L 2      
# .
# ├── LICENSE
# ├── README.md
# ├── app
# │   └── Dockerfile
# └── infra
#     ├── Pulumi.dev.yaml
#     ├── Pulumi.yaml
#     ├── index.ts
#     ├── node_modules
#     ├── package-lock.json
#     ├── package.json
#     └── tsconfig.json

# 4 directories, 9 files


$ echo "environment:" >> infra/Pulumi.dev.yaml
$ echo "    - esc-pulumi-ghactions-env" >> infra/Pulumi.dev.yaml


# <Not shown> add a secret to the gh repo 
# <Not shown> create ESC env


# Add pipeline GH Action
# See .github/workflows/pipeline.yml 


# update modules to latest
$ cd infra
$ npm update --save
$ cd -


# Create PR + commit
$ echo "**node_modules" >> .gitignore
$ git add .
$ git commit -m "pipeline test"
$ gh pr create

# ? Where should we push the 'add-pipeline' branch? desteves/esc-pulumi-ghactions

# Creating pull request for add-pipeline into main in desteves/esc-pulumi-ghactions

# ? Title pipeline test
# ? Body <Received>
# ? What's next? Submit as draft
# remote: 
# remote: 
# To https://github.com/desteves/esc-pulumi-ghactions.git
#  * [new branch]      HEAD -> add-pipeline
# branch 'add-pipeline' set up to track 'origin/add-pipeline'.
# https://github.com/desteves/esc-pulumi-ghactions/pull/1



# $ pulumi up --dir infra --yes

```