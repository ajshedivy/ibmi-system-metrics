# ibmi-system-metrics
Grafana and Prometheus deployments for analyzing ibm i system metrics

## Container Architecture Overview

This project uses Docker Compose to launch two containers: Grafana and Prometheus. Grafana is a popular open-source dashboarding and visualization tool, while Prometheus is a monitoring and alerting toolkit for time series data.

In this architecture, Prometheus collects metrics from two ibm i systems running [prometheus-jdbc-exporter](https://github.com/ThePrez/prometheus-exporter-jdbc) and stores them in its time series database. Grafana then retrieves these metrics from Prometheus and displays them in customizable dashboards.

## Setup and Usage

To get started, follow these steps:

1. Install Docker on your system if you haven't already done so. You can find installation instructions for your platform on the Docker website.

2. Clone this repository to your local machine:

```
git clone https://github.com/ajshedivy/ibmi-system-metrics.git
```
3. Navigate to the repository directory:

```
cd ibmi-system-metrics/
```

4. Start the containers using Docker Compose:

```
docker compose up -d
```

This command will start the containers in the background and leave them running. By default, Prometheus is configured to scrape metrics from the `localhost` endpoint. Nativate to `http://localhost:9090/targets` to see the list of ibm i exporters running:

![image](https://user-images.githubusercontent.com/50843283/227840352-4d903228-f34d-4931-8f93-f84b9f66bc2f.png)

You can now access Grafana by opening a web browser and navigating to `http://localhost:3000`. Login using the Grafana credentials
- user: admin
- password: grafana

![image](https://user-images.githubusercontent.com/50843283/227840931-5053cb8e-74dd-4055-a883-85d2a71076ca.png)




 ## Install Prometheus Plugin
 
To install a plugin, just find the "Settings" button (a gear icon) on the left-hand pane and go to "Plugins"

![image](https://user-images.githubusercontent.com/50843283/227841054-c4458679-c8d5-4d54-b532-66919caabc92.png)

Search for "Prometheus" and install the Prometheus plugin from Grafana Labs
![image](https://user-images.githubusercontent.com/50843283/227841228-04cf87bf-1a7d-4211-8874-11b081c61976.png)

### Add a data source
Now, you will add your running prometheus deployment as a data source. To do so, go to Grafana configurations and find the "Data sources" option. Click the button to add a data source.

![image](https://user-images.githubusercontent.com/50843283/227841406-8a623b27-b06d-4f4f-b9b4-3b52bb1e142e.png)

add the url: `http://host.docker.internal:9090`

`host.docker.internal` is a special hostname that can be used to connect from within a Docker container to the host machine's network.

Click "Save and Test" to verify that Grafana can talk to your backend!

## Import Grafana dashboard

Click the Dashboards button on the left-hand navigation pane (looks like four small squares) and click the "import" button.

![image](https://user-images.githubusercontent.com/50843283/227841894-a6da8267-87c5-4460-9f77-ad49b6fd79ad.png)

Upload the json configuration file `src\grafana\multi-host-dashboard.json` and click "load".

![image](https://user-images.githubusercontent.com/50843283/227842155-c10b0d6f-65db-4af1-ae2f-3b330cfb7fee.png)

Now you're visualizing live metric data from two ibm i systems `ut33p23` and `ut51p41`

![image](https://user-images.githubusercontent.com/50843283/227842360-5de315f0-fde1-4733-ba64-b16b74ebb92d.png)

# Data exploration

Feel free to experiment with the dashboard configuration and differenet metrics. I have linked an example python scraper for analyzing the prometheus data in a local python environment. We can utilize the prometheus deplyment endpoint `http://localhost:9090/api/v1/query` to query the various metrics. There is also a [prometheus client api](https://pypi.org/project/prometheus-api-client/) that we can experiment with which you can install using pip. 














