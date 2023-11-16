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
