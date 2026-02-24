# Set Up Local Network Access

## Table of Contents

- [Overview](#overview)
- [Accessing DGX Spark via VS Code Remote Window](#accessing-dgx-spark-via-vs-code-remote-window)
- [Use SSH Tunneling for Web Applications](#use-ssh-tunneling-for-web-applications)
- [Troubleshooting](#troubleshooting)

---

## Overview

## Basic idea

If you primarily work on another system, such as a laptop, and want to use your **Dell GB10 (DGX Spark)** as a remote resource, this playbook shows you how to connect and work over SSH. With SSH, you can
securely open a terminal session or tunnel ports to access web apps and APIs on your DGX Spark
from your local machine.

## What you'll accomplish

For this lab, you will be accessing the DGX Spark via 2 approaches:

1. Use VSCode to directly access DGX Spark via VSCode's inbuilt Remote Window.
2. Map a port on your laptop to a port where an app on DGX Spark is listening. This method is also known as SSH tunneling.

These approaches will enable you to run terminal commands, access web applications and manage your DGX Spark remotely from your laptop.

## What to know before starting

- Basic terminal/command line usage
- Understanding of SSH concepts and key-based authentication
- Familiarity with network concepts like hostnames, IP addresses, and port forwarding

## Prerequisites

- Your DGX Spark [device is set up](https://docs.nvidia.com/dgx/dgx-spark/first-boot.html) and you have created a local user account
- You have your DGX Spark username and password
- You have your device's IP address
- VSCode installed in your local machine

## Time & risk

- **Time estimate:** 15-20 minutes
- **Risk level:** Low - SSH setup involves credential configuration but no system-level changes to the DGX Spark device
- **Rollback:** SSH key removal can be done by editing `~/.ssh/authorized_keys` on your DGX Spark.

## Accessing DGX Spark via VS Code Remote Window

## Step 1. Download and Install VSCode

This lab requires that you download and install VSCode in your laptop. VSCode will also be used in subsequent labs.

Download and install VSCode here:

- https://code.visualstudio.com/download

## Step 2. Set up Remote Session from VSCode

After installing VSCode, follow the steps below to set up a remote session to DGX Spark.

- Launch VSCode
- On the console, open a remote window by clicking on <img src="./images/remote_window_button.png" width="28"> button
located at the bottom left corner of your screen

  <img src="./images/screenshot_1.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- A dropdown menu will be displayed. Click on **Connect Current Window to Host**

  <img src="./images/screenshot_2.png" width="500" style="padding-top: 8px; padding-bottom: 8px" >

- Click **Add New SSH Host**

  <img src="./images/screenshot_3.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- From the input bar, enter the following ssh command

```bash
ssh -p 443 <SPARK_USER>@<SPARK_IP>
```

  <img src="./images/screenshot_4.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- Select the first option when prompted to save your configuration into your SSH configuration file

  <img src="./images/screenshot_5.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- A pop up will appear at the bottom right of the screen. Click **Open Config** to view the file.

  <img src="./images/screenshot_6.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- This is the default configuration file that stores the ssh connections. You can manually add additional entries here as well.

  <img src="./images/screenshot_7.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- Open the Remote Window again by clicking on <img src="./images/remote_window_button.png" width="28"> Button > Connect Current Window to Host > IP Address you have configured

  <img src="./images/screenshot_8.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- Select **Linux** when prompted for the platform

  <img src="./images/screenshot_9.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- Enter your user password when prompted

  <img src="./images/screenshot_10.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- The console will log you into the DGX Spark environment.
  To verify that you are inside DGX Spark, open the terminal by pressing ``Ctrl + ` ``, then enter `bash`

- You should see the following hostname in your terminal, confirming that you are connected to DGX Spark

  <img src="./images/screenshot_11.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- We have prepared a jupyterlab notebook for you. Go to **Explorer > Open Folder > jupyterlab > OK**

  <img src="./images/screenshot_12.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- A new VSCode window will pop up. **You will be prompted to enter password again**

  <img src="./images/screenshot_13.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

## Let's Try It Out

- Open **lab-1.ipynb** and click `Run All`

> NOTE: If prompted to select environment, select **Python Environments > .venv(3.12.3)(Python 3.12.3)**

  <img src="./images/venv.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- A successful setup will show that the GPU is available

  <img src="./images/screenshot_14.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

## Use SSH Tunneling for Web Applications

To access web applications running on your DGX Spark, use SSH port
forwarding. In this example we'll access the DGX Dashboard web application.

> NOTE: DGX Dashboard runs on localhost, port 11000.

- On VSCode, click on the **Ports** tab

  <img src="./images/screenshot_15.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- Click **Forward a Port** > Fill in 11000 at the **Port** column. The rest of the fields will be populated for you

    <img src="./images/screenshot_16.png" width="500" style="padding-top: 8px; padding-bottom: 8px">

- Click on <img src="./images/globe.png" width="28"> button to open your browser. You should be able to access your DGX Dashboard

<img src="./images/dgx_dashboard.png" style="padding-top: 8px; padding-bottom: 8px">

## Next steps

If mDNS is configured, you can:

- Open persistent terminal sessions in your host terminal: `ssh <YOUR_USERNAME>@<SPARK_HOSTNAME>.local`.
- Forward web application ports from your host terminal: `ssh -L <local_port>:localhost:<remote_port> <YOUR_USERNAME>@<SPARK_HOSTNAME>.local`.

## Troubleshooting

## Possible issues connecting via manual SSH

| Symptom                     | Cause                                 | Fix                                                                 |
| --------------------------- | ------------------------------------- | ------------------------------------------------------------------- |
| Device name doesn't resolve | mDNS blocked on network               | Use IP address instead of hostname.local                            |
| Connection refused/timeout  | DGX Spark not booted or SSH not ready | Wait for device boot completion; SSH available after updates finish |
| Port forwarding fails       | Service not running or port conflict  | Verify remote service is active; try different local port           |
