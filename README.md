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
{% set loggstring="
<DAQPRJ><ANALOG><CHANNEL id='0' name='ZK' dop='0'/><CHANNEL id='1' name='O2' unit='%'/><CHANNEL id='2' name='O2soll' unit='%'/><CHANNEL id='3' name='TK' unit='°C'/><CHANNEL id='4' name='TKsoll' unit='°C'/><CHANNEL id='5' name='TRL' unit='°C'/><CHANNEL id='6' name='TRLsoll' unit='°C'/><CHANNEL id='7' name='GBF' unit='°' dop='0'/><CHANNEL id='8' name='GBF soll' unit='°' dop='0'/><CHANNEL id='9' name='Förder' unit='%' dop='0'/><CHANNEL id='10' name='Leistung' unit='%' dop='0'/><CHANNEL id='11' name='SZ' unit='%' dop='0'/><CHANNEL id='12' name='SZsoll' unit='%' dop='0'/><CHANNEL id='13' name='UD' unit='Pa' dop='0'/><CHANNEL id='14' name='UD rel' unit='%' dop='0'/><CHANNEL id='15' name='UD_NO_FILT' unit='Pa' dop='0'/><CHANNEL id='16' name='PLK' unit='%' dop='0'/><CHANNEL id='17' name='PLKsoll' unit='%' dop='0'/><CHANNEL id='18' name='TLK' unit='%' dop='0'/><CHANNEL id='19' name='TLKsoll' unit='%' dop='0'/><CHANNEL id='20' name='TPo' unit='°C'/><CHANNEL id='21' name='TPm' unit='°C'/><CHANNEL id='22' name='TPu' unit='°C'/><CHANNEL id='23' name='Pufferfüllgrad' unit='%' dop='0'/><CHANNEL id='24' name='Puffer_soll oben' unit='°C' dop='0'/><CHANNEL id='25' name='Puffer_soll unten' unit='°C' dop='0'/><CHANNEL id='26' name='PuffZustand' dop='0'/><CHANNEL id='27' name='Max Anf Kessel' unit='°C' dop='0'/><CHANNEL id='28' name='TRG' unit='°C'/><CHANNEL id='29' name='BRT' unit='°C'/><CHANNEL id='30' name='ATÜ' unit='°C' dop='2'/><CHANNEL id='31' name='ETÜ' unit='°C' dop='0'/><CHANNEL id='32' name='min.Leist.TRG' unit='%'/><CHANNEL id='33' name='max.Leist.TRG' unit='%'/><CHANNEL id='34' name='max.Leist.BRT' unit='%'/><CHANNEL id='35' name='max.Leist.Fuell' unit='%'/><CHANNEL id='36' name='max.Leist.TPO' unit='%'/><CHANNEL id='37' name='max.Leist.ExtHK' unit='%'/><CHANNEL id='38' name='TVG' unit='°C'/><CHANNEL id='39' name='TVG2' unit='°C'/><CHANNEL id='40' name='Programm' dop='0'/><CHANNEL id='41' name='Störungs Nr' dop='0'/><CHANNEL id='42' name='Max Anf ZenPuf' unit='°C' dop='0'/><CHANNEL id='43' name='I Es' unit='mA' dop='0'/><CHANNEL id='44' name='I Ra' unit='mA' dop='0'/><CHANNEL id='45' name='I Aa' unit='mA' dop='0'/><CHANNEL id='46' name='I Ahf' unit='mA' dop='0'/><CHANNEL id='47' name='I OP1' unit='mA' dop='0'/><CHANNEL id='48' name='I OPT2' unit='mA' dop='0'/><CHANNEL id='49' name='I Vert/RW' unit='mA' dop='0'/><CHANNEL id='50' name='Vert Füllstand' unit='cm'/><CHANNEL id='51' name='Fördermenge Ist' unit='%'/><CHANNEL id='52' name='Fördermenge Takt' unit='s'/><CHANNEL id='53' name='BSZ_LB_ENT' unit='min'/><CHANNEL id='54' name='BSZ_ENT_KLEIN' dop='0'/><CHANNEL id='55' name='BSZ_ES_PEL' unit='min'/><CHANNEL id='56' name='Heiz P Lambda' unit='W' dop='2'/><CHANNEL id='57' name='Heiz U Lambda' unit='V' dop='2'/><CHANNEL id='58' name='Heiz I Lambda' unit='mA' dop='0'/><CHANNEL id='59' name='U_Lambda' unit='mV'/><CHANNEL id='60' name='Es2RostPos' unit='°' dop='0'/><CHANNEL id='61' name='AschRostPos' unit='°' dop='0'/><CHANNEL id='62' name='Er2Rost Torque' unit='Nm' dop='0'/><CHANNEL id='63' name='ArRost Torque' unit='Nm' dop='0'/><CHANNEL id='64' name='Er2Rost NmSoll' unit='Nm' dop='0'/><CHANNEL id='65' name='ArRost NmSoll' unit='Nm' dop='0'/><CHANNEL id='66' name='Mode Fw' dop='0'/><CHANNEL id='67' name='TFW' unit='°C'/><CHANNEL id='68' name='Leistungsvorgabe' unit='V'/><CHANNEL id='69' name='Wasserdruck' unit='bar' dop='2'/><CHANNEL id='70' name='Spreizung' unit='°C'/><CHANNEL id='71' name='Tplat' unit='°C' dop='0'/><CHANNEL id='72' name='TÜB' unit='°C' dop='0'/><CHANNEL id='73' name='TÜB2' unit='°C' dop='0'/><CHANNEL id='74' name='Taus' unit='°C'/><CHANNEL id='75' name='TA Gem.' unit='°C'/><CHANNEL id='76' name='Programm HKM0' dop='0'/><CHANNEL id='77' name='Solltmp. ExtHK' unit='°C' dop='0'/><CHANNEL id='78' name='Solltmp. ExtHK_1' unit='°C' dop='0'/><CHANNEL id='79' name='TVL_A' unit='°C'/><CHANNEL id='80' name='TVLs_A' unit='°C' dop='0'/><CHANNEL id='81' name='TRA_A' unit='°C'/><CHANNEL id='82' name='TRs_A' unit='°C'/><CHANNEL id='83' name='HKZustand_A' dop='0'/><CHANNEL id='84' name='FRA Zustand' dop='0'/><CHANNEL id='85' name='TVL_1' unit='°C'/><CHANNEL id='86' name='TVLs_1' unit='°C' dop='0'/><CHANNEL id='87' name='TRA_1' unit='°C'/><CHANNEL id='88' name='TRs_1' unit='°C'/><CHANNEL id='89' name='HKZustand_1' dop='0'/><CHANNEL id='90' name='FR1 Zustand' dop='0'/><CHANNEL id='91' name='TVL_2' unit='°C'/><CHANNEL id='92' name='TVLs_2' unit='°C' dop='0'/><CHANNEL id='93' name='TRA_2' unit='°C'/><CHANNEL id='94' name='TRs_2' unit='°C'/><CHANNEL id='95' name='HKZustand_2' dop='0'/><CHANNEL id='96' name='FR2 Zustand' dop='0'/><CHANNEL id='97' name='TVL_B' unit='°C'/><CHANNEL id='98' name='TVLs_B' unit='°C' dop='0'/><CHANNEL id='99' name='TRA_B' unit='°C'/><CHANNEL id='100' name='TRs_B' unit='°C'/><CHANNEL id='101' name='HKZustand_B' dop='0'/><CHANNEL id='102' name='FRB Zustand' dop='0'/><CHANNEL id='103' name='TBA' unit='°C'/><CHANNEL id='104' name='TBs_A' unit='°C' dop='0'/><CHANNEL id='105' name='TB1' unit='°C'/><CHANNEL id='106' name='TBs_1' unit='°C' dop='0'/><CHANNEL id='107' name='BoiZustand_1' dop='0'/><CHANNEL id='108' name='TBB' unit='°C'/><CHANNEL id='109' name='TBs_B' unit='°C' dop='0'/><CHANNEL id='110' name='HKR Anf' unit='°C'/><CHANNEL id='111' name='Anf. HKR0' unit='°C' dop='0'/><CHANNEL id='112' name='Anf. HKR1' unit='°C' dop='0'/><CHANNEL id='113' name='Anf. HKR2' unit='°C' dop='0'/><CHANNEL id='114' name='Anf. HKR3' unit='°C' dop='0'/><CHANNEL id='115' name='Anf. HKR4' unit='°C' dop='0'/><CHANNEL id='116' name='Anf. HKR5' unit='°C' dop='0'/><CHANNEL id='117' name='Anf. HKR6' unit='°C' dop='0'/><CHANNEL id='118' name='Anf. HKR7' unit='°C' dop='0'/><CHANNEL id='119' name='Anf. HKR8' unit='°C' dop='0'/><CHANNEL id='120' name='Anf. HKR9' unit='°C' dop='0'/><CHANNEL id='121' name='Anf. HKR10' unit='°C' dop='0'/><CHANNEL id='122' name='Anf. HKR11' unit='°C' dop='0'/><CHANNEL id='123' name='Anf. HKR12' unit='°C' dop='0'/><CHANNEL id='124' name='Anf. HKR13' unit='°C' dop='0'/><CHANNEL id='125' name='Anf. HKR14' unit='°C' dop='0'/><CHANNEL id='126' name='Anf. HKR15' unit='°C' dop='0'/><CHANNEL id='127' name='TPO_ZusPuf0' unit='°C'/><CHANNEL id='128' name='TPM_ZusPuf0' unit='°C'/><CHANNEL id='129' name='TPU_ZusPuf0' unit='°C'/></ANALOG><DIGITAL><CHANNEL id='0' bit='0' name='Störung'/><CHANNEL id='0' bit='1' name='Stb'/><CHANNEL id='0' bit='2' name='NotAus'/><CHANNEL id='0' bit='3' name='RLP/PuffP'/><CHANNEL id='0' bit='4' name='RLm_auf'/><CHANNEL id='0' bit='5' name='RLm_zu'/><CHANNEL id='0' bit='6' name='FS Pellets'/><CHANNEL id='0' bit='7' name='Pell Saug'/><CHANNEL id='0' bit='8' name='FS Asche'/><CHANNEL id='0' bit='9' name='INI Asche'/><CHANNEL id='0' bit='10' name='WS freig.'/><CHANNEL id='0' bit='11' name='Akt. Code'/><CHANNEL id='0' bit='12' name='Betriebsmeldung'/><CHANNEL id='0' bit='13' name='Oel Out'/><CHANNEL id='0' bit='14' name='FW Freig.'/><CHANNEL id='0' bit='15' name='gFlP'/><CHANNEL id='0' bit='16' name='gFlM auf'/><CHANNEL id='0' bit='17' name='gFlM zu'/><CHANNEL id='0' bit='18' name='gFl2P'/><CHANNEL id='0' bit='19' name='gFl2M auf'/><CHANNEL id='0' bit='20' name='gFl2M zu'/><CHANNEL id='1' bit='0' name='L Heiz.'/><CHANNEL id='1' bit='1' name='Z Heiz.'/><CHANNEL id='1' bit='2' name='Z Geb.'/><CHANNEL id='1' bit='3' name='ES Run'/><CHANNEL id='1' bit='4' name='ES Dir'/><CHANNEL id='1' bit='5' name='RA Run'/><CHANNEL id='1' bit='6' name='RA Dir'/><CHANNEL id='1' bit='7' name='Deckel'/><CHANNEL id='1' bit='8' name='OPT1/RA2 Run'/><CHANNEL id='1' bit='9' name='OPT1/RA2 Dir'/><CHANNEL id='1' bit='10' name='OPT1 Deckel'/><CHANNEL id='1' bit='11' name='OPT2/RA3 Run'/><CHANNEL id='1' bit='12' name='OPT2/RA3 Dir'/><CHANNEL id='1' bit='13' name='OPT2 Deckel'/><CHANNEL id='1' bit='14' name='ER Run'/><CHANNEL id='1' bit='15' name='AR Run'/><CHANNEL id='1' bit='16' name='AA Run'/><CHANNEL id='1' bit='17' name='AA Dir'/><CHANNEL id='1' bit='18' name='AA Saug'/><CHANNEL id='1' bit='19' name='PutzM Run'/><CHANNEL id='1' bit='20' name='PutzM Dir'/><CHANNEL id='1' bit='21' name='Ahf Run'/><CHANNEL id='1' bit='22' name='Ahf Dir'/><CHANNEL id='1' bit='23' name='ES2 Run'/><CHANNEL id='1' bit='24' name='ES2 Dir'/><CHANNEL id='2' bit='0' name='HKPA'/><CHANNEL id='2' bit='1' name='MAA'/><CHANNEL id='2' bit='2' name='MAZ'/><CHANNEL id='2' bit='3' name='HKP1'/><CHANNEL id='2' bit='4' name='M1A'/><CHANNEL id='2' bit='5' name='M1Z'/><CHANNEL id='2' bit='6' name='HKP2'/><CHANNEL id='2' bit='7' name='M2A'/><CHANNEL id='2' bit='8' name='M2Z'/><CHANNEL id='2' bit='9' name='HKP3'/><CHANNEL id='2' bit='10' name='M3A'/><CHANNEL id='2' bit='11' name='M3Z'/><CHANNEL id='2' bit='12' name='HKP4'/><CHANNEL id='2' bit='13' name='M4A'/><CHANNEL id='2' bit='14' name='M4Z'/><CHANNEL id='2' bit='15' name='HKP5'/><CHANNEL id='2' bit='16' name='M5A'/><CHANNEL id='2' bit='17' name='M5Z'/><CHANNEL id='2' bit='18' name='HKP6'/><CHANNEL id='2' bit='19' name='M6A'/><CHANNEL id='2' bit='20' name='M6Z'/><CHANNEL id='2' bit='21' name='HKPB'/><CHANNEL id='2' bit='22' name='MBA'/><CHANNEL id='2' bit='23' name='MBZ'/><CHANNEL id='3' bit='0' name='BPA'/><CHANNEL id='3' bit='1' name='BP1'/><CHANNEL id='3' bit='2' name='BP2'/><CHANNEL id='3' bit='3' name='BP3'/><CHANNEL id='3' bit='4' name='BPB'/><CHANNEL id='3' bit='5' name='BZPA'/><CHANNEL id='3' bit='6' name='BZP1'/><CHANNEL id='3' bit='7' name='BZP2'/><CHANNEL id='3' bit='8' name='BZP3'/><CHANNEL id='3' bit='9' name='BZPB'/><CHANNEL id='4' bit='0' name='SK Aschebox'/><CHANNEL id='4' bit='1' name='SK RaDeckel'/><CHANNEL id='4' bit='2' name='SK MOp1Deckel'/><CHANNEL id='4' bit='3' name='SK Op2Deckel'/><CHANNEL id='4' bit='4' name='SK Lager'/><CHANNEL id='4' bit='5' name='SK Bruecke'/><CHANNEL id='4' bit='6' name='FLP1'/><CHANNEL id='4' bit='7' name='FLP2'/><CHANNEL id='4' bit='8' name='ATW'/><CHANNEL id='4' bit='9' name='Entasch gesp.'/><CHANNEL id='4' bit='10' name='Puffer(PWT) Pumpe'/><CHANNEL id='4' bit='11' name='Use MotAktiv'/><CHANNEL id='4' bit='12' name='PB Lastabwurf'/><CHANNEL id='4' bit='13' name='HKV'/><CHANNEL id='4' bit='14' name='DIN_E13'/><CHANNEL id='4' bit='15' name='DIN_E14'/><CHANNEL id='4' bit='16' name='KaskM A Anf.Ent.'/><CHANNEL id='4' bit='17' name='KaskM B Anf.Ent.'/><CHANNEL id='4' bit='18' name='KaskM C Anf.Ent.'/><CHANNEL id='4' bit='19' name='KaskM D Anf.Ent.'/><CHANNEL id='4' bit='20' name='KaskM E Anf.Ent.'/><CHANNEL id='4' bit='21' name='KaskM F Anf.Ent.'/><CHANNEL id='4' bit='22' name='KaskM G Anf.Ent.'/><CHANNEL id='4' bit='23' name='KaskM H Anf.Ent.'/><CHANNEL id='5' bit='0' name='Feuerung aktiv (F2)'/><CHANNEL id='5' bit='1' name='Feuerung aktiv (F3)'/><CHANNEL id='5' bit='2' name='eCleaner 1 aktiv'/><CHANNEL id='5' bit='3' name='eCleaner 2 aktiv'/><CHANNEL id='5' bit='4' name='eCleaner AA Run'/><CHANNEL id='5' bit='5' name='eCleaner AA Dir'/><CHANNEL id='5' bit='8' name='eCleaner RR Run'/><CHANNEL id='5' bit='9' name='SK eCleaner 1'/><CHANNEL id='5' bit='10' name='SK eCleaner 2'/><CHANNEL id='5' bit='11' name='M-AFS FS1 Run'/><CHANNEL id='5' bit='12' name='M-AFS FS1 Dir'/><CHANNEL id='5' bit='13' name='M-AFS FS2 Run'/><CHANNEL id='5' bit='14' name='M-AFS FS2 Dir'/><CHANNEL id='5' bit='15' name='M-AFS FS3 Run'/><CHANNEL id='5' bit='16' name='M-AFS FS3 Dir'/><CHANNEL id='5' bit='17' name='SK M-AFS FS1'/><CHANNEL id='5' bit='18' name='SK M-AFS FS2'/><CHANNEL id='5' bit='19' name='SK M-AFS FS3'/><CHANNEL id='5' bit='20' name='M-AFS eCleaner FS1 Run'/><CHANNEL id='5' bit='21' name='M-AFS eCleaner FS1 Dir'/><CHANNEL id='5' bit='22' name='M-AFS eCleaner FS2 Run'/><CHANNEL id='5' bit='23' name='M-AFS eCleaner FS2 Dir'/><CHANNEL id='5' bit='24' name='M-AFS eCleaner FS3 Run'/><CHANNEL id='5' bit='25' name='M-AFS eCleaner FS3 Dir'/><CHANNEL id='5' bit='26' name='SK M-AFS eCleaner FS1'/><CHANNEL id='5' bit='27' name='SK M-AFS eCleaner FS2'/><CHANNEL id='5' bit='28' name='SK M-AFS eCleaner FS3'/><CHANNEL id='5' bit='29' name='FS Asche eCleaner'/><CHANNEL id='6' bit='0' name='ExtHK'/><CHANNEL id='6' bit='1' name='ExtHK_1'/><CHANNEL id='6' bit='2' name='ExtHK_2'/><CHANNEL id='6' bit='3' name='ExtHK_3'/><CHANNEL id='6' bit='4' name='HKP Ext'/><CHANNEL id='6' bit='5' name='HKP Ext_1'/><CHANNEL id='6' bit='6' name='HKP Ext_2'/><CHANNEL id='6' bit='7' name='HKP Ext_3'/><CHANNEL id='6' bit='8' name='KASK1 MinLeist'/><CHANNEL id='6' bit='9' name='KASK2 MinLeist'/><CHANNEL id='6' bit='10' name='KASK3 MinLeist'/><CHANNEL id='6' bit='11' name='KASK4 MinLeist'/><CHANNEL id='6' bit='12' name='KASK5 MinLeist'/><CHANNEL id='6' bit='13' name='KASK6 MinLeist'/><CHANNEL id='6' bit='14' name='KASK1 MaxLeist'/><CHANNEL id='6' bit='15' name='KASK2 MaxLeist'/><CHANNEL id='6' bit='16' name='KASK3 MaxLeist'/><CHANNEL id='6' bit='17' name='KASK4 MaxLeist'/><CHANNEL id='6' bit='18' name='KASK5 MaxLeist'/><CHANNEL id='6' bit='19' name='KASK6 MaxLeist'/><CHANNEL id='6' bit='20' name='KASK1 Run'/><CHANNEL id='6' bit='21' name='KASK2 Run'/><CHANNEL id='6' bit='22' name='KASK3 Run'/><CHANNEL id='6' bit='23' name='KASK4 Run'/><CHANNEL id='6' bit='24' name='KASK5 Run'/><CHANNEL id='6' bit='25' name='KASK6 Run'/><CHANNEL id='6' bit='26' name='KASK1 OK'/><CHANNEL id='6' bit='27' name='KASK2 OK'/><CHANNEL id='6' bit='28' name='KASK3 OK'/><CHANNEL id='6' bit='29' name='KASK4 OK'/><CHANNEL id='6' bit='30' name='KASK5 OK'/><CHANNEL id='6' bit='31' name='KASK6 OK'/><CHANNEL id='7' bit='0' name='DReg P2'/><CHANNEL id='7' bit='1' name='DReg P3'/><CHANNEL id='7' bit='2' name='DReg Mi auf'/><CHANNEL id='7' bit='3' name='DReg Mi zu'/><CHANNEL id='7' bit='5' name='FwReg P2'/><CHANNEL id='7' bit='6' name='FwReg Mi auf'/><CHANNEL id='7' bit='7' name='FwReg Mi zu'/><CHANNEL id='7' bit='9' name='DReg3 P2'/><CHANNEL id='7' bit='10' name='DReg3 P3'/><CHANNEL id='7' bit='11' name='DReg3 Mi auf'/><CHANNEL id='7' bit='12' name='DReg3 Mi zu'/><CHANNEL id='7' bit='14' name='Vert/RW Run'/><CHANNEL id='7' bit='15' name='Vert/RW Dir'/><CHANNEL id='7' bit='16' name='Vert/RW Ext Anf'/><CHANNEL id='7' bit='17' name='Vert Ini Unten'/><CHANNEL id='7' bit='18' name='Vert Ini Oben'/><CHANNEL id='7' bit='19' name='Vert Anf Out'/><CHANNEL id='7' bit='20' name='Vert/RW Ext Err'/><CHANNEL id='7' bit='27' name='PwrC P1 OK'/><CHANNEL id='7' bit='28' name='PwrC P2 OK'/><CHANNEL id='7' bit='29' name='PwrC P3 OK'/><CHANNEL id='7' bit='30' name='PwrC Stromgrenze'/><CHANNEL id='7' bit='31' name='PwrC PFC OK'/></DIGITAL></DAQPRJ>
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
