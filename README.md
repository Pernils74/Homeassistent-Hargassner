# Homeassistent-Hargassner

Use the file editor to add a tcp sensor in configuration.yaml 

Add this this 2 sensors

<code>
 sensor:
  - platform: tcp
    name: Panna_telnet
    host: 192.168.1.XX   # change to your boiler ip
    port: 23
    #timeout: 5
    payload: "\n"
    scan_interval: 300   
    value_template: "{{ value[:254] }}"   # max 255 byte is allowed in a sensor state

 - platform: tcp
    name: Panna_telnet2
    host: 192.168.1.XX
    port: 23
    #timeout: 5
    payload: "\n"
    scan_interval: 300       
    value_template: "{{ value[254:500] }}"   # max 255 byte is allowed
</code>



dhjhkjahkcjh a
**test**

<code>
dlksdöjjjjjjjjjjjjjjjlköl
</code>



`jkdsljdslkjd
dädds
`



> ökaöksakskac
