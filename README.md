# niu_card
This is my documentation of my NIU Card in Home Assistant

The current status of the implementation looks like this:


<img src="https://github.com/H89P/niu_card/blob/master/NIU_Lovelace.png" width="300">

In the following all details of my hass.io implementation are documented:

# Documentation:

* Custom components
* Configuration.yaml
* [Lovelace.yaml](https://github.com/H89P/niu_card/blob/master/resources/lovelace.yaml)


## Custom components:


* [**layout-card**](https://github.com/thomasloven/lovelace-layout-card) - 
  This component is used to make the screen one column only. This is important for the Desktop / Tablet view. This component requires the component [card-tools](https://github.com/thomasloven/lovelace-card-tools) to work
* [**bar-card**](https://github.com/custom-cards/bar-card) - 
  This component is used to show the charging state
* [**mini-graph-card**](https://github.com/kalkih/mini-graph-card) - 
  This component is used to show the charging cycle in Watts

