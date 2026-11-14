# NerdOCTAXE-Gamma LGA rev2.2 + LAN — Fixed Hashrate Overflow Build

Target board: `NERDOCTAXEGAMMA` (NerdOCTAXE-Gamma LGA rev2.2, 4x BM1370)  
Commit base: `762092d` (v1.1.0) with custom W5500 SPI Ethernet pins:
- SCK: GPIO 12
- MOSI: GPIO 21
- MISO: GPIO 16
- CS: GPIO 3
- INT: GPIO 2

## Fixes Applied:
- **`HashrateMonitor::getChipHashrate` (`0x4202cd88`)**: Assembly-level anomaly clamp. Automatically intercepts any chip hashrate reading exceeding `2048.0 GH/s` (e.g. corrupt packets, microsecond delta spikes, or counter overflows such as 44M–50M GH/s) and clamps it to `0.00 GH/s`. Any real hashing chip (e.g. 1.2 TH/s) passes through untouched.
- **`HashrateMonitor::getTotalChipHashrate` (`0x4202cdb0`)**: Restructured assembly loop calling `getChipHashrate` for each ASIC, ensuring total miner hashrate, Prometheus metrics, and the LCD display strictly aggregate valid hashrates without phantom values.
- **`HashrateMonitor::onRegisterReply` (`0x4202ce48`)**: Restored clean register preservation and counter tracking for all ASICs.
- **Log visual identifier**: outputs `hashrate_monitor: chip [PATCHED]: ...` so you can verify immediately in the serial / web log that the patched binary is running.

## Binary Checksums (MD5):
```
dc366c7b1d45572dd666730cb5e19ffe  esp-miner.bin
23e0d751bce31c105d0334819a63ce52  esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
af860c57052434a5c2e4a363d0495fa8  esp-miner-update-NERDOCTAXEGAMMA-LAN.hex
e52478d24c902990d9218b704752677a  bootloader.bin
5564bf7cc6773a0af894079b83d28500  partition-table.bin
84d04c9d6cc8ef35bf825d51a5277699  ota_data_initial.bin
0a30ad94003b56a23de8f0395107649c  www.bin
```
