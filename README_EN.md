# Kagawa(Unofficial) COVID-19 Task Force website

![](https://github.com/kagawa-metropolitan-gov/covid19/workflows/production%20deploy/badge.svg)

[![Kagawa COVID-19 Task Force website](https://user-images.githubusercontent.com/19589940/76959982-f2868a80-695d-11ea-9d8e-28e9e9225d2c.png)](https://kagawa-covid19.netlify.com/)

### [日本語](./README.md) | [English](./README_EN.md)

## How to Contribute

All contributions are welcome!
Please check [How to contribute](./.github/CONTRIBUTING_EN.md) for details.

## Code of Conduct

Please check [Code of conduct for developers](./.github/CODE_OF_CONDUCT_EN.md) for details.

## License
This software is released under [the MIT License](./LICENSE.txt).

## For Developers

### How to Set Up Environments

- Required Node.js version: 10.19.0 or higher

**Use yarn**
```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev
```

**Use docker**
```bash
# serve with hot reload at localhost:3000
$ docker-compose up --build
```

### How to resolve `Cannot find module ****` error

**Use yarn**
```bash
$ yarn install
```

**Use docker**
```bash
$ docker-compose run --rm app yarn install
```

### Detect production/others environment

On the production environment, `'production'` is assigned to `process.env.GENERATE_ENV` variable, on the other case `'development'` is assigned to the variable.  
Please use the variable to detect which enviroinment is used at the runtime.

### Deployment to Staging & Production Environments

When `master` branch is updated, the HTML files will be automatically built onto `production` branch,
and then the production site (https://kagawa-covid19.netlify.com/) will be also updated.

When `development` branch is updated, the HTML files will be automatically built onto `dev-pages` branch,
and then the development site (https://dev-covid19-kagawa.netlify.com/) will be also updated.

### Branch rules

Pull Request is allowed only for `development`.  
Please use the following naming rules for the branch when sending a Pull Request.

Feature implementation: feature/#{ISSUE_ID}-#{branch_title_name}  
Hotfix commit: hotfix/#{ISSUE_ID}-{branch_title_name}

#### Basic branch
| Purpose | Branch | Confirmation URL | Remarks |
| ---- | -------- | ---- | ---- |
| Development | development | http://dev-covid19-kagawa.netlify.com/ | base branch. Basically send a Pull Request here |
| Production | master | http://stopcovid19.metro.kagawa.lg.jp/ | Pull Requests other than Administrators are prohibited |

#### Branch used by the system
| Purpose | Branch | Confirmation URL | Remarks |
| ---- | -------- | ---- | ---- |
| Development site HTML | dev-pages | http://stopcovid19.metro.kagawa.lg.jp/ | Location where statically built HTML is located |
| Production site HTML | production | http://stopcovid19.metro.kagawa.lg.jp/ | Location where statically built HTML is located |
