# فریمور LAN — نسخه 1.1.0 (auto-probe) — NerdOCTAXE-Gamma LGA_Design-Rev-2.2 + آداپتر BitRex

Current Version: 1.1.0 — پایه shufps/ESP-Miner-NerdQAxePlus (762092d) — BOARD=NERDOCTAXEGAMMA

## جدید در این بیلد
- **تشخیص خودکار سیم‌کشی**: هنگام بوت ۴ حالت (قطعیت RST × جابه‌جایی MOSI/MISO) امتحان و درست‌ترین انتخاب می‌شود؛ نتیجه هر امتحان در لاگ سریال ثبت می‌شود (`probe[...]: ver=0x..`)
- رفع باگ ترتیب netif، غیرفعال‌سازی TMP451 MUX در حالت LAN، آیکون شبکه

## پین‌ها (طراحی آداپتر — تأیید شد)
SCLK=GPIO2 · RST=GPIO3 · MOSI=GPIO12 · MISO=GPIO16 · CS=GPIO21 — ماژول W5500 با هدر 2×5 (SCLK GND / SCS INT / MOSI RST / MISO GND / 3.3V 5V)، تغذیه 5V

## فلش
```bash
esptool.py --chip esp32s3 write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m \
  0x0 esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
```

## MD5
```
10ccf46b466729a6bf609d951af72c10  bootloader.bin
3bebd2fc06459c1fbc79d02ddbaf7a32  esp-miner.bin
4ac398b35ca19f34d9e5da264d932f91  esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
84d04c9d6cc8ef35bf825d51a5277699  ota_data_initial.bin
5564bf7cc6773a0af894079b83d28500  partition-table.bin
ecfc7424f9e066089c9973645927988a  www.bin
```
