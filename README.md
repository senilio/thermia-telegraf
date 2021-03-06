# thermia-telegraf

Here's some Telegraf configuration that pulls metrics from your Thermia heat pump and saves them into a database of your choice.

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
    { name = "compressor_speed_rpm", data_type="FIXED", scale=1.0, byte_order="AB", address = [5]},
    { name = "discharge_pipe_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [7]},
    { name = "condenser_in_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [8]},
    { name = "condenser_out_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [9]},
    { name = "outdoor_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [13]},
    { name = "tap_water_top_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [15]},
    { name = "tap_water_lower_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [16]},
    { name = "tap_water_weighted_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [17]},
    { name = "system_supply_line_calculated_set_point", data_type="FIXED", scale=0.01, byte_order="AB", address = [18]},
    { name = "selected_heat_curve", data_type="FIXED", scale=0.01, byte_order="AB", address = [19]},
    { name = "condenser_circulation_pump_speed_%", data_type="FIXED", scale=0.01, byte_order="AB", address = [39]},
    { name = "brine_circulation_pump_speed_%", data_type="FIXED", scale=0.01, byte_order="AB", address = [44]},
    { name = "hgw_supply_line_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [45]},
    { name = "compressor_operating_hours", data_type="FIXED", scale=1.0, byte_order="AB", address = [48]},
    { name = "tap_water_operating_hours", data_type="FIXED", scale=1.0, byte_order="AB", address = [50]},
    { name = "external_additional_heater_operating_hours", data_type="FIXED", scale=1.0, byte_order="AB", address = [52]},
    { name = "compressor_speed_percent", data_type="FIXED", scale=0.01, byte_order="AB", address = [54]},
    { name = "compressor_current_gear", data_type="FIXED", scale=0.01, byte_order="AB", address = [61]},
    { name = "internal_additional_heater_current_step", data_type="FIXED", scale=1.0, byte_order="AB", address = [67]},
    { name = "bubble_point", data_type="FIXED", scale=0.01, byte_order="AB", address = [122]},
    { name = "dew_point", data_type="FIXED", scale=0.01, byte_order="AB", address = [124]},
    { name = "superheat_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [125]},
    { name = "sub_cooling_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [126]},
    { name = "low_pressure_side", data_type="FIXED", scale=0.01, byte_order="AB", address = [127]},
    { name = "high_pressure_side", data_type="FIXED", scale=0.01, byte_order="AB", address = [128]},
    { name = "liquid_line_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [129]},
    { name = "suction_gas_temperature", data_type="FIXED", scale=0.01, byte_order="AB", address = [130]},
  ]
```
