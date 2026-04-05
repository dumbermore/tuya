# Beken T1-2S-NL

## Affected Device(s)

<a href="https://www.aziot.life/collections/smart-switches/products/aziot-1-node16amp-smart-switch-wifi-bluetooth-communication-made-in-india-timer-function-works-with-google-home-and-amazon-alexa">
  <img src="https://github.com/dumbermore/tuya/blob/main/assets/1_NODE.png?raw=true" width="200" alt="AZIOT 1 NODE">
</a>

#### [CVE-2026-30613](https://www.cve.org/CVERecord/SearchResults?query=CVE-2026-30613)

> [!WARNING]
> **Vendor Neutrality:** While this guide uses an **AZIOT** switch as a reference, the extraction methodology is effective against any device using the Beken chipset family.
---
<br>

## Chip Info
<a href="https://developer.tuya.com/en/docs/iot/T1-2S-module-datasheet?id=Kcl2d5xjy1rly">
  <img src="https://github.com/dumbermore/tuya/blob/main/assets/beken.png?raw=true" width="400" alt="Beken T1-2S-NT">
</a>

### Pin Description
<table>
  <tr> <td><b>Tx1, Rx1</b></td> <td>Firmware Flashing</td> </tr>
  <tr> <td><b>TX2, Rx2</b></td> <td>UART Output</td> </tr>
  <tr> <td><b>CEN</b></td> <td>Chip Enable</td> </tr>
  <tr> <td><b>CSN</b></td> <td>Chip Select</td> </tr>
</table>

###  Memory Mapping
<table>
  <tr> <td><b>0x000000</b></td> <td>Bootloader</td> </tr>
  <tr> <td><b>0x010000</b></td> <td>Application</td> </tr>
  <tr> <td><b>0x01f500</b></td> <td>Storage</td> </tr>
</table>
<br>


###  Tools used
1. bk7231tools [link](https://github.com/tuya-cloudcutter/bk7231tools.git)
2. picocom
3. xxd
---
<br>

## Methodology

### 1. Check Default UART log
```bash
sudo picocom -b 115200 /dev/ttyUSB0
```
<details> 
  
  <summary>Click to expand terminal output</summary>
  
```text
picocom v3.1

port is        : /dev/ttyUSB0
flowcontrol    : none
baudrate is    : 115200
parity is      : none
databits are   : 8
stopbits are   : 1
escape is      : C-a
local echo is  : no
noinit is      : no
noreset is     : no
hangup is      : no
nolock is      : no
send_cmd is    : sz -vv
receive_cmd is : rz -vv -E
imap is        : 
omap is        : 
emap is        : crcrlf,delbs,
logfile is     : none
initstring     : none
exit_after is  : not set
exit is        : no

Type [C-a] [C-h] to see available commands
Terminal ready

V: T1_2.0.0
REG:cpsr     spsr     r13      r14

SVC:000000d3 00000010 00401c1c 000033a0 
IRQ:000000d2 00000010 00401e0c ed6bb0af 
FIR:000000d1 00000010 00401ffc 7bd9092b 
ST:00000000
__read_manage_block: mag->blockid=255, id=0, mag->state=255.
__read_manage_block: mag->magic=0xffffffff, rescrc=0x00000000, mag->crc32=0xffffffff.
__read_manage_block: mag->blockid=255, id=1, mag->state=255.
__read_manage_block: mag->magic=0xffffffff, rescrc=0x00000000, mag->crc32=0xffffffff.
_judge_ota_info checkerr0:1, checkerr1:1
init err -5
jump to:0x10000
prvHeapInit-start addr:0x415120, size:175840
[Flash]id:0x852015
bk_timer_init exit
spi_init exit
sctrl_sta_ps_init
**********tuya_upgrade_main need upgrade? [255]**********
cset:0 0 0 0
[FUNC]rwnxl_init
IP Rev: 431458e2
tkl_ethernetif_init
tkl_[bk]tx_txdesc_flush
ex_txdesc_flush
[FUNC]intc_init
[FUNC]calibration_main
gpio_level=1,txpwr_state=15
device_id=0x22068000
[01-01 00:00:00 ty E][4163][log_seq.c:884] logseq empty
[01-01 00:00:00 ty N][4163][ty_sys.c:277] sdk_info:< TuyaOS V:3.8.31 BS:40.00_PT:2.3_LAN:3.5_CAD:1.0.5_CD:1.0.0 >
< BUILD AT:2025_01_08_15_12_50 BY ci_manage FOR tuyaos-iot AT T1 >
IOT DEFS < WIFI_GW:1 DEBUG:0 KV_FILE:0 LITTLE_END:1 SL:0 OPERATING_SYST[01-01 00:00:00 ty N][4163][ty_sys.c:278] name:oem_t1_switch:1.1.9
[01-01 00:00:00 ty N][4163][ty_sys.c:279] firmware compiled at Jun 25 2025 15:18:52
[01-01 00:00:00 ty N][4163][ty_sys.c:280] system reset reason:[0]
[01-01 00:00:00 ty N][4163][simple_flash_protected.c:86] init protected data length 506 wr_cnt 8
[01-01 00:00:00 ty N][4163][simple_flash.c:447] key_addr: 0x1f5000   block_sz 4096
[01-01 00:00:00 ty N][4163][simple_flash.c:533] get key:
calibration_main over
flash txpwr table:0xf
dif g and n20 ID in flash:4
read txpwr tab from flash success
uncali adc value:[6c df 00]
temp in flash is:234
lpf_i & q in flash is:84, 90
found flash XTAL:86
use xtal:86
xtal_cali:86
--init_xtal = 86
[FUNC]ps_init
[FUNC]func_init_extended OVER!!!

start_type:0
Version:
app_init finished
[01-01 00:00:00 ty E][416d][api_lib.c:227] LWIP_NETCONN_THREAD_SEM_FREE:not find thread sem

[01-01 00:00:00 ty N][4163][tuya_tls.c:911] uni_random_init...
[01-01 00:00:00 ty N][4163][tuya_tls.c:354] tuya_tls_rand_init ok!
[01-01 00:00:00 ty N][4163][tuya_svc_devos.c:319] gw_cntl->gw_wsm.stat:0
temp_code:15 - adc_code:255 - adc_trend:[13]:234->[11]:254
init_xtal:86, delta:2, last_xtal:86
[01-01 00:00:00 ty N][4163][ty_sys.c:343] mf_init succ
[01-01 00:00:00 ty E][4163][ty_app_elec_oem_config.c:358] get net_reuse_m fail
[01-01 00:00:00 ty E][4163][ty_app_elec_oem_config.c:274] ret:-6
[01-01 00:00:00 ty N][4163][ty_app_elec_oem_config.c:531] oem cfg get value, ffc_select = 1
[01-01 00:00:00 ty N][4163][ty_app_elec_oem_config.c:539] oem cfg get value, pair_time = 30
[01-01 00:00:00 ty N][4163][app_count_reset_times.c:61] reset info -> reason is 0
find old KV:reset_cnt
defrag no enable
write process
write_cnt=1
[01-01 00:00:00 ty N][4163][app_count_reset_times.c:143] rst cnt=2, threshold=3
[01-01 00:00:00 ty E][4163][ty_app_elec_oem_hardware.c:200] ret:-6
[01-01 00:00:00 ty E][4163][app_elec_button.c:533] ret:-6
[01-01 00:00:00 ty E][4163][ty_app_elec_oem_hardware.c:376] ret:-6
[01-01 00:00:00 ty E][4163][ty_app_elec_oem_hardware.c:539] ret:-6
[01-01 00:00:00 ty N][4163][app_elec_led.c:173] sg_net_led.is_mux:0
[01-01 00:00:00 ty N][4163][app_elec_led.c:221] sg_net_led.sg_net_led_count:1
tkl_wifi_scan_ap
[sa_sta]MM_RESET_REQ
[bk]tx_txdesc_flush
[sa_sta]ME_CONFIG_REQ
[sa_sta]ME_CHAN_CONFIG_REQ
[sa_sta]MM_START_REQ
sizeof(wpa_supplicant)=928
hapd_intf_add_vif,type:2, s:0, id:0
wpa_dInit
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
netif_is_added: 0x40c2e8
netif_is_added: 0x40c2a0
net_wlan_add_netif already exist!, vif_idx:0
mac  0:33:7a:5a:ee:7e
net_wlan_add_netif done!, vif_idx:0
wpa_supplicant_req_scan
Setting scan[retry1] request: 0.000000 sec
wpa_suppwlan_sta_scan_once trielscan_once tries count:s count: 0
 0
wpa_supplicant_scan 868
wpa_drv_scan
wpa_send_scan_req
ht in scan
scan_start_req_handler
wpa_driver_scan_start_cb
RSSI: 36:68:93:d6:5e:56  -41 -> -40
RSSI: 36:68:93:d6:5e:56  -40 -> -37
RSSI: 98:03:8e:0f:7b:a1  -46 -> -45
scanu_confirm: status 0, upload_cnt 95, recv_cnt 95, time 1327268us, result 22
wpa_driver_scan_cb
Scan completed in 1.324000 seconds
wpa_get_scan_rst:22
[01-01 00:00:02 ty N][4163][tuya_iot_wifi_api.c:307] wifi soc init. pid:keyfxuj3d5wg53wx firmwarekey:keyfxuj3d5wg53wx ver:1.1.9
[01-01 00:00:02 ty N][4163][tuya_wifi_link.c:86] start wifi link params validate, nc_tp:10 md:0
[01-01 00:00:02 ty N][4163][tuya_wifi_link.c:108] gw_wsm.nc_tp:10
[01-01 00:00:02 ty N][4163][tuya_wifi_link.c:109] gw_wsm.md:0
[01-01 00:00:02 ty E][4163][tuya_wifi_reset.c:644] read wf start mode err:-6
[01-01 00:00:02 ty N][4163][tuya_svc_devos.c:547] Last reset reason: 0
[01-01 00:00:02 ty N][4163][tuya_svc_devos.c:376] gw_cntl->gw_if.abi:0 input:0
[01-01 00:00:02 ty N][4163][tuya_svc_devos.c:377] gw_cntl->gw_if.product_key:keyfxuj3d5wg53wx, input:keyfxuj3d5wg53wx
[01-01 00:00:02 ty N][4163][tuya_svc_devos.c:378] gw_cntl->gw_if.tp:0, input:0
[01-01 00:00:02 ty N][4163][tuya_svc_devos.c:381] gw_cntl->gw_if.firmware_key:keyfxuj3d5wg53wx, input:keyfxuj3d5wg53wx
[01-01 00:00:02 ty N][4163][tuya_svc_devos.c:580] enter success_proc
[01-01 00:00:02 ty N][4163][tuya_svc_devos.c:583] serial_no:00337a5aee7e
[01-01 00:00:02 ty E][41e6][tuya_wifi_reset.c:644] read wf start mode err:-6
[01-01 00:00:02 ty E][4163][app_elec_button.c:533] ret:-6
[01-01 00:00:02 ty N][4163][tdl_button_manage.c:356] button no existence
[01-01 00:00:02 ty E][4163][tdl_button_manage.c:610] tdl create updata err
rw_ieee80211_set_country[01-01 00:00:02 ty E][4 3][app_elec_button.c:2163][app_elec_button.c:22] ret:-1
code: CN
222] ret:-1
pair_time 3001-01 00:00:02 ty N][4-:02 ty N][4163][app_be163][app_beacon_remote.cacon_remote.c:364] ble_:364] ble_pair_time 30
mode: MAN
         UAL
bk_wlan cca closed
net_wlan_remove_netif done!, vif_idx:0
sm external auth not in corrent state
Cancelling scan request
scanu completed
wpa/host apd remove_if:0
ap net info ip: 192.168.176.1, mask: 255.255.255.0, gw: 192.168.176.1
ssid:SmartLife-EE7E, key:, channel: 6
[saap]MM_RESET_REQ
[bk]tx_txdesc_flush
[saap]ME_CONFIG_REQ
[saap]ME_CHAN_CONFIG_REQ
[saap]MM_START_REQ
[csa]csa_in_progress[0:0]-clear
hapd_intf_add_vif,type:3, s:0, id:0
apm start with vif:0
------beacon_int_set:100 TU
set_active param 0
[msg]APM_STOP_CFM
update_ongoing_1_bcn_update
vif_idx:0, ch_idx:0, bcmc_idx:1
sending broadcast_deauth:5
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
update_ongoing_1_bcn_update
netif_is_added: 0x40c2e8
net_wlan_add_netif already exist!, vif_idx:0
mac  0:33:7a:5a:ee:7f
net_wlan_add_netif done!, vif_idx:0
uap_ip_start

configuring uap(with IP)def netif is sta
sending broadcast_deauth:5
[01-01 00:00:02 ty N][41e6][tuya_wifi_status.c:167] cur stat:13  0x5edc1 -->>
[01-01 00:00:02 ty N][41e6][tuya_wifi_status.c:169] wifi netstat changed to:13  -->>
[01-01 00:00:02 ty N][41e6][tuya_wifi_status.c:173] report wifi netstat[13] to callback  -->>
[01-01 00:00:02 ty N][41e6][ty_app_elec_trigger_network.c:83] --->net state change to:13
[01-01 00:00:02 ty N][41e6][tuya_wifi_reset.c:410] timer stated, short timer:0x0, long timer:0x0
[01-01 00:00:02 ty N][41e6][tuya_bt_link.c:63] bt startup attr:ff
[01-01 00:00:02 ty N][41e6][tuya_ble_svc.c:1204] upd product_id type:1 keyfxuj3d5wg53wx
initia[01-01 00:00:l00:00:02 ty N][41a8][a02 ty N][41a8][app_elecpp_elec_led.c:323] indi_led.c:323] indicate_mocate_mode:2
ble mac:00-33-7a-5a-ee-7f
rwip_heap_env addr:0x427328 size:972
rwip_heap_msg addr:0x42b558 size:3208
rwip_heap_non_ret addr:0x427700 size:668
xvr_reg_init
tx_pwr_idx:34
enter normal mode
de:2
[01-01 00:00:02 ty N][41e6][tuya_ble_svc.c:1299] ty bt sdk init success finish
[01-01 00:00:02 ty N][41e6][tuya_ble_sdk.c:384] start ble adv!!!
[01-01 00:00:02 ty N][41e6][ble_gap.c:2509] Start Adv
[01-01 00:00:02 ty E][41e6][tuya_svc_timer_task.c:1376] read timer_full_key failed
[01-01 00:00:02 ty N][41e6][tuya_svc_devos.c:231] __devos_init_evt success
[01-01 00:00:02 ty N][41e6][ble_gap.c:2509] Start Adv
[01-01 00:00:02 ty N][41e6][tuya_ble_svc.c:1386] ble adv updated
[01-01 00:00:02 ty E][41e6][wifi_netcfg_frame_transporter.c:325] timer is not run,wait
temp_code:22 - adc_code:242 - adc_trend:[11]:254->[12]:244
init_xtal:86, delta:1, last_xtal:88
defrag enable,30,5912
find old KV:reset_cnt
defrag
MOVE
empty_block_num=2 > 1
defrag no need block=255
write process
write_cnt=1
[01-01 00:00:07 ty N][41b9][tfm_timing_storage.c:168] storage sucess
temp_code:25 - adc_code:236 - adc_trend:[12]:244->[13]:234
init_xtal:86, delta:0, last_xtal:87
temp_code:29 - adc_code:225 - adc_trend:[13]:234->[14]:224
init_xtal:86, delta:-1, last_xtal:86
[01-01 00:04:44 ty N][427c][tuya_ble_svc.c:747] Ble Connected
[01-01 00:04:44 ty N][427c][tuya_ble_data_handler.c:69] ble reset sn
[01-01 00:04:44 ty N][427c][tuya_ble_svc.c:1685] ble set conn stat:1
[01-01 00:04:44 ty N][427c][tkl_bluetooth.c:208] BLE_GAP_EVENT_CONN_UPDATE,0x6,0x0,0x1f4
[01-01 00:04:45 ty N][427c][tkl_bluetooth.c:208] BLE_GAP_EVENT_CONN_UPDATE,0x24,0x0,0x1f4
[01-01 00:04:45 ty E][427c][tuya_ble_data_handler.c:736] ble recv data len err:39097
[01-01 00:04:45 ty E][427c][tuya_ble_data_handler.c:736] ble recv data len err:3688
[01-01 00:04:46 ty N][427c][tkl_bluetooth.c:189] BLE_GAP_EVENT_DISCONNECT(0x213)

[01-01 00:04:46 ty N][427c][tuya_ble_svc.c:776] Ble Disonnected
[01-01 00:04:46 ty N][427c][tuya_ble_data_handler.c:69] ble reset sn
[01-01 00:04:46 ty N][427c][tuya_ble_data_handler.c:83] ble clear pair rand
[01-01 00:04:46 ty N][427c][tuya_ble_svc.c:1685] ble set conn stat:0
[01-01 00:04:46 ty N][41e6][ble_gap.c:2509] Start Adv
[01-01 00:04:46 ty N][41e6][tuya_ble_svc.c:1386] ble adv updated
temp_code:34 - adc_code:214 - adc_trend:[14]:224->[15]:214
init_xtal:86, delta:-2, last_xtal:85
temp_code:29 - adc_code:225 - adc_trend:[15]:214->[14]:224
init_xtal:86, delta:-1, last_xtal:84
temp_code:34 - adc_code:214 - adc_trend:[14]:224->[15]:214
init_xtal:86, delta:-2, last_xtal:85
temp_code:29 - adc_code:225 - adc_trend:[15]:214->[14]:224
init_xtal:86, delta:-1, last_xtal:84
[01-01 00:05:36 ty N][427c][tuya_ble_svc.c:747] Ble Connected
[01-01 00:05:36 ty N][427c][tuya_ble_data_handler.c:69] ble reset sn
[01-01 00:05:36 ty N][427c][tuya_ble_svc.c:1685] ble set conn stat:1
[01-01 00:05:37 ty N][427c][tkl_bluetooth.c:208] BLE_GAP_EVENT_CONN_UPDATE,0x6,0x0,0x1f4
[01-01 00:05:37 ty N][427c][tkl_bluetooth.c:208] BLE_GAP_EVENT_CONN_UPDATE,0x24,0x0,0x1f4
[01-01 00:05:38 ty E][427c][tuya_ble_data_handler.c:736] ble recv data len err:30066
[01-01 00:05:38 ty E][427c][tuya_ble_data_handler.c:736] ble recv data len err:25112
[01-01 00:05:39 ty N][427c][tkl_bluetooth.c:189] BLE_GAP_EVENT_DISCONNECT(0x213)

[01-01 00:05:39 ty N][427c][tuya_ble_svc.c:776] Ble Disonnected
[01-01 00:05:39 ty N][427c][tuya_ble_data_handler.c:69] ble reset sn
[01-01 00:05:39 ty N][427c][tuya_ble_data_handler.c:83] ble clear pair rand
[01-01 00:05:39 ty N][427c][tuya_ble_svc.c:1685] ble set conn stat:0
[01-01 00:05:39 ty N][41e6][ble_gap.c:2509] Start Adv
[01-01 00:05:39 ty N][41e6][tuya_ble_svc.c:1386] ble adv updated
[01-01 00:06:05 ty N][427c][tuya_ble_svc.c:747] Ble Connected
[01-01 00:06:05 ty N][427c][tuya_ble_data_handler.c:69] ble reset sn
[01-01 00:06:05 ty N][427c][tuya_ble_svc.c:1685] ble set conn stat:1
[01-01 00:06:06 ty N][427c][tkl_bluetooth.c:208] BLE_GAP_EVENT_CONN_UPDATE,0x6,0x0,0x1f4
[01-01 00:06:06 ty N][427c][tkl_bluetooth.c:208] BLE_GAP_EVENT_CONN_UPDATE,0x24,0x0,0x1f4
[01-01 00:06:07 ty N][427c][tuya_ble_data_handler.c:624] tuya_ble_handle_dev_infor_req state:0, pkg_len:243
[01-01 00:06:07 ty N][427c][tuya_ble_svc.c:1685] ble set conn stat:3
[01-01 00:06:07 ty N][427c][tuya_ble_data_handler.c:394] PAIR_REQ ok:0
[01-01 00:06:07 ty N][427c][tuya_ble_data_handler.c:372] app_flag len:1, val:1
[01-01 00:06:07 ty N][427c][tuya_ble_data_handler.c:349] ble_time_req start...
rw_ieee80211_set_country code:
code: US
channel: 1 - 11
mode: MANUAL
bk_wlan cca closed
[01-01 00:06:08 ty N][41d4][tuya_devos_utils.c:1554] set psk30 cfg 1
sending broadcast_deauth:5
uap_ip_down
net_wlan_remove_netif done!, vif_idx:0
sending broadcast_deauth:5
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
[msg]rw_msg_send_apm_stop_req
[msg]wait APM_STOP_CFM
me_set_ps_disable:943 0 0 0 0 0 0
set_active param 0
[msg]APM_STOP_CFM
wpa/host apd remove_if:0
sending broadcast_deauth:5
net_wlan_remove_netif vif_idx invalid
[01-01 00:06:08 ty N][41d4][tuya_wifi_status.c:167] cur stat:5  0x5edc1 -->>
[01-01 00:06:08 ty N][41d4][tuya_wifi_status.c:169] wifi netstat changed to:5  -->>
[01-01 00:06:08 ty N][41d4][tuya_wifi_status.c:173] report wifi netstat[5] to callback  -->>
[01-01 00:06:08 ty N][41d4][ty_app_elec_trigger_network.c:83] --->net state change to:5
[01-01 00:06:08 ty N][41e6][tuya_wifi_netcfg.c:206] stop smt cfg mthd:3, ret:0
[01-01 00:06:08 ty N][41a8][app_elec_led.c:323] indicate_mode:0
[01-01 00:06:08 ty E][41e6][tuya_wifi_netcfg.c:201] all netcfg stop fail:-1
[01-01 00:06:08 ty N][41e6][tuya_wifi_netcfg.c:206] stop smt cfg mthd:3, ret:0
tkl_wifi_station_connect !!!
>>> mhdr_set_station_status_cb
>>> _wifi_station_status_cb 0
>>> mhdr_set_station_status 0
>>> _wifi_station_status_cb 0
[sa_sta]MM_RESET_REQ
[bk]tx_txdesc_flush
[sa_sta]ME_CONFIG_REQ
[sa_sta]ME_CHAN_CONFIG_REQ
[sa_sta]MM_START_REQ
PSKC: ssid iotlab, passphrase labiot123sizeof(wpa_sup
plicant)=928
hapd_intf_add_vif,type:2, s:0, id:0
wpa_dInit
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
hapd_intf_ioctl:939
net_wlan_add_netif add netif vif_idx:0
tkl_ethernetif_init
mac  0:33:7a:5a:ee:7e
net_wlan_add_netif done!, vif_idx:0
wpa_supplicant_req_scan
Setting scan[retry1] request: 0.000000 sec
wpa_supplicant_scan
wpa_drv_scan
wpa_send_s[01-01 00:06:08 ty N][41e6][tuya_wifi_status.cc6:08 ty N][41e6][tuya_:167] cur stat:5  0x5edc1wif -->>
i_status.c:167] cur stat:5  0x5edc1 -->>
>>> mhdr_set_station_status 1
>>> _wifi_station_status_cb 1
ht in scan
scan_start_req_handler
wpa_driver_scan_start_cb
[01-01 00:06:08 ty E][41b9][tuya_ws_db.c:319] kvs_read fails rmt_ctrl_0 -6
[01-01 00:06:08 ty E][41b9][tuya_ws_db.c:319] kvs_read fails rmt_ctrl_1 -6
[01-01 00:06:08 ty E][41b9][tuya_ws_db.c:319] kvs_read fails rmt_ctrl_2 -6
[01-01 00:06:08 ty E][41b9][tuya_ws_db.c:319] kvs_read fails rmt_ctrl_3 -6
[01-01 00:06:08 ty E][41b9][tuya_ws_db.c:319] kvs_read fails rmt_ctrl_4 -6
[01-01 00:06:08 ty E][41b9][ap_netcfg.c:1232] ap_broadcast_data_make fail
scanu_confirm: status 0, upload_cnt 5, recv_cnt 43, time 1126350us, result 1
wpa_driver_scan_cb
Scan completed in 1.124000 seconds
>>> mhdr_set_station_status 2
>>> _wifi_station_status_cb 2
wpa_get_scan_rst:1
cipher2security 2 2 16 16
PSKC: end
cipher2security 2 2 16 16
wpa_supplicant_connect
Cancelling scan request
wpa_driver_associate: auth_alg 0x1
>>> mhdr_set_station_status 3
>>> _wifi_station_status_cb 3
found scan rst rssi -21 > -50
bssid 9c:ef:d5:fc:9f:ac, cap_info 0x411, BI 100, ssid iotlab
sm_auth_send:1
sm_auth_handler
ht in assoc req
sm_assoc_rsp_handler
rc_init: station_id=0 format_mod=2 pre_type=0 short_gi=0 max_bw=0
rc_init: nss_max=0 mcs_max=7 r_idx_min=0 r_idx_max=3 no_samples=10
---------SM_CONNECT_IND_ok
Cancelling scan request
new dtim period:2

new ie: 0 : 69 6f 74 6c 61 62 
new ie: 1 : 82 84 8b 96 c 12 18 24 
new ie: 3 : b 
new ie: 30 : 1 0 0 f ac 4 1 0 0 f ac 4 1 0 0 f ac 2 c 0 
new ie: 2d : c 0 13 ff ff 0 0 1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 

WPA: TK 0b2ed602e17beef1d04cd86d2295d8e8
hapd_intf_add_key CCMP
add sta_mgmt_get_sta
sta:0, vif:0, key:0
hapd_intf_add_key:322
sta_mgmt_add_key
add hw key idx:24
WPA: GTK 14d670654f518d6d23bc97d529988b20
hapd_intf_add_key CCMP
add is_broadcast_ether_addr
sta:255, vif:0, key:1
add hw key idx:1
ctrl_port_hdl:1
me_set_ps_disable:943 0 0 0 0 3 0
>>> mhdr_set_station_status 10
>>> _wifi_station_status_cb 10
WLAN_EVENT_CONNECTED
sta_ip_start

configuring mlan(with DHCPc)writed fci to flash ssid=iotlab
ip_addr: 3ac8a8c0
>>> mhdr_set_station_status 11
>>> _wifi_station_status_cb 11
WFE_CONNECTED 11
[01-01 00:06:13 ty N][41e6][tal_wifi_reconnet.c:246] wifi status changed to 0, stat: 1
[01-01 00:06:13 ty E][41e6][tuya_svc_mqtt_client.c:1485] handle null
[01-01 00:06:13 ty N][41e6][tuya_wifi_status.c:167] cur stat:6  0x5edc1 -->>
[01-01 00:06:13 ty N][41e6][tuya_wifi_status.c:169] wifi netstat changed to:6  -->>
[01-01 00:06:13 ty N][41e6][tuya_wifi_status.c:173] report wifi netstat[6] to callback  -->>
[01-01 00:06:13 ty N][41e6][ty_app_elec_trigger_network.c:83] --->net state change to:6
tkl_wifi[01-01 00:06:13 _ty N][:06:13 ty N41a8][app][4_elec_led.c:323] indica1a8][app_elec_led.c:323te_mode:1
] indicate_mode:1
[01-01 00:06:13 ty E][41e6][tuya_wifi_connect.c:75] ap_info_v2 read fail:-6
[01-01 00:06:13 ty E][41e6][tuya_wifi_connect.c:120] ws_db_connect_ap_info_read fail:-6
[01-14 06:20:51 ty N][41d4][tuya_svc_online_log.c:253] timeout,delete uf local log
[01-14 06:20:51 ty N][41d4][tuya_ble_svc.c:543] upd login key, bound stat:1
[01-14 06:20:51 ty N][41e6][tuya_ble_svc.c:1386] ble adv updated
[01-14 06:20:51 ty N][41d4][tuya_ble_svc.c:543] upd login key, bound stat:1
[01-14 06:20:51 ty E][41d4][base_event.c:153] ret:-2
[01-14 06:20:51 ty N][41e6][tuya_ble_svc.c:1386] ble adv updated
[01-14 06:20:51 ty N][41e6][tuya_svc_devos.c:68] already bind
[01-14 11:50:51 ty N][41e6][tuya_svc_mqtt_client.c:179] [mqtts://m3-us.lifeaiot.com:8883] mqtt state change 0 -> 1
[01-14 11:50:51 ty N][41e6][tuya_svc_devos_daemons.c:306] Main Module: v1.0.0 (1.1.9)
[01-14 11:50:51 ty E][41e6][astro_timer.c:326] astro timer read fail:-6
[01-14 11:50:51 ty E][41e6][astro_timer.c:861] read fail:-6
[01-14 11:50:51 ty N][42de][tuya_svc_mqtt_client.c:934] connect to mqtt broker mqtts://m3-us.lifeaiot.com:8883 port 8883
[01-14 11:50:51 ty N][42de][tuya_svc_mqtt_client.c:946] mqtt client ip:192.168.200.58, link-tp:2
[01-14 11:50:53 ty N][42de][tuya_svc_mqtt_client.c:981] setup mqtt transporter success
[01-14 11:50:53 ty N][42de][tuya_svc_mqtt_client.c:179] [mqtts://m3-us.lifeaiot.com:8883] mqtt state change 1 -> 2
[01-14 11:50:53 ty N][42de][tuya_svc_mqtt_client.c:1007] send mqtt connect success
[01-14 11:50:53 ty N][42de][tuya_svc_mqtt_client.c:179] [mqtts://m3-us.lifeaiot.com:8883] mqtt state change 2 -> 3
[01-14 11:50:54 ty N][42de][tuya_svc_mqtt_client.c:1037] mqtt connect success
[01-14 11:50:54 ty N][42de][tuya_svc_mqtt_client.c:179] [mqtts://m3-us.lifeaiot.com:8883] mqtt state change 3 -> 4
[01-14 11:50:54 ty N][42de][tuya_svc_mqtt_client.c:779] mqtt topics cnt 1
[01-14 11:50:54 ty N][42de][tuya_svc_mqtt_client.c:793] send mqtt subscribe success
[01-14 11:50:54 ty N][42de][tuya_svc_mqtt_client.c:179] [mqtts://m3-us.lifeaiot.com:8883] mqtt state change 4 -> 5
[01-14 11:50:55 ty N][41d4][tuya_ble_svc.c:1520] bt rpt conn stat:9
[01-14 11:50:55 ty N][41d4][ty_app_main.c:54] Main Module: v1.0.0 (1.1.9)
[01-14 11:50:56 ty N][42de][tuya_svc_mqtt_client.c:1078] mqtt subscribe success
[01-14 11:50:56 ty N][42de][tuya_svc_mqtt_client.c:179] [mqtts://m3-us.lifeaiot.com:8883] mqtt state change 5 -> 6
[01-14 11:50:56 ty N][41e6][tuya_wifi_status.c:167] cur stat:7  0x5edc1 -->>
[01-14 11:50:56 ty N][41e6][tuya_wifi_status.c:169] wifi netstat changed to:7  -->>
[01-14 11:50:56 ty N][41e6][tuya_wifi_status.c:173] report wifi netstat[7] to callback  -->>
[01-14 11:50:56 ty N][41e6][ty_app_elec_trigger_network.c:83] --->net state change to:7
[01-14 11:50:56 ty E][41a8][smart_frame.c:625] devid:NULL dparr[1]:29 not find
[01-14 11:50:56 ty E][41a8][smart_frame.c:625] devid:NULL dparr[0]:15 not find
[01-14 11:50:56 ty E][41a8][app_elec_delay_off_timer.c:369] ret:-1
[01-14 11:50:56 ty E][41a8][smart_frame.c:625] devid:NULL dparr[0]:50 not find
[01-14 11:50:58 ty N][427c][tkl_bluetooth.c:189] BLE_GAP_EVENT_DISCONNECT(0x216)

[01-14 11:50:58 ty N][427c][tuya_ble_svc.c:776] Ble Disonnected
[01-14 11:50:58 ty N][427c][tuya_ble_data_handler.c:69] ble reset sn
[01-14 11:50:58 ty N][427c][tuya_ble_data_handler.c:83] ble clear pair rand
[01-14 11:50:58 ty N][427c][tuya_ble_svc.c:1685] ble set conn stat:2
[01-14 11:51:06 ty E][41e6][svc_online_log_db.c:25] ret:-6
get rssi: -17
get rssi: -17
get rssi: -17
get rssi: -17
get rssi: -16
[01-14 11:51:10 ty E][41e6][tuya_ble_svc.c:1340] bt hal send fail.-28689
[01-14 11:51:12 ty E][41d4][uf_flash_file_app.c:338] uf_open netcfg_log err 8
[01-14 11:51:13 ty E][41d4][tuya_svc_upgrade.c:917] result null
[01-14 11:51:21 ty N][41b9][tfm_timing_storage.c:168] storage sucess

```
</details>

### 2. Dump the Firmware
```bash
bk7231tools read_flash -d /dev/ttyUSB0 -s 0 -l 0x200000 dump.bin
```
<details> 
  
  <summary>Click to expand terminal output</summary>
  
```text
Unknown bootloader CRC - 0xF42F8C32 - please report this on GitHub issues!
Reading 4k page at 0x011000 (0.00%)
BK72xx connected - protocol: FULL, chip: BK7238, bootloader: None, chip ID: 0x7238, boot version: None
Connected! Chip info: BK7238 / Flash ID: 85 20 15 / Flash size: 0x200000 / Protocol: FULL
Reading 2097152 bytes from 0x0
Reading 4k page at 0x000000 (0.00%)
Reading 4k page at 0x001000 (0.20%)
Reading 4k page at 0x002000 (0.39%)
Reading 4k page at 0x003000 (0.59%)
Reading 4k page at 0x004000 (0.78%)
Reading 4k page at 0x005000 (0.98%)
Reading 4k page at 0x006000 (1.17%)
Reading 4k page at 0x007000 (1.37%)
Reading 4k page at 0x008000 (1.56%)
Reading 4k page at 0x009000 (1.76%)
Reading 4k page at 0x00A000 (1.95%)
Reading 4k page at 0x00B000 (2.15%)
Reading 4k page at 0x00C000 (2.34%)
Reading 4k page at 0x00D000 (2.54%)
Reading 4k page at 0x00E000 (2.73%)
Reading 4k page at 0x00F000 (2.93%)
Reading 4k page at 0x010000 (3.12%)
Reading 4k page at 0x011000 (3.32%)
Reading 4k page at 0x012000 (3.52%)
Reading 4k page at 0x013000 (3.71%)
Reading 4k page at 0x014000 (3.91%)
Reading 4k page at 0x015000 (4.10%)
Reading 4k page at 0x016000 (4.30%)
Reading 4k page at 0x017000 (4.49%)
Reading 4k page at 0x018000 (4.69%)
Reading 4k page at 0x019000 (4.88%)
Reading 4k page at 0x01A000 (5.08%)
Reading 4k page at 0x01B000 (5.27%)
Reading 4k page at 0x01C000 (5.47%)
Reading 4k page at 0x01D000 (5.66%)
Reading 4k page at 0x01E000 (5.86%)
Reading 4k page at 0x01F000 (6.05%)
Reading 4k page at 0x020000 (6.25%)
Reading 4k page at 0x021000 (6.45%)
Reading 4k page at 0x022000 (6.64%)
Reading 4k page at 0x023000 (6.84%)
Reading 4k page at 0x024000 (7.03%)
Reading 4k page at 0x025000 (7.23%)
Reading 4k page at 0x026000 (7.42%)
Reading 4k page at 0x027000 (7.62%)
Reading 4k page at 0x028000 (7.81%)
Reading 4k page at 0x029000 (8.01%)
Reading 4k page at 0x02A000 (8.20%)
Reading 4k page at 0x02B000 (8.40%)
Reading 4k page at 0x02C000 (8.59%)
Reading 4k page at 0x02D000 (8.79%)
Reading 4k page at 0x02E000 (8.98%)
Reading 4k page at 0x02F000 (9.18%)
Reading 4k page at 0x030000 (9.38%)
Reading 4k page at 0x031000 (9.57%)
Reading 4k page at 0x032000 (9.77%)
Reading 4k page at 0x033000 (9.96%)
Reading 4k page at 0x034000 (10.16%)
Reading 4k page at 0x035000 (10.35%)
Reading 4k page at 0x036000 (10.55%)
Reading 4k page at 0x037000 (10.74%)
Reading 4k page at 0x038000 (10.94%)
Reading 4k page at 0x039000 (11.13%)
Reading 4k page at 0x03A000 (11.33%)
Reading 4k page at 0x03B000 (11.52%)
Reading 4k page at 0x03C000 (11.72%)
Reading 4k page at 0x03D000 (11.91%)
Reading 4k page at 0x03E000 (12.11%)
Reading 4k page at 0x03F000 (12.30%)
Reading 4k page at 0x040000 (12.50%)
Reading 4k page at 0x041000 (12.70%)
Reading 4k page at 0x042000 (12.89%)
Reading 4k page at 0x043000 (13.09%)
Reading 4k page at 0x044000 (13.28%)
Reading 4k page at 0x045000 (13.48%)
Reading 4k page at 0x046000 (13.67%)
Reading 4k page at 0x047000 (13.87%)
Reading 4k page at 0x048000 (14.06%)
Reading 4k page at 0x049000 (14.26%)
Reading 4k page at 0x04A000 (14.45%)
Reading 4k page at 0x04B000 (14.65%)
Reading 4k page at 0x04C000 (14.84%)
Reading 4k page at 0x04D000 (15.04%)
Reading 4k page at 0x04E000 (15.23%)
Reading 4k page at 0x04F000 (15.43%)
Reading 4k page at 0x050000 (15.62%)
Reading 4k page at 0x051000 (15.82%)
Reading 4k page at 0x052000 (16.02%)
Reading 4k page at 0x053000 (16.21%)
Reading 4k page at 0x054000 (16.41%)
Reading 4k page at 0x055000 (16.60%)
Reading 4k page at 0x056000 (16.80%)
Reading 4k page at 0x057000 (16.99%)
Reading 4k page at 0x058000 (17.19%)
Reading 4k page at 0x059000 (17.38%)
Reading 4k page at 0x05A000 (17.58%)
Reading 4k page at 0x05B000 (17.77%)
Reading 4k page at 0x05C000 (17.97%)
Reading 4k page at 0x05D000 (18.16%)
Reading 4k page at 0x05E000 (18.36%)
Reading 4k page at 0x05F000 (18.55%)
Reading 4k page at 0x060000 (18.75%)
Reading 4k page at 0x061000 (18.95%)
Reading 4k page at 0x062000 (19.14%)
Reading 4k page at 0x063000 (19.34%)
Reading 4k page at 0x064000 (19.53%)
Reading 4k page at 0x065000 (19.73%)
Reading 4k page at 0x066000 (19.92%)
Reading 4k page at 0x067000 (20.12%)
Reading 4k page at 0x068000 (20.31%)
Reading 4k page at 0x069000 (20.51%)
Reading 4k page at 0x06A000 (20.70%)
Reading 4k page at 0x06B000 (20.90%)
Reading 4k page at 0x06C000 (21.09%)
Reading 4k page at 0x06D000 (21.29%)
Reading 4k page at 0x06E000 (21.48%)
Reading 4k page at 0x06F000 (21.68%)
Reading 4k page at 0x070000 (21.88%)
Reading 4k page at 0x071000 (22.07%)
Reading 4k page at 0x072000 (22.27%)
Reading 4k page at 0x073000 (22.46%)
Reading 4k page at 0x074000 (22.66%)
Reading 4k page at 0x075000 (22.85%)
Reading 4k page at 0x076000 (23.05%)
Reading 4k page at 0x077000 (23.24%)
Reading 4k page at 0x078000 (23.44%)
Reading 4k page at 0x079000 (23.63%)
Reading 4k page at 0x07A000 (23.83%)
Reading 4k page at 0x07B000 (24.02%)
Reading 4k page at 0x07C000 (24.22%)
Reading 4k page at 0x07D000 (24.41%)
Reading 4k page at 0x07E000 (24.61%)
Reading 4k page at 0x07F000 (24.80%)
Reading 4k page at 0x080000 (25.00%)
Reading 4k page at 0x081000 (25.20%)
Reading 4k page at 0x082000 (25.39%)
Reading 4k page at 0x083000 (25.59%)
Reading 4k page at 0x084000 (25.78%)
Reading 4k page at 0x085000 (25.98%)
Reading 4k page at 0x086000 (26.17%)
Reading 4k page at 0x087000 (26.37%)
Reading 4k page at 0x088000 (26.56%)
Reading 4k page at 0x089000 (26.76%)
Reading 4k page at 0x08A000 (26.95%)
Reading 4k page at 0x08B000 (27.15%)
Reading 4k page at 0x08C000 (27.34%)
Reading 4k page at 0x08D000 (27.54%)
Reading 4k page at 0x08E000 (27.73%)
Reading 4k page at 0x08F000 (27.93%)
Reading 4k page at 0x090000 (28.12%)
Reading 4k page at 0x091000 (28.32%)
Reading 4k page at 0x092000 (28.52%)
Reading 4k page at 0x093000 (28.71%)
Reading 4k page at 0x094000 (28.91%)
Reading 4k page at 0x095000 (29.10%)
Reading 4k page at 0x096000 (29.30%)
Reading 4k page at 0x097000 (29.49%)
Reading 4k page at 0x098000 (29.69%)
Reading 4k page at 0x099000 (29.88%)
Reading 4k page at 0x09A000 (30.08%)
Reading 4k page at 0x09B000 (30.27%)
Reading 4k page at 0x09C000 (30.47%)
Reading 4k page at 0x09D000 (30.66%)
Reading 4k page at 0x09E000 (30.86%)
Reading 4k page at 0x09F000 (31.05%)
Reading 4k page at 0x0A0000 (31.25%)
Reading 4k page at 0x0A1000 (31.45%)
Reading 4k page at 0x0A2000 (31.64%)
Reading 4k page at 0x0A3000 (31.84%)
Reading 4k page at 0x0A4000 (32.03%)
Reading 4k page at 0x0A5000 (32.23%)
Reading 4k page at 0x0A6000 (32.42%)
Reading 4k page at 0x0A7000 (32.62%)
Reading 4k page at 0x0A8000 (32.81%)
Reading 4k page at 0x0A9000 (33.01%)
Reading 4k page at 0x0AA000 (33.20%)
Reading 4k page at 0x0AB000 (33.40%)
Reading 4k page at 0x0AC000 (33.59%)
Reading 4k page at 0x0AD000 (33.79%)
Reading 4k page at 0x0AE000 (33.98%)
Reading 4k page at 0x0AF000 (34.18%)
Reading 4k page at 0x0B0000 (34.38%)
Reading 4k page at 0x0B1000 (34.57%)
Reading 4k page at 0x0B2000 (34.77%)
Reading 4k page at 0x0B3000 (34.96%)
Reading 4k page at 0x0B4000 (35.16%)
Reading 4k page at 0x0B5000 (35.35%)
Reading 4k page at 0x0B6000 (35.55%)
Reading 4k page at 0x0B7000 (35.74%)
Reading 4k page at 0x0B8000 (35.94%)
Reading 4k page at 0x0B9000 (36.13%)
Reading 4k page at 0x0BA000 (36.33%)
Reading 4k page at 0x0BB000 (36.52%)
Reading 4k page at 0x0BC000 (36.72%)
Reading 4k page at 0x0BD000 (36.91%)
Reading 4k page at 0x0BE000 (37.11%)
Reading 4k page at 0x0BF000 (37.30%)
Reading 4k page at 0x0C0000 (37.50%)
Reading 4k page at 0x0C1000 (37.70%)
Reading 4k page at 0x0C2000 (37.89%)
Reading 4k page at 0x0C3000 (38.09%)
Reading 4k page at 0x0C4000 (38.28%)
Reading 4k page at 0x0C5000 (38.48%)
Reading 4k page at 0x0C6000 (38.67%)
Reading 4k page at 0x0C7000 (38.87%)
Reading 4k page at 0x0C8000 (39.06%)
Reading 4k page at 0x0C9000 (39.26%)
Reading 4k page at 0x0CA000 (39.45%)
Reading 4k page at 0x0CB000 (39.65%)
Reading 4k page at 0x0CC000 (39.84%)
Reading 4k page at 0x0CD000 (40.04%)
Reading 4k page at 0x0CE000 (40.23%)
Reading 4k page at 0x0CF000 (40.43%)
Reading 4k page at 0x0D0000 (40.62%)
Reading 4k page at 0x0D1000 (40.82%)
Reading 4k page at 0x0D2000 (41.02%)
Reading 4k page at 0x0D3000 (41.21%)
Reading 4k page at 0x0D4000 (41.41%)
Reading 4k page at 0x0D5000 (41.60%)
Reading 4k page at 0x0D6000 (41.80%)
Reading 4k page at 0x0D7000 (41.99%)
Reading 4k page at 0x0D8000 (42.19%)
Reading 4k page at 0x0D9000 (42.38%)
Reading 4k page at 0x0DA000 (42.58%)
Reading 4k page at 0x0DB000 (42.77%)
Reading 4k page at 0x0DC000 (42.97%)
Reading 4k page at 0x0DD000 (43.16%)
Reading 4k page at 0x0DE000 (43.36%)
Reading 4k page at 0x0DF000 (43.55%)
Reading 4k page at 0x0E0000 (43.75%)
Reading 4k page at 0x0E1000 (43.95%)
Reading 4k page at 0x0E2000 (44.14%)
Reading 4k page at 0x0E3000 (44.34%)
Reading 4k page at 0x0E4000 (44.53%)
Reading 4k page at 0x0E5000 (44.73%)
Reading 4k page at 0x0E6000 (44.92%)
Reading 4k page at 0x0E7000 (45.12%)
Reading 4k page at 0x0E8000 (45.31%)
Reading 4k page at 0x0E9000 (45.51%)
Reading 4k page at 0x0EA000 (45.70%)
Reading 4k page at 0x0EB000 (45.90%)
Reading 4k page at 0x0EC000 (46.09%)
Reading 4k page at 0x0ED000 (46.29%)
Reading 4k page at 0x0EE000 (46.48%)
Reading 4k page at 0x0EF000 (46.68%)
Reading 4k page at 0x0F0000 (46.88%)
Reading 4k page at 0x0F1000 (47.07%)
Reading 4k page at 0x0F2000 (47.27%)
Reading 4k page at 0x0F3000 (47.46%)
Reading 4k page at 0x0F4000 (47.66%)
Reading 4k page at 0x0F5000 (47.85%)
Reading 4k page at 0x0F6000 (48.05%)
Reading 4k page at 0x0F7000 (48.24%)
Reading 4k page at 0x0F8000 (48.44%)
Reading 4k page at 0x0F9000 (48.63%)
Reading 4k page at 0x0FA000 (48.83%)
Reading 4k page at 0x0FB000 (49.02%)
Reading 4k page at 0x0FC000 (49.22%)
Reading 4k page at 0x0FD000 (49.41%)
Reading 4k page at 0x0FE000 (49.61%)
Reading 4k page at 0x0FF000 (49.80%)
Reading 4k page at 0x100000 (50.00%)
Reading 4k page at 0x101000 (50.20%)
Reading 4k page at 0x102000 (50.39%)
Reading 4k page at 0x103000 (50.59%)
Reading 4k page at 0x104000 (50.78%)
Reading 4k page at 0x105000 (50.98%)
Reading 4k page at 0x106000 (51.17%)
Reading 4k page at 0x107000 (51.37%)
Reading 4k page at 0x108000 (51.56%)
Reading 4k page at 0x109000 (51.76%)
Reading 4k page at 0x10A000 (51.95%)
Reading 4k page at 0x10B000 (52.15%)
Reading 4k page at 0x10C000 (52.34%)
Reading 4k page at 0x10D000 (52.54%)
Reading 4k page at 0x10E000 (52.73%)
Reading 4k page at 0x10F000 (52.93%)
Reading 4k page at 0x110000 (53.12%)
Reading 4k page at 0x111000 (53.32%)
Reading 4k page at 0x112000 (53.52%)
Reading 4k page at 0x113000 (53.71%)
Reading 4k page at 0x114000 (53.91%)
Reading 4k page at 0x115000 (54.10%)
Reading 4k page at 0x116000 (54.30%)
Reading 4k page at 0x117000 (54.49%)
Reading 4k page at 0x118000 (54.69%)
Reading 4k page at 0x119000 (54.88%)
Reading 4k page at 0x11A000 (55.08%)
Reading 4k page at 0x11B000 (55.27%)
Reading 4k page at 0x11C000 (55.47%)
Reading 4k page at 0x11D000 (55.66%)
Reading 4k page at 0x11E000 (55.86%)
Reading 4k page at 0x11F000 (56.05%)
Reading 4k page at 0x120000 (56.25%)
Reading 4k page at 0x121000 (56.45%)
Reading 4k page at 0x122000 (56.64%)
Reading 4k page at 0x123000 (56.84%)
Reading 4k page at 0x124000 (57.03%)
Reading 4k page at 0x125000 (57.23%)
Reading 4k page at 0x126000 (57.42%)
Reading 4k page at 0x127000 (57.62%)
Reading 4k page at 0x128000 (57.81%)
Reading 4k page at 0x129000 (58.01%)
Reading 4k page at 0x12A000 (58.20%)
Reading 4k page at 0x12B000 (58.40%)
Reading 4k page at 0x12C000 (58.59%)
Reading 4k page at 0x12D000 (58.79%)
Reading 4k page at 0x12E000 (58.98%)
Reading 4k page at 0x12F000 (59.18%)
Reading 4k page at 0x130000 (59.38%)
Reading 4k page at 0x131000 (59.57%)
Reading 4k page at 0x132000 (59.77%)
Reading 4k page at 0x133000 (59.96%)
Reading 4k page at 0x134000 (60.16%)
Reading 4k page at 0x135000 (60.35%)
Reading 4k page at 0x136000 (60.55%)
Reading 4k page at 0x137000 (60.74%)
Reading 4k page at 0x138000 (60.94%)
Reading 4k page at 0x139000 (61.13%)
Reading 4k page at 0x13A000 (61.33%)
Reading 4k page at 0x13B000 (61.52%)
Reading 4k page at 0x13C000 (61.72%)
Reading 4k page at 0x13D000 (61.91%)
Reading 4k page at 0x13E000 (62.11%)
Reading 4k page at 0x13F000 (62.30%)
Reading 4k page at 0x140000 (62.50%)
Reading 4k page at 0x141000 (62.70%)
Reading 4k page at 0x142000 (62.89%)
Reading 4k page at 0x143000 (63.09%)
Reading 4k page at 0x144000 (63.28%)
Reading 4k page at 0x145000 (63.48%)
Reading 4k page at 0x146000 (63.67%)
Reading 4k page at 0x147000 (63.87%)
Reading 4k page at 0x148000 (64.06%)
Reading 4k page at 0x149000 (64.26%)
Reading 4k page at 0x14A000 (64.45%)
Reading 4k page at 0x14B000 (64.65%)
Reading 4k page at 0x14C000 (64.84%)
Reading 4k page at 0x14D000 (65.04%)
Reading 4k page at 0x14E000 (65.23%)
Reading 4k page at 0x14F000 (65.43%)
Reading 4k page at 0x150000 (65.62%)
Reading 4k page at 0x151000 (65.82%)
Reading 4k page at 0x152000 (66.02%)
Reading 4k page at 0x153000 (66.21%)
Reading 4k page at 0x154000 (66.41%)
Reading 4k page at 0x155000 (66.60%)
Reading 4k page at 0x156000 (66.80%)
Reading 4k page at 0x157000 (66.99%)
Reading 4k page at 0x158000 (67.19%)
Reading 4k page at 0x159000 (67.38%)
Reading 4k page at 0x15A000 (67.58%)
Reading 4k page at 0x15B000 (67.77%)
Reading 4k page at 0x15C000 (67.97%)
Reading 4k page at 0x15D000 (68.16%)
Reading 4k page at 0x15E000 (68.36%)
Reading 4k page at 0x15F000 (68.55%)
Reading 4k page at 0x160000 (68.75%)
Reading 4k page at 0x161000 (68.95%)
Reading 4k page at 0x162000 (69.14%)
Reading 4k page at 0x163000 (69.34%)
Reading 4k page at 0x164000 (69.53%)
Reading 4k page at 0x165000 (69.73%)
Reading 4k page at 0x166000 (69.92%)
Reading 4k page at 0x167000 (70.12%)
Reading 4k page at 0x168000 (70.31%)
Reading 4k page at 0x169000 (70.51%)
Reading 4k page at 0x16A000 (70.70%)
Reading 4k page at 0x16B000 (70.90%)
Reading 4k page at 0x16C000 (71.09%)
Reading 4k page at 0x16D000 (71.29%)
Reading 4k page at 0x16E000 (71.48%)
Reading 4k page at 0x16F000 (71.68%)
Reading 4k page at 0x170000 (71.88%)
Reading 4k page at 0x171000 (72.07%)
Reading 4k page at 0x172000 (72.27%)
Reading 4k page at 0x173000 (72.46%)
Reading 4k page at 0x174000 (72.66%)
Reading 4k page at 0x175000 (72.85%)
Reading 4k page at 0x176000 (73.05%)
Reading 4k page at 0x177000 (73.24%)
Reading 4k page at 0x178000 (73.44%)
Reading 4k page at 0x179000 (73.63%)
Reading 4k page at 0x17A000 (73.83%)
Reading 4k page at 0x17B000 (74.02%)
Reading 4k page at 0x17C000 (74.22%)
Reading 4k page at 0x17D000 (74.41%)
Reading 4k page at 0x17E000 (74.61%)
Reading 4k page at 0x17F000 (74.80%)
Reading 4k page at 0x180000 (75.00%)
Reading 4k page at 0x181000 (75.20%)
Reading 4k page at 0x182000 (75.39%)
Reading 4k page at 0x183000 (75.59%)
Reading 4k page at 0x184000 (75.78%)
Reading 4k page at 0x185000 (75.98%)
Reading 4k page at 0x186000 (76.17%)
Reading 4k page at 0x187000 (76.37%)
Reading 4k page at 0x188000 (76.56%)
Reading 4k page at 0x189000 (76.76%)
Reading 4k page at 0x18A000 (76.95%)
Reading 4k page at 0x18B000 (77.15%)
Reading 4k page at 0x18C000 (77.34%)
Reading 4k page at 0x18D000 (77.54%)
Reading 4k page at 0x18E000 (77.73%)
Reading 4k page at 0x18F000 (77.93%)
Reading 4k page at 0x190000 (78.12%)
Reading 4k page at 0x191000 (78.32%)
Reading 4k page at 0x192000 (78.52%)
Reading 4k page at 0x193000 (78.71%)
Reading 4k page at 0x194000 (78.91%)
Reading 4k page at 0x195000 (79.10%)
Reading 4k page at 0x196000 (79.30%)
Reading 4k page at 0x197000 (79.49%)
Reading 4k page at 0x198000 (79.69%)
Reading 4k page at 0x199000 (79.88%)
Reading 4k page at 0x19A000 (80.08%)
Reading 4k page at 0x19B000 (80.27%)
Reading 4k page at 0x19C000 (80.47%)
Reading 4k page at 0x19D000 (80.66%)
Reading 4k page at 0x19E000 (80.86%)
Reading 4k page at 0x19F000 (81.05%)
Reading 4k page at 0x1A0000 (81.25%)
Reading 4k page at 0x1A1000 (81.45%)
Reading 4k page at 0x1A2000 (81.64%)
Reading 4k page at 0x1A3000 (81.84%)
Reading 4k page at 0x1A4000 (82.03%)
Reading 4k page at 0x1A5000 (82.23%)
Reading 4k page at 0x1A6000 (82.42%)
Reading 4k page at 0x1A7000 (82.62%)
Reading 4k page at 0x1A8000 (82.81%)
Reading 4k page at 0x1A9000 (83.01%)
Reading 4k page at 0x1AA000 (83.20%)
Reading 4k page at 0x1AB000 (83.40%)
Reading 4k page at 0x1AC000 (83.59%)
Reading 4k page at 0x1AD000 (83.79%)
Reading 4k page at 0x1AE000 (83.98%)
Reading 4k page at 0x1AF000 (84.18%)
Reading 4k page at 0x1B0000 (84.38%)
Reading 4k page at 0x1B1000 (84.57%)
Reading 4k page at 0x1B2000 (84.77%)
Reading 4k page at 0x1B3000 (84.96%)
Reading 4k page at 0x1B4000 (85.16%)
Reading 4k page at 0x1B5000 (85.35%)
Reading 4k page at 0x1B6000 (85.55%)
Reading 4k page at 0x1B7000 (85.74%)
Reading 4k page at 0x1B8000 (85.94%)
Reading 4k page at 0x1B9000 (86.13%)
Reading 4k page at 0x1BA000 (86.33%)
Reading 4k page at 0x1BB000 (86.52%)
Reading 4k page at 0x1BC000 (86.72%)
Reading 4k page at 0x1BD000 (86.91%)
Reading 4k page at 0x1BE000 (87.11%)
Reading 4k page at 0x1BF000 (87.30%)
Reading 4k page at 0x1C0000 (87.50%)
Reading 4k page at 0x1C1000 (87.70%)
Reading 4k page at 0x1C2000 (87.89%)
Reading 4k page at 0x1C3000 (88.09%)
Reading 4k page at 0x1C4000 (88.28%)
Reading 4k page at 0x1C5000 (88.48%)
Reading 4k page at 0x1C6000 (88.67%)
Reading 4k page at 0x1C7000 (88.87%)
Reading 4k page at 0x1C8000 (89.06%)
Reading 4k page at 0x1C9000 (89.26%)
Reading 4k page at 0x1CA000 (89.45%)
Reading 4k page at 0x1CB000 (89.65%)
Reading 4k page at 0x1CC000 (89.84%)
Reading 4k page at 0x1CD000 (90.04%)
Reading 4k page at 0x1CE000 (90.23%)
Reading 4k page at 0x1CF000 (90.43%)
Reading 4k page at 0x1D0000 (90.62%)
Reading 4k page at 0x1D1000 (90.82%)
Reading 4k page at 0x1D2000 (91.02%)
Reading 4k page at 0x1D3000 (91.21%)
Reading 4k page at 0x1D4000 (91.41%)
Reading 4k page at 0x1D5000 (91.60%)
Reading 4k page at 0x1D6000 (91.80%)
Reading 4k page at 0x1D7000 (91.99%)
Reading 4k page at 0x1D8000 (92.19%)
Reading 4k page at 0x1D9000 (92.38%)
Reading 4k page at 0x1DA000 (92.58%)
Reading 4k page at 0x1DB000 (92.77%)
Reading 4k page at 0x1DC000 (92.97%)
Reading 4k page at 0x1DD000 (93.16%)
Reading 4k page at 0x1DE000 (93.36%)
Reading 4k page at 0x1DF000 (93.55%)
Reading 4k page at 0x1E0000 (93.75%)
Reading 4k page at 0x1E1000 (93.95%)
Reading 4k page at 0x1E2000 (94.14%)
Reading 4k page at 0x1E3000 (94.34%)
Reading 4k page at 0x1E4000 (94.53%)
Reading 4k page at 0x1E5000 (94.73%)
Reading 4k page at 0x1E6000 (94.92%)
Reading 4k page at 0x1E7000 (95.12%)
Reading 4k page at 0x1E8000 (95.31%)
Reading 4k page at 0x1E9000 (95.51%)
Reading 4k page at 0x1EA000 (95.70%)
Reading 4k page at 0x1EB000 (95.90%)
Reading 4k page at 0x1EC000 (96.09%)
Reading 4k page at 0x1ED000 (96.29%)
Reading 4k page at 0x1EE000 (96.48%)
Reading 4k page at 0x1EF000 (96.68%)
Reading 4k page at 0x1F0000 (96.88%)
Reading 4k page at 0x1F1000 (97.07%)
Reading 4k page at 0x1F2000 (97.27%)
Reading 4k page at 0x1F3000 (97.46%)
Reading 4k page at 0x1F4000 (97.66%)
Reading 4k page at 0x1F5000 (97.85%)
Reading 4k page at 0x1F6000 (98.05%)
Reading 4k page at 0x1F7000 (98.24%)
Reading 4k page at 0x1F8000 (98.44%)
Reading 4k page at 0x1F9000 (98.63%)
Reading 4k page at 0x1FA000 (98.83%)
Reading 4k page at 0x1FB000 (99.02%)
Reading 4k page at 0x1FC000 (99.22%)
Reading 4k page at 0x1FD000 (99.41%)
Reading 4k page at 0x1FE000 (99.61%)
Reading 4k page at 0x1FF000 (99.80%)

```
</details>

### 3. Analyse the Dump
```bash
bk7231tools dissect_dump -e -O dump_extract_dir dump.bin
```
```text
Missing bootloader RBL container. Using a scan pattern instead
	0x0: bootloader - [NO RBL, size=0xbac0]
		extracted to dump_extract_dir
Missing app RBL container. Using a scan pattern instead
	0x11000: app - [NO RBL, size=0xf2020]
		extracted to dump_extract_dir
Storage partition:
	0x1f5000: 32 KiB - 11 keys
	- 'e1n7i2f8'
	- 'tls_ca_cnt'
	- 'bt_encrpt_v2'
	- 'em_sys_env'
	- 'ap_info_v2'
	- 'gw_bi'
	- 'user_param_key'
	- 'gw_di'
	- 'ble_beaconkey'
	- 'last_reset_info_new'
	- 'RLY_STAT'
		extracted all keys to dump_extract_dir/dump_storage.json
Storage area `user_param_key`:
	- found! Extracted to dump_extract_dir/dump_user_param_key.json
```

### 4. Check File Type
```bash
file *
```
```text
dump_app_pattern_scan.bin:                  data
dump_app_pattern_scan_decrypted.bin:        data
dump_bootloader_pattern_scan.bin:           data
dump_bootloader_pattern_scan_decrypted.bin: data
dump_storage.json:                          JSON text data
dump_user_param_key.json:                   JSON text data
```

### 5. Analyse "*_storage.json" File
```bash
cat *_storage.json | jq
```
<details> 
  
  <summary>Click to expand terminal output</summary>
  
```text
{
  "e1n7i2f8": [
    {
      "mode": "rw",
      "property": {
        "type": "bool"
      },
      "id": 1,
      "type": "obj"
    },
    {
      "mode": "rw",
      "property": {
        "min": 0,
        "max": 86400,
        "scale": 0,
        "step": 1,
        "type": "value"
      },
      "id": 7,
      "type": "obj"
    },
    {
      "mode": "rw",
      "property": {
        "range": [
          "off",
          "on",
          "memory"
        ],
        "type": "enum"
      },
      "id": 14,
      "type": "obj"
    },
    {
      "mode": "rw",
      "property": {
        "type": "string",
        "maxlen": 255
      },
      "id": 17,
      "type": "obj"
    },
    {
      "mode": "rw",
      "property": {
        "type": "string",
        "maxlen": 255
      },
      "id": 18,
      "type": "obj"
    },
    {
      "mode": "rw",
      "property": {
        "type": "string",
        "maxlen": 255
      },
      "id": 19,
      "type": "obj"
    },
    {
      "mode": "rw",
      "property": {
        "range": [
          "flip",
          "sync",
          "button"
        ],
        "type": "enum"
      },
      "id": 47,
      "type": "obj"
    }
  ],
  "tls_ca_cnt": 0,
  "bt_encrpt_v2": 1,
  "em_sys_env": "T1",
  "ap_info_v2": "HEX:20050000696f746c61620000000000000000000000000000000000000000000000000000009cefd5fc9fac050b65323162623531333966373739333265393965386534386166613664336361663036363733633736653263633936613939396438326264613632383765656465006c6162696f743132330000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000009e09640011040000ebffffff8c000006696f746c6162010882848b960c12182403010b2a010432043048606c30140100000fac040100000fac040100000fac020c002d1a0c0013ffff0000010000000000000001000000000000000000003d160b0004000000000000000000000000000000000000007f080000000200000040dd180050f2020101010003a4000027a4000042435e0062322f00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001967c9fcd915f630d7359ea9877d2c8c3",
  "gw_bi": {
    "uuid": "9d43ce823d6f988f",
    "psk_key": "t7Ac0lyOg55fE1IKAyjIfcSwDTCosNwYujyGx",
    "auth_key": "Ut49cfOMk4cFI9AwuOO2sl4x13Gw2H4v",
    "ap_ssid": "SmartLife",
    "ap_passwd": null,
    "country_code": "CN",
    "bt_mac": null,
    "bt_hid": null,
    "prod_test": false,
    "fac_pin": "qeugymgl8v2hisyv",
    "psk30_key": "GEArA6ejhQIQeDEwroTC7UnNiuZAChAlY0A0b2jCveQ=",
    "pin": null
  },
  "user_param_key": {
    "ble_pair_time": 30,
    "bt1_lv": 1,
    "bt1_pin": 9,
    "ch_cddpid1": 7,
    "ch_dpid1": 1,
    "ch_flag1": 1,
    "ch_num": 1,
    "clean_t": 5,
    "crc": 3,
    "ext_bt_type": 3,
    "ffc_select": 1,
    "jv": "1.2.1",
    "light_status_select": 0,
    "module": "T1-2S-NL",
    "net_trig": 1,
    "netled_lv": 0,
    "netled_pin": 8,
    "netn_led": 1,
    "nety_led": 0,
    "onoff_clear_t": 5,
    "onoff_cnt": 10,
    "onoff_n": 3,
    "onoff_rst_m": 1,
    "pd_category": 1,
    "reset_t": 5,
    "rl1_lv": 1,
    "rl1_pin": 24,
    "total_stat": 2,
    "zero_select": 0
  },
  "gw_di": {
    "abi": 0,
    "id": "eb02ab98abbc359772tyv2",
    "swv": "1.1.9",
    "bv": "40.00",
    "pv": "2.3",
    "lpv": "3.5",
    "pk": "keyfxuj3d5wg53wx",
    "firmk": "keyfxuj3d5wg53wx",
    "cadv": "1.0.5",
    "cdv": "1.0.0",
    "dev_swv": "1.1.9",
    "s_id": "e1n7i2f8",
    "dtp": 0,
    "sync": 0,
    "attr_num": 1,
    "mst_tp_0": 9,
    "mst_ver_0": "1.1.9",
    "mst_md5_0": null,
    "mst_tp_1": 0,
    "mst_ver_1": null,
    "mst_md5_1": null,
    "mst_tp_2": 0,
    "mst_ver_2": null,
    "mst_md5_2": null,
    "mst_tp_3": 0,
    "mst_ver_3": null,
    "mst_md5_3": null,
    "mst_tp_4": 0,
    "mst_ver_4": null,
    "mst_md5_4": null,
    "mst_tp_5": 0,
    "mst_ver_5": null,
    "mst_md5_5": null,
    "mst_tp_6": 0,
    "mst_ver_6": null,
    "mst_md5_6": null,
    "mst_tp_7": 0,
    "mst_ver_7": null,
    "mst_md5_7": null,
    "mst_tp_8": 0,
    "mst_ver_8": null,
    "mst_md5_8": null,
    "mst_tp_9": 0,
    "mst_ver_9": null,
    "mst_md5_9": null
  },
  "ble_beaconkey": "1266F7DDD82F9DAFAAB4FA02FB0566F2",
  "last_reset_info_new": {
    "region": "AZ",
    "type": 2
  },
  "RLY_STAT": ""
}
```
</details>

### 6. Convert HEX to ASCII
```bash
echo "20050000696f746c61620000000000000000000000000000000000000000000000000000009cefd5fc9fac050b65323162623531333966373739333265393965386534386166613664336361663036363733633736653263633936613939396438326264613632383765656465006c6162696f743132330000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000009e09640011040000ebffffff8c000006696f746c6162010882848b960c12182403010b2a010432043048606c30140100000fac040100000fac040100000fac020c002d1a0c0013ffff0000010000000000000001000000000000000000003d160b0004000000000000000000000000000000000000007f080000000200000040dd180050f2020101010003a4000027a4000042435e0062322f00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001967c9fcd915f630d7359ea9877d2c8c3"| \
xxd -r -p | strings
```
```text
iotlab
e21bb5139f77932e99e8e48afa6d3caf06673c76e2cc96a999d82bda6287eede
labiot123
iotlab
0H`l0

```

### 7. Verify SSID and PSK (optional)
```bash
nmcli dev wifi connect "iotlab" password "labiot123"
```
<br>

## Credits
- Researcher: `Mohammad Natiq Khan`
- Organization: `Payatu Security Consulting Pvt. Ltd.`
