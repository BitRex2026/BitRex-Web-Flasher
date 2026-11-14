Current Version: v1.1.0-patched — shufps/ESP-Miner-NerdQAxePlus (762092d) — BOARD=NERDOCTAXEGAMMA + LAN (W5500)

## Patches & Fixes
- **LAN-Adapter (W5500)**: SCLK=GPIO2, RST=GPIO3, MOSI=GPIO12, MISO=GPIO16, CS=GPIO21 (Polling mode).
- **VR Regulator**: Forced TPS53647 (GPIO3 reserved for LAN RST, skipping VR_DETECT).
- **Hashrate Monitor Glitch Fix**: Patched 32-bit counter underflow and anomalous register read spikes (`delta >= 4096` / `counterNow < prevCounter`). Prevents astronomical multi-million GH/s display errors (28M+ GH/s) and protects 1m/10m/1h history moving averages.

## pins
SCLK=GPIO2 · RST=GPIO3 · MOSI=GPIO12 · MISO=GPIO16 · CS=GPIO21 — W5500 (SCLK GND / SCS INT / MOSI RST / MISO GND / 3.3V 5V)

## FLASH
```bash
esptool.py --chip esp32s3 write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m \
  0x0 esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
```

## MD5
```
e52478d24c902990d9218b704752677a  bootloader.bin
f6f17f69030c49edf58989645cdb9977  esp-miner.bin
143bac4f917aa7c2b95a8531bce039c2  esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
b870e3f1774fd487230055e02050ce21  esp-miner-update-NERDOCTAXEGAMMA-LAN.hex
84d04c9d6cc8ef35bf825d51a5277699  ota_data_initial.bin
5564bf7cc6773a0af894079b83d28500  partition-table.bin
0a30ad94003b56a23de8f0395107649c  www.bin
```
