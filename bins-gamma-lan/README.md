# فریمور LAN — NerdOCTAXE-Gamma (LGA_Design-Rev-2.2) + آداپتر BitRex (W5500)

فریمور پایه: **shufps/ESP-Miner-NerdQAxePlus** (`762092d`) — فریمور رسمی همین برد طبق README پروژه Patsch91.
تارگت: `BOARD=NERDOCTAXEGAMMA` — ESP-IDF v5.3.3 — ESP32-S3 — فلش 16MB

## پین‌های W5500 (مطابق شماتیک LAN-Adapter)
| سیگنال | GPIO |
|---|---|
| SCLK | 2 |
| RSTn | 3 |
| MOSI | 12 |
| MISO | 16 |
| SCSn | 21 |
| INTn | وصل نیست → polling |

## ۵ پچ نسبت به نسخه اصلی
1. `w5500.h` — پین‌های بالا + حالت polling به‌جای INT
2. `nerdoctaxegamma.h` — فعال‌سازی اتریت (`hasEthernet`)
3. `nerdoctaxegamma.cpp` — اجبار TPS53647 (GPIO3 دیگر VR_DETECT نیست)
4. `tmp451_mux.cpp` — مقداردهی GPIO2/12 فقط بعد از probe (محافظ SCLK/MOSI)
5. `system.cpp` — آیکون شبکه با تغییر رابط WiFi↔ETH هم تازه می‌شود

## فلش
```bash
esptool.py --chip esp32s3 write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m \
  0x0 esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
```
یا با bitaxetool به‌همراه `config.cvs`.

## MD5
```
bee23d7d868138c0c21b23e333725e02  bootloader.bin
afeab8887f499121701ac67f7b9c0371  esp-miner.bin
cddd1d1ef305778130628aa13fa677a9  esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
84d04c9d6cc8ef35bf825d51a5277699  ota_data_initial.bin
5564bf7cc6773a0af894079b83d28500  partition-table.bin
d6fdd1b6de79923928fd7498d9868897  www.bin
```
