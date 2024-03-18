# Homeassistent-Hargassner

Hargassner boiler is sending out data on telnet port 23. Different firmwares have different index for the data. To be able to see and test out what index of data you are interesseted in you can use this method.
Another approach could be to use the nano_pk script
https://github.com/TheRealKillaruna/nano_pk?tab=readme-ov-file#how-to-use-with-other-hargassner-models-or-different-firmware-versions


Use the file editor to add a tcp sensor in **configuration.yaml**

Add this this 2 sensors

<code>
 sensor:
  - platform: tcp
    name: boiler_telnet
    host: 192.168.1.123   # change to your boiler ip
    port: 23
    #timeout: 5
    payload: "\n"
    scan_interval: 300   
    value_template: "{{ value[:254] }}"   # max 255 byte is allowed in a sensor state
 - platform: tcp
    name: boiler_telnet2
    host: 192.168.1.123   # change to your boiler ip
    port: 23
    #timeout: 5
    payload: "\n"
    scan_interval: 300       
    value_template: "{{ value[254:500] }}"   # max 255 byte is allowed
</code>

Save and restart homeasistent.

This new sensor should now be visible under **Developer tools -> States**

In **Developer tools -> Template** past this in

<code>
{#  Paste your loggstring bellow #}
{% set loggstring="<DAQPRJ><ANALOG><CHANNEL id='0' name='ZK' dop='0'><CHANNEL id='1' name='O2' unit='%'><CHANNEL id='2' name='O2soll' unit='%'><CHANNEL id='3' name='TK' unit='°C'><CHANNEL id='4' name='TKsoll' unit='°C'><CHANNEL id='5' name='TRL' unit='°C'><CHANNEL id='6' name='TRLsoll' unit='°C'></DIGITAL></DAQPRJ>
"-%}
{% set boiler = loggstring.split('<DAQPRJ><ANALOG><') [1] -%}

{#  Prints explanations #}
{{ '%-5s' | format('indx') }} {{ '%-10s' | format('data') }}  {{ 'Caption' }} {{ '\n'}}       
{% for item in boiler.split("><") -%}
{% set itemname = item.split('name=') [1]  -%}
{% set telnetdata = states('sensor.panna_telnet').split(' ') [loop.index]  -%}

{{ '%-5s' | format(loop.index) }} {{ '%-10s' | format(telnetdata) }}  {{ itemname }} {{ '\n'}}       
{%- endfor %}

</code>

In the given list you see the values index. 

From this info you can edit the <code> {{ states('sensor.boiler_telnet').split(" ") [7] + "   Boiler temp " }} </code> to fit you needs

When you have found your desired values, for example...

<code>
{{ states('sensor.boiler_telnet').split(" ") [22] + "   Acc temp top" }}
{{ states('sensor.boiler_telnet').split(" ") [23] + "   Acc temp middle" }}
{{ states('sensor.boiler_telnet').split(" ") [24] + "   Acc temp bottom" }}
</code>


You can match it in this sensor skeleton.

<code>
 sensor:
  - platform: tcp
    name: Acc_temp_top
    host: 192.168.1.123   # change to your boiler ip
    port: 23
    #timeout: 5
    payload: "\n"
    scan_interval: 300   
    value_template: "{{ value.split(" ") [22] }}"   
sensor:
  - platform: tcp
    name: Acc_temp_middle
    host: 192.168.1.123   # change to your boiler ip
    port: 23
    #timeout: 5
    payload: "\n"
    scan_interval: 300   
    value_template: "{{ value.split(" ") [23] }}"   
</code>

And paste it in **configuration.yaml** and save and restart homeassistant
