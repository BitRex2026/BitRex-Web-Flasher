# فریمور LAN v3 — NerdOCTAXE-Gamma (LGA_Design-Rev-2.2) + آداپتر BitRex (W5500)

پایه: shufps/ESP-Miner-NerdQAxePlus (762092d) — BOARD=NERDOCTAXEGAMMA — ESP-IDF v5.3.3

## پین‌های W5500 (شماتیک LAN-Adapter)
SCLK=GPIO2 · RST=GPIO3 · MOSI=GPIO12 · MISO=GPIO16 · CS=GPIO21 — INTn وصل نیست (polling)

## تغییرات v3 (رفع خطای esp_eth_start: handle null)
- رفع باگ ترتیب اجرا: ساخت netif/glue به بعد از esp_netif_init منتقل شد (دلیل اصلی کارنکردن اتریت)
- غیرفعال‌سازی TMP451 MUX در حالت LAN (GPIO2/12 مشترک با SCLK/MOSI) — دمای ASIC از خود چیپ‌ها + TMP1075 + VR خوانده می‌شود
- به‌علاوه پچ‌های قبلی: پین‌های W5500، اجبار TPS53647، محافظ MUX، تازه‌شدن آیکون شبکه

## فلش
```bash
esptool.py --chip esp32s3 write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m \
  0x0 esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
```

## MD5
```
ed1e180cfcde8a2ba6c33f4e823f18ed  bootloader.bin
e472945950ba069df9e78e766156bb6a  esp-miner.bin
75df3b4f77afa18c34ef2d1fbabf5ed4  esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
84d04c9d6cc8ef35bf825d51a5277699  ota_data_initial.bin
5564bf7cc6773a0af894079b83d28500  partition-table.bin
54bda1b659ecfe7c0c5ada1adfa988ae  www.bin
```
