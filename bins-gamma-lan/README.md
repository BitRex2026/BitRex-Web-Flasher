# NerdOCTAXE-Gamma LGA rev2.2 + LAN — Fixed Hashrate Overflow Build

Target board: `NERDOCTAXEGAMMA` (NerdOCTAXE-Gamma LGA rev2.2, 4x BM1370)  
Commit base: `762092d` (v1.1.0) with custom W5500 SPI Ethernet pins:
- SCK: GPIO 12
- MOSI: GPIO 21
- MISO: GPIO 16
- CS: GPIO 3
- INT: GPIO 2

## Fix Applied:
- **Assembly filter in `HashrateMonitor::onRegisterReply` (`0x4202ce48`)**: filters out anomalous / overflow counter delta readings (`counterDelta > 4095`) from Chip 0 / register 0x90.
- **Log visual identifier**: outputs `hashrate_monitor: chip [PATCHED]: ...` so you can verify immediately in the serial / web log that the patched binary is running.

## Binary Checksums (MD5):
```
f4f394f03b12f331380c3aaec41cdd38  esp-miner.bin
0e28358f0eb5e1b0bfec2cbfc80110d6  esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
53a543f3ca26e81f4af31984141da818  esp-miner-update-NERDOCTAXEGAMMA-LAN.hex
e52478d24c902990d9218b704752677a  bootloader.bin
5564bf7cc6773a0af894079b83d28500  partition-table.bin
84d04c9d6cc8ef35bf825d51a5277699  ota_data_initial.bin
0a30ad94003b56a23de8f0395107649c  www.bin
```
