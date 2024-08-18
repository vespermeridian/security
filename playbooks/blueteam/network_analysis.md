# Network Analysis Playbook

## Wireshark

#### Description
Free and open-source network protocol and traffic analyzer that can be used to capture network traffic, troubleshoot networks, and much more.

#### Things to know / Pre-requisites
* If using a wireless adapter you should use a network interface capable of monitoring mode in order to capture packets. You will not be able to without it.
* You should run wireshark as *sudo* to get complete usability, otherwise you may see problems during analysis

#### PCAP Files
PCAP files are the packets capture via Wireshark used for analysis. It captures packets from layer 2-7 in the OSI model. In those PCAP files you can analyze headers to find information about network requests.

In particular for blue teams the following are interesting:

* Flags
* Source
* Destination
* Data
* Protocol

#### Installation
[Wireshark Website](https://www.wireshark.org/)

```sudo apt-get install wireshark -y```

#### Customization
* Recommendations
   * Preferences
      * Appearance (Set up your view for comfort here)
         * Columns
            * Change Time column type to UTC
            * Add Source Port column (unresolved)
            * Add Destination Port column (unresolved)
         * Layout
            * Packet separator selection is great
   * Align columns in view pain on center to help with readability
* View
   * Coloring rules (Can be handy if you have custom accessibility needs)
      
### Traffic captures
---

#### Capture Filters
* Filters can be combined with `and`, `or`, or `not`
* `tcp` filters tcp only packets, as do other filters like `udp`, and `http`
* `src <ip address>` limits results to communications that IP address as would `dst`

#### Display Filters
* `http.request` for HTTP requests
* `ip.addr
#### Analyzing Traffic

#### Common Commands/Functions


#### SOP Planning
1. During an event packets should be captured.