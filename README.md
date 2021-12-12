# thermia-telegraf

Here's some Telegraf configuration that pull metrics out of your Thermia heat pump and save them into a database of your choice.

Read up on Telegraf [here](https://docs.influxdata.com/telegraf)

All info on Modbus registers for the Genesis platform was found [here](http://www.tcmadmin.thermia.se/docroot/dokumentbank/Modbus%20protocol%20for%20Genesis%20platform%2010.pdf).

The example config below gets data once per minute and saves it to an InfluxDB instance.

```
[agent]
  interval = "1m"

[[outputs.influxdb]]
  urls = ["http://localhost:8086"]
  database = "thermia"
  precision = "s"

[[inputs.modbus]]
  name = "Thermia"
  slave_id = 1
  timeout = "1s"
  controller = "tcp://10.0.10.45:502"
  input_registers = [
    { name = "brine_in_temperature",  data_type="FIXED", scale=0.01, byte_order="AB", address = [10]},
    { name = "brine_out_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [11]},
    { name = "compressor_speed_RPM", data_type="FIXED", scale=1.0, byte_order="AB", address = [5]},
    { name = "discharge_pipe_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [7]},
    { name = "condenser_in_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [8]},
    { name = "condenser_out_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [9]},
    { name = "outdoor_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [13]},
    { name = "tap_water_top_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [15]},
    { name = "tap_water_lower", data_type="FIXED", scale=0.01, byte_order="AB", address = [16]},
    { name = "tap_water_weighted", data_type="FIXED", scale=0.01, byte_order="AB", address = [17]},
    { name = "selected_heat_curve", data_type="FIXED", scale=0.01, byte_order="AB", address = [19]},
    { name = "condenser_circulation_pump_speed_%", data_type="FIXED", scale=0.01, byte_order="AB", address = [39]},
    { name = "brine_circulation_pump_speed_%", data_type="FIXED", scale=0.01, byte_order="AB", address = [44]},
    { name = "hgw_supply_line_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [45]},
    { name = "compressor_operating_hours", data_type="FIXED", scale=1.0, byte_order="AB", address = [48]},
    { name = "tap_water_operating_hours", data_type="FIXED", scale=1.0, byte_order="AB", address = [50]},
    { name = "compressor_speed_percent", data_type="FIXED", scale=0.01, byte_order="AB", address = [54]},
    { name = "compressor_current_gear", data_type="FIXED", scale=0.01, byte_order="AB", address = [61]},
  ]
```
