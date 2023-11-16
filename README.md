# Branches

  * [branch-00](#model-familiarity-and-initial-analysis-branch-00)
  * [branch-01](#protocol-and-controller-analysis-branch-01)

# Model Familiarity and Initial Analysis (branch-00)

## In this branch, you will:

  * Confirm access to Grafana dashboard and Node-RED HMI
  * Confirm access to Wireshark container desktop
  * Confirm access to configuration files in VS Code

## Steps

  1. Confirm that all the containers are up and running in the Gitpod workspace (there should be _10_ containers running)
  1. Browse to the public ports exposed by Gitpod for Grafana and Node-RED HMI
      * Copy the links provided on the Ports tab for Grafana and HMI (or click on `world` icon to right of links) to open two separate tabs in the browser
          * For the Node-RED HMI, use the primary URL and add `/ui` to the path, for example `https://<address>/ui`
      * For Grafana turbine dashboard &mdash; hamburger menu (upper left) > Dashboards > General > Turbine
  1. Browse to the public port exposed by Gitpod for the `wireshark` container (add `/vnc.html` to the primary URL similar to the Node-RED HMI)
      * Copy the provided on the Ports tab for Wireshark (or click on `world` icon to right of links) to open a separate in the browser
      * Click the `Connect` button in the NoVNC dialogue
      * Wireshark should be running already &mdash; if not, right-click on desktop > Wireshark to start Wireshark
      * Begin to capture traffic on interface that starts with `br-`
  1. Locate the relevant configuration files located in the `configs/ot-sim` directory

## After completing the above steps, you should work to:

  * Describe the overall architecture of the system
      * Are you able to describe what the containers are functioning as in context of a larger wind turbine system?
  * Identify main communication traffic patterns (focus on Modbus)
      * Who is talking the loudest and the most?
      * Who are they talking to?
      * What else stands out to you in the communications between containers?
  * Identify controller responsibilities
      * Are you able to determine what the controller is responsible for in the overall context of the system?

## The following are available to you to assist in the above data gathering:

  * Traffic analysis tools (the Wireshark container)
  * Initial configuration files

> NOTE: there will be a Q&A session at the end of this module. During this time,
> you should stop the current Gitpod workspace and deploy the next branch in
> Gitpod using this URL:
> https://gitpod.io/#https://github.com/patsec/uiuc-lab/tree/branch-01

# Protocol and Controller Analysis (branch-01)

## Answers for branch-00

  * There are six containers running controllers that are relevant to the turbine operations.
      * The `main-controller` is the loudest, doing the most talking
      * The `blade` containers, three of them, are the most quiet, talking the least
  * The `main-controller` is responsible for monitoring and then controlling the turbine system based on weather data (primarily).
      * For example, the blades will be feathered based on wind speed: it is too slow or too fast, the blades need to feather so the turbine will not operate in poor conditions.
  * The controller communication, using on the Modbus protocol, is based on values assigned to known registers; changes to Modbus register values results in a change to a controller in the system.
      * Filter on `modbus` in Wireshark to limit packets to just Modbus communications
  * The Grafana turbine dashboard and the Node-RED HMI provide a visual reporting on the state of key weather data as well as the operation of the turbine.
  * The configuration files are for the following controllers:
      * `alpine-eclipse.yml`: this is the yaw controller
          * This controller monitors the current yaw setting as well as the set point for the yaw.
      * `crazy-trader.yml`, `odd-prodigy.yml`, and `wavy-bird.yml`: these are the blade controllers
          * These controllers change the pitch for each blade (a boolean value &mdash; feathered or not feathered).
      * `exalted-bear.yml`: this is the anemometer controller
          * This controller ingests the data from the `weather.csv` file and then publishes it as live weather updates.
      * `low-patriot.yml`: this is the main controller
          * Based on data published from the other controllers, this controller publishes adjustments to the blades (whether or not to feather) and to the yaw controller (what direction to face).
          * As the wind speed, temperature, and pressure change &mdash; along with the feather value for each blade &mdash; the appropriate power generation is calculated (and simulated).

> NOTE: in addition to Modbus traffic, each controller also generates HTTP traffic to the OpenSearch container. This provides the ground truth data in the Grafana turbine dashboard, which is separate from the Modbus data used by the Node-RED UI.

## In this branch, you will work to describe:

  * The Modbus protocol usage
      * A helpful [Modbus](https://www.modbus.org/docs/Modbus_Application_Protocol_V1_1b3.pdf) protocol read ahead
  * The wind turbine controller logic
      * DOE has a simply overview on the basic operations of a [wind turbine](https://www.energy.gov/eere/wind/how-wind-turbine-works-text-version)

> NOTE: this branch uses accurate names for the controller containers.

## Steps

  1. If you have not already done so, deploy a new Gitpod workspace for this branch using this URL: https://gitpod.io/#https://github.com/patsec/uiuc-lab/tree/branch-01
  1. Browse to the public port exposed by Gitpod for the `wireshark` container (add `/vnc.html` to the primary URL similar to the Node-RED HMI)
      * Copy the provided on the Ports tab for Wireshark (or click on `world` icon to right of links) to open a separate in the browser
      * Click the `Connect` button in the NoVNC dialogue
      * Wireshark should be running already &mdash; if not, right-click on desktop > Wireshark to start Wireshark
      * Begin to capture traffic on interface that starts with `br-`
  1. Review the logic in the `configs/ot-sim/main-ctlr.xml` and `configs/ot-sim/yaw-ctlr.xml` configuration files.

## After completing the above steps, you should work to:

  * Identify the Modbus register types and addresses in use
      * What do the Modbus packets tell you about the Modbus traffic?
  * Identify values being sent for Modbus registers
  * Describe what the logic in the main and yaw controllers is doing

> NOTE: there will be a Q&A session at the end of this module. During this time,
> you should stop the current Gitpod workspace and deploy the next branch in
> Gitpod using this URL:
> https://gitpod.io/#https://github.com/patsec/uiuc-lab/tree/branch-02
