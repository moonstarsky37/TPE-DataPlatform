# Taipei City Data Dashboard Platform

This repository hosts the codebase for the Taipei City Data Dashboard Platform—a showcase project designed to demonstrate and implement applications related to pregnancy issues. The platform serves as a static interface to display relevant data applications, with the data processing flow and backend components residing in separate projects.

## Project Details

The UI and architecture of this project are built upon the foundation provided by [Taipei City Dashboard Frontend](https://github.com/tpe-doit/Taipei-City-Dashboard-FE). Additional features and content specific to pregnancy issues have been integrated.

### Preview

## Getting Started

1. Clone this repository.
2. Use the provided activation method suitable for your environment.
3. Add `.env` file as following content

```
BASE_URL=../..
VITE_APP_TITLE=臺北城市儀表板開源版 # 本應用程式的標題
VITE_MAPBOXTOKEN=pk.....
VITE_MAPBOXTILE=mapbox://... # 在這裡輸入 Mapbox Tileset 連結（台北市大樓 3D 模型）
```

4. Activate and access the platform at [http://localhost:80](http://localhost:80) after successful activation.

### Activation Methods

#### Docker

1. Install [Docker](https://www.docker.com/products/docker-desktop/) on your computer and start running it.
2. Fork this repository then clone the project to your computer. Execute `pwd` (mac) or `cd` in the repository terminal to get the complete path.
3. Execute the following command in the system terminal and replace "<repository path>" with the path you got in step 2.

```bash
docker run -v <repository path>:/opt/Taipei-City-Dashboard-FE -p 80:80 -it node:18.18.1-alpine3.18  sh
```

4. Execute the following commands to enter the project folder and install packages.

```bash
cd /opt/Taipei-City-Dashboard-FE
npm install
```

5. You should now be able to locally host this project by executing `npm run dev` in the respository terminal.
6. Refer to the [Docs](https://tuic.gov.taipei/documentation/front-end/project-setup) to complete further configurations.

#### Local Environment

1. Download [Node.js](https://nodejs.org/en) on your computer.
2. Fork this repository then clone the project to your computer.
3. Execute `npm install` in the respository terminal
4. You should now be able to locally host this project by executing `npm run dev` in the respository terminal.
5. Refer to the [Docs](https://tuic.gov.taipei/documentation/front-end/project-setup) to complete further configurations.

#### Docker Compose

The platform supports various activation methods, including native activation and compatibility with Docker Compose. Follow the instructions below for activation:

```yaml
version: "3.3"
services:
    node:
        volumes:
            - "path/to/this/repository:/opt/Taipei-City-Dashboard-FE"
        ports:
            - "80:80"
        stdin_open: true
        tty: true
        image: "node:18.18.1-alpine3.18"
        command: sh
```

## Documentation

Check out the complete documentation for Taipei City Dashboard FE [here](https://tuic.gov.taipei/documentation).
