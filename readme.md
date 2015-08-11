# prometheusTest

A quick demonstrator of using [Prometheus](http://prometheus.io) to monitor Docker containers. It uses [cAdvisor](https://github.com/google/cadvisor) to gather and expose the metrics and uses [Redis](https://hub.docker.com/_/redis/) as two stunt containers.

The Prometheus VM also contains [AlertManager](http://prometheus.io/docs/alerting/alertmanager/) and [PromDash](https://github.com/prometheus/promdash/blob/master/README.md)

## Getting Started

Install Vagrant (tested with 1.7.2) and run the Vagrantfile:

    > vagrant up

The Vagrantfile will run up three machines. These consist of: 
- 192.168.33.10 - Redis and cAdvisor installed
- 192.168.33.11 - Redis and cAdvisor installed
- 192.168.33.12 - Prometheus and cAdvisor instatlled.

Once the start up has completed you can open [http://localhost:9090](http://localhost:9090) which will bring up the Prometheus UI. Here you can see the status of the items that are being monitored and the current machine configuration.

## Viewing Data

### Using Prometheus

From the Prometheus UI, select **Graph**. In the top edit box add **container\_cpu_usage\_seconds\_total {name=~"redis\*"}** and press **Execute**. This will run a search for the CPU time used by items that match **redis**. You should end up with two entries showing the time used for the two Redis nodes we have. 

If you select the **Graph** tab, you can see the data plotted.

### Using PromDash

Navigate to [http://localhost:3000](http://localhost:3000) and you should be shown a blank Promdash page. After starting up the VMs, Promdash will be completely clean so we will need to add our Prometheus server and then create a graph.

Click on the **Servers** tab and click **New Server**. Add a name of **LocalTest**, a **Url** of **http://localhost:9090**, set the **Server type** to **Prometheus** and click **Create Server**. 

Click on the **Dashboards** tab at the top where you will be presented with an empty list. Click **New Dashboard** and set **Name** to **Test** and click **Create Dashboard**. You will then be presented with a blank dashboard. 

To add a dashboard, hover your mouse to the top-right of the cavas (opposite to **Title**) and click the second icon from the left (tagged as **Datasources**). Click **Add Expression** and enter in **container\_cpu_usage\_seconds\_total {name=~"redis\*"}**. Ensure that **LocalTest** is selected in the left-most dropdown and click the **X** at the top-right. On closing the dialog you should be presented with the details of the two Redis containers. 

