> Work in Progress - these configs have not yet been tested

## Introduction

### Goal
To get an Ubiquiti Edgerouter X SFP (ERX-SFP) into 'bypass mode' to bypass an ATT Fiber Router. In very basic terms, generally what happens is ATT runs a physical fiber optic line into your house and it goes into a device called an Optical Network Terminal (ONT). The ONT then goes into an ATT Gateway/Router. The G/R then is used to connect your devices to the internet. The G/R tends to be pretty basic, and presumably if you are reading this you are using Ubuquiti gear and would like more control over your home network than what is available by using ATT's hardware. The problem is ATT locks everything down pretty tightly, so it's not a straightforward process to do away with their hardware.

What I am trying to accomplish is to 'bypass' the G/R by connecting the ONT directly to the ERX-SFP. The G/R will still technically be needed to authenticate the connection with ATT's network, but once the connection is authenticated the G/R doesn't do anything. In fact, you can unplug the G/R completely once the connection is successfully established. However, just note that if the connection gets interrupted (power outage, etc), you will need to reconnect the G/R to be able to authenicate again (or just leave the G/R connected anyway).

There are a lot of resources online that focus on the Ubiquiti Unified Security Gateway (USG), however the settings are slightly different. There are also some resources online that focus on the Ubuqiti Edgerouter Lite (ERL), which is very similar to the ERX-SFP except that the ERL (effectively) only has one LAN port while the ERX-SFP has one SFP port and 4 LAN ports (assuming one of your ports is being used for WAN).

What I want to accomplish is:
1. Use eth0 as the WAN port (connected directly to the ONT)
2. Use eth5 (the SFP port with an RJ45 SFP adapter) to authenticate with the G/R
3. Use the remaining eth ports (1, 2, 3, 4) as a LAN switch

### Files

You need to primarily look at `erx-setup.md`, the other files are probably not important for what you need right now. I'll clean this up eventually once I have time to play with it and make sure everything is working and documented correctly.

### Primary Sources
  - https://www.reddit.com/r/Ubiquiti/comments/ekz8us/anyone_had_success_with_an_erx_sfp_and_bridge/
  - https://www.dslreports.com/forum/r32488644-eap-proxy-bypass-on-an-ER-X-utilizing-the-leftover-ports
  - https://github.com/jaysoffian/eap_proxy
  
### Other Sources
  - https://medium.com/@mrtcve/at-t-gigabit-fiber-modem-bypass-using-unifi-usg-updated-c628f7f458cf
  - https://www.reddit.com/r/Ubiquiti/comments/cjw9jt/howto_bypass_att_gateway_with_erx/
  - https://www.dslreports.com/forum/r29903721-AT-T-Residential-Gateway-Bypass-True-bridge-mode
  - https://www.dslreports.com/forum/r31900599-ATT-TrueBridge-Mode-for-for-Ubiquity-Security-Gateway-USG
  
