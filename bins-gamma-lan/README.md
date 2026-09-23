# فریمور LAN برای NerdOCTAXE-Gamma (LGA_Design-Rev-2.2) + آداپتر W5500

ساخته‌شده از **shufps/ESP-Miner-NerdQAxePlus** (شاخه `develop`، تاریخ سپتامبر ۲۰۲۶) با ۵ پچ کوچک سازگارسازی با آداپتر LAN-Adapter (BitRex2026).

---

## ۱) نتیجه بررسی سخت‌افزار — آداپتر با برد LGA_Design-Rev-2.2 کار می‌کند؟

**بله — سیم‌کشی درست است.** ✅ آداپتر دقیقاً ۵ پین آزاد برد را می‌گیرد و به بقیه پین‌ها کاری ندارد (عبور مستقیم ۱:۱):

| سیگنال W5500 | پین J1 (کانکتور ماژول) | پد T-Display | GPIO | وضعیت روی برد LGA rev2.2 |
|---|---|---|---|---|
| SCLK | 1 | 3 | GPIO2 | آزاد ✅ |
| GND | 2 | — | — | — |
| SCS (CS) | 3 | 18 | GPIO21 | آزاد ✅ |
| **INT** | **4** | **—** | **—** | **وصل نشده** ⚠️ |
| MOSI | 5 | 7 | GPIO12 | آزاد ✅ |
| RST | 6 | 4 | GPIO3 | آزاد ✅ |
| MISO | 7 | 17 | GPIO16 | آزاد ✅ |
| GND | 8 | — | — | — |
| NC | 9 | — | — | — |
| 5V | 10 | 12 | 5V | — |

پین‌های پر برد (بدون تداخل با آداپتر، عبور سالم):
GPIO1=RESET زنجیره ASIC، GPIO10=PWR_EN، GPIO11=VR_RDY، GPIO13=LDO_EN، GPIO17=TXD، GPIO18=RXD، GPIO44=SDA، GPIO43=SCL

هندسه کانکتور هم با سوکت برد (U30) یکی است: ۲×۱۲ پین، پیچ ۲.۵۴mm، فاصله دو ردیف ۲۲.۹۲mm.

### نکات مهم در طراحی (قبل از سفارش PCB)

1. **پین INT (پایه ۴ J1) معلق است** — در ویرایش آخر شما لیبل `6-INT` به دو لیبل جدا `6` و `4` تغییر کرد و اتصال INT قطع شد. چون روی این برد GPIO آزاد ششمی وجود ندارد، این ایراد نیست و **در فریمور با polling حل شده** — اما اگر خواستید INT وصل کنید، فقط با تغییر برد (پین آزاد) ممکن است. (اتصال آن به GPIO11 یا GPIO13 اشتباه است؛ آن پین‌ها VR_RDY و LDO_EN هستند!)
2. **فایل‌های Gerber داخل ریپو قدیمی هستند** (۲۷ آگوست = قبل از تغییرات امروز شما). حتماً از `ESP32-SPI.kicad_pcb` فعلی دوباره خروجی Gerber بگیرید، وگرنه بردِ تولیدشده همان نسخه قدیمی است که RST روی GPIO13 و INT روی GPIO11 دارد و **با LDO_EN/VR_RDY برد گاما تداخل دارد** ⚠️.
3. ماژول W5500 را با کابل/هدر ۱:۱ به J1 وصل کنید. تغذیه ۵V (پایه ۱۰) از رگولاتور خود ماژول تغذیه می‌شود ✅.
4. **فقط برای LGA_Design-Rev-2.2:** روی ریویژن‌های ۳.۴ (TMP451-mux) پین‌های GPIO2/12 با مولتی‌پلکسر دما مشترک‌اند و این آداپتر با آن بردها سازگار نیست. برد شما (rev2.2) TMP1075 دارد و مشکلی نیست.

---

## ۲) پچ‌های فریمور (۴ تغییر کوچک)

| فایل | تغییر | چرا |
|---|---|---|
| `main/network/w5500.h` | RST: GPIO13→**GPIO3**، حالت INT: **polling** (`W5500_USE_INT 0`) | مطابق سیم‌کشی آداپتر شما؛ پین 13/11 روی این برد LDO_EN/VR_RDY است |
| `main/boards/nerdoctaxegamma.h` | `hasEthernet() → true` | فریمور فقط روی برد‌های با ETH مقداردهی W5500 را انجام می‌دهد |
| `main/boards/nerdoctaxegamma.cpp` | رد کردن VR_DETECT و **اجبار TPS53647** | GPIO3 در فریمور برای تشخیص نوع رگولاتور خوانده می‌شد؛ با ماژول W5500، پول‌آپ RSTn باعث تشخیص اشتباه TPS53667 (پروفایل ۶ فاز/خطرناک) می‌شد. برد LGA rev2.2 همیشه TPS53647 (۴ فاز) دارد |
| `main/boards/drivers/tmp451_mux.cpp` | اول probe سپس پیکربندی GPIO2/12 | مقداردهی بی‌قید GPIO2/12 قبل از probe، سیگنال‌های SCLK/MOSI ماژول W5500 را می‌دزدید |
| `main/system.cpp` | آیکون شبکه با تغییر رابط (WiFi↔ETH) هم تازه می‌شود، نه فقط با تغییر رشته IP | اگر روتر به هر دو رابط یک IP می‌داد (رزرو بر اساس hostname)، آیکون روی وای‌فای گیر می‌کرد |

بقیه چیزها دست‌نخورده: رابط وب AxeOS، پروتکل‌های استراتوم V1/V2، TLS، OTA، CAN و…

---

## ۳) فایل‌های خروجی

| فایل | کاربرد |
|---|---|
| **`esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin`** | ایمیج کامل فلش ۱۶MB — **فلش از آدرس 0x0** (فلش کامل/اولیه) |
| `esp-miner-update-NERDOCTAXEGAMMA-LAN.hex` | آپدیت OTA شامل esp-miner + www + ota_data |
| `esp-miner.bin` | فقط اپلیکیشن (آدرس 0x10000) |
| `www.bin` | رابط وب AxeOS (آدرس 0x410000) |
| `bootloader.bin` / `partition-table.bin` / `ota_data_initial.bin` | پارتیشن‌های جدا (فلش دستی) |

نسخه پایه: ESP-IDF v5.3.3، تارگت `esp32s3`، `BOARD=NERDOCTAXEGAMMA`، خروجی بیلد استاندارد پروژه (factory مثل ریلیزهای رسمی).

## ۴) راهنمای فلش

**روش ۱ — esptool (USB-C خود T-Display-S3):**
```bash
esptool.py --chip esp32s3 -b 460800 --before default_reset --after hard_reset \
  write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m \
  0x0 esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin
```

**روش ۲ — bitaxetool با فایل کانفیگ:**
```bash
bitaxetool --config ./config.cvs --firmware esp-miner-factory-NERDOCTAXEGAMMA-LAN.bin -p /dev/ttyACM0
```
(از `config.cvs.example` کپی بگیرید و stratum/wifi را پر کنید)

**روش ۳ — Webflasher:** اگر فریمور قبلیِ AxeOS روی دستگاه است، از منوی OTA داخل وب‌インターハيس (`/update`) یا webflasher با انتخاب فایل سفارشی استفاده کنید.

**بعد از فلش:** اگر شبکه بی‌سیم تنظیم نشده باشد، دستگاه AP راه می‌اندازد؛ با **کابل LAN وصل‌شده به آداپتر**، DHCP خودکار انجام می‌شود و اگر اتریت IP بگیرد، Wi-Fi خاموش می‌شود (اولویت با LAN است). آدرس دستگاه در لاگ سریال یا mDNS (`hostname.local`) نمایش داده می‌شود.

## ۵) تست پیشنهادی بعد از فلش
1. سریال 115200: دنبال `LAN-Adapter build: VR detect skipped ... forcing TPS53647` و `W5500::init start` بگردید.
2. با وصل بودن کابل: `ETH link UP` و `ETH GOT IP: ...` باید دیده شود.
3. ماینینگ استخر که وصل شد، در صفحه وب آیکون/نشانگر ETH باید فعال باشد.


---
**نسخه v2 (۲۳ سپتامبر ۲۰۲۶):** افزودن پچ آیکون شبکه (`system.cpp`) — مخصوص روترهایی که به WiFi و اتریت یکسان IP می‌دهند.
