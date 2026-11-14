# NerdOCTAXE-Gamma LGA rev2.2 + LAN — Fixed Hashrate Overflow Build

Target board: `NERDOCTAXEGAMMA` (NerdOCTAXE-Gamma LGA rev2.2, 4x BM1370)  
Commit base: `762092d` (v1.1.0) with custom W5500 SPI Ethernet pins:
- SCK: GPIO 12
- MOSI: GPIO 21
- MISO: GPIO 16
- CS: GPIO 3
- INT: GPIO 2

## Fixes Applied:
- **`HashrateMonitor::onRegisterReply` (`0x4202ce9a`)**: Hardware-grade Sample-and-Hold Outlier Rejection Filter.
  Directly intercepts calculated chip hashrates before updating internal chip storage. Any anomaly exceeding `2048.0 GH/s` (e.g. 45M–50M GH/s spikes caused by microsecond delta replies or counter jumps) is dropped, holding `0.00 GH/s` for phantom chips without mutating stack or caller frames. Valid BM1370 hashrate (e.g. 1.28 TH/s) updates seamlessly every second.
- **100% Stock Original Boot & Display Functions**: `getTotalChipHashrate` and `getChipHashrate` remain untouched stock leaf functions. Eliminates pre-mining reboots and stack window overflows.
- **Log visual identifier**: outputs `hashrate_monitor: chip [PATCHED]: ...` so you can verify immediately in the serial / web log that the patched binary is running.

## Binary Checksums (MD5):
```
e221ae28ff5b3bf68388ee75cd3d1435  esp-miner.bin
bdcf22e3e7128c9a5da2353294433c19  esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
0c4ea2c05938ee9aac7c37c95f6606fd  esp-miner-update-NERDOCTAXEGAMMA-LAN.hex
e52478d24c902990d9218b704752677a  bootloader.bin
5564bf7cc6773a0af894079b83d28500  partition-table.bin
84d04c9d6cc8ef35bf825d51a5277699  ota_data_initial.bin
0a30ad94003b56a23de8f0395107649c  www.bin
```
