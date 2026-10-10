# STM32 LED Blink

STM32F103 üzerinde PC13 pinine bağlı LED'i 500 ms aralıklarla açıp kapatan C uygulaması. STM32 HAL ile GPIO yapılandırmasını, dahili saat kullanımını ve temel bir gömülü sistem ana döngüsünü gösterir.

## Nasıl çalışır?

Başlangıçta HAL, sistem saati ve GPIO çevre birimi hazırlanır. Ana döngüde `HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13)` çağrısı çıkış seviyesini değiştirir; ardından `HAL_Delay(500)` ile beklenir. LED'in bir tam açılıp kapanma çevrimi yaklaşık bir saniyedir.

| Yapılandırma | Değer |
| --- | --- |
| Mikrodenetleyici | STM32F103C8, Cortex-M3 |
| LED çıkışı | PC13 |
| GPIO modu | Push-pull çıkış, düşük hız |
| Saat kaynağı | Dahili HSI, PLL kapalı |
| Durum değişim aralığı | 500 ms |

## Gereksinimler

- STM32F103 tabanlı kart ve PC13'e bağlı LED.
- SWD üzerinden bağlanan ST-LINK veya uyumlu programlayıcı.
- `arm-none-eabi-gcc` araç zinciri ve GNU Make.
- Firmware yüklemek için STM32CubeProgrammer veya eşdeğer bir araç.

## Derleme

```bash
git clone https://github.com/muzafferturkoglu/stm32-led-blink.git
cd stm32-led-blink
make
```

Araç zinciri PATH üzerinde değilse `make GCC_PATH=/arac/zinciri/bin` kullanın. Derleme sonucunda `build/Blink.elf`, `build/Blink.hex` ve `build/Blink.bin` oluşur. Firmware'i kartınıza yükleyin; `.bin` dosyası için başlangıç adresi `0x08000000`'dır.

## Proje yapısı

```text
Core/Inc/                   Başlık dosyaları
Core/Src/main.c             LED kontrol döngüsü
Core/Src/stm32f1xx_it.c      Kesme işleyicileri
Drivers/                   HAL ve CMSIS bileşenleri
Blink.ioc                  STM32CubeMX yapılandırması
Makefile                   Derleme ayarları
STM32F103XX_FLASH.ld        Bellek yerleşimi
startup_stm32f103xb.s       Başlangıç kodu
```

`Blink.ioc`, pin ve saat yapılandırmasını incelemek veya düzenlemek için STM32CubeMX ile açılabilir. Yanıp sönme hızını değiştirmek için `Core/Src/main.c` içindeki `HAL_Delay(500)` değerini düzenleyin. Bu sürüm bloklayan gecikme kullanır.

**Geliştirici:** [Muzaffer Türkoğlu](https://github.com/muzafferturkoglu)
