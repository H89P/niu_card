# niu_card
This is my documentation of my NIU Card in Home Assistant

The current status of the implementation looks like this:


<img src="https://github.com/H89P/niu_card/blob/master/NIU_Lovelace.png" width="300">

In the following all details of my hass.io implementation are documented:

# Documentation:

* Custom components
* [Configuration.yaml](https://github.com/H89P/niu_card/blob/master/resources/configuration.yaml)
* [Lovelace.yaml](https://github.com/H89P/niu_card/blob/master/resources/lovelace.yaml)
* [Node-Red Config](https://github.com/H89P/niu_card/blob/master/resources/node-red.md)
* [Telegram Bot](https://www.home-assistant.io/components/telegram/)
* [Picutures](https://github.com/H89P/niu_card/tree/master/resources/pictures)


## Custom components:


* [**layout-card**](https://github.com/thomasloven/lovelace-layout-card) - 
  This component is used to make the screen one column only. This is important for the Desktop / Tablet view. This component requires the component [card-tools](https://github.com/thomasloven/lovelace-card-tools) to work
* [**bar-card**](https://github.com/custom-cards/bar-card) - 
  This component is used to show the charging state
* [**mini-graph-card**](https://github.com/kalkih/mini-graph-card) - 
  This component is used to show the charging cycle in Watts

## Node-Red Addon:
This addon is used for the whole API request implementation and some automations. This of course can be done with other means as well but was done with Node Red as it was the most practicable solution for me.

The addon can be found in the hass.io addon store. Documentation can be found [here](https://github.com/hassio-addons/addon-node-red/blob/v4.0.6/README.md)

The documentation of my Node-Red flow can be found [here](https://github.com/H89P/niu_card/blob/master/resources/node-red.md)
