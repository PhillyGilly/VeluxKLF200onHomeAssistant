# Velux KLF 200 on Home Assistant

I found a how-to guide somewhere and now can't find the author so big apologies for not attributing it.  
However I have two Velux windows with electric opening and electric blinds which I controlled using Velux remote. However - for the usual reasons - I wanted to bring them into my Home Assitant eco-system.

## Set up the KLF200
1. On power on the KLF is LAN disabled and Wifi enabled for 10 minutes
2. Use mobile phone to find the SSID: VELUX_KKLF_BCD1 and log in with password on back of KLF
3. Open web browser to http://klf200.velux and login using velux123
4. Configure as “Interface”
5. Configure network to be DHCP, LAN always on Wifi on for 10 minutes.
6. Import the devices from the 200 remote. You need to send the codes form the remote under the add device menu.
7. Set Static IP in your router.

## Set up Home Assistant
8. Add this integration https://www.home-assistant.io/integrations/velux
9. During configuration, you will be asked for a hostname and password:
      Hostname: enter the IP address of the KLF 200 gateway.
      Password: enter the password of the gateway’s wireless access point (printed on the underside - e.g. ZtD5PdYCFy not the web login password).
    You must complete the configuration within 5 minutes of rebooting the KLF 200 gateway while the access point is still available.
10. There is a problem with the KLF 200 gateway where the connection cannot be established after a restart of Home Assistant, only a manual power off and on fixes this. As a workaround, you can use an automation to force a restart of the KLF 200 before exiting Home Assistant, like this:
```
automation:
  - alias: "KLF reboot on hass stop event"
    description: "Reboots the KLF200 in order to avoid SSL Handshake issue"
    triggers:
      - trigger: homeassistant
        event: shutdown
    actions:
      - action: velux.reboot_gateway
```
11. That's pretty much it. Power cycle the KLF200 and restart HA and you should be in business.
