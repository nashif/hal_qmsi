if(CONFIG_QMSI)
zephyr_library()

zephyr_compile_definitions(                                  ENABLE_EXTERNAL_ISR_HANDLING)
zephyr_compile_definitions_ifdef(CONFIG_SYS_POWER_DEEP_SLEEP_STATES ENABLE_RESTORE_CONTEXT)

if(CONFIG_SOC_QUARK_SE_C1000_SS)
set(SOC_SERIES quark_se)
endif()

zephyr_include_directories(
  include
  drivers/include
  )
zephyr_include_directories_ifdef(CONFIG_ARC drivers/sensor/include)
zephyr_include_directories(                 soc/${SOC_SERIES}/include)

set_ifndef(SOC_WATCH_ENABLE 0)
if(CONFIG_SOC_WATCH)
  set(SOC_WATCH_ENABLE 1)
  zephyr_compile_definitions(SOC_WATCH_ENABLE)
endif()

zephyr_library_sources(
  drivers/flash/qm_flash.c
  soc/${SOC_SERIES}/drivers/power_states.c
  soc/${SOC_SERIES}/drivers/clk.c
  )
if(CONFIG_SOC_QUARK_SE_C1000_SS)
  zephyr_library_sources(
    drivers/clk/ss_clk.c
    soc/quark_se/drivers/vreg.c
    soc/quark_se/drivers/ss_power_states.c
    )
endif()
if(CONFIG_SOC_QUARK_SE_C1000)
  zephyr_library_sources(soc/${SOC_SERIES}/drivers/vreg.c)
endif()

zephyr_library_sources_ifdef(CONFIG_AIO_COMPARATOR_QMSI  drivers/comparator/qm_comparator.c)
zephyr_library_sources_ifdef(CONFIG_AON_COUNTER_QMSI     drivers/aon_counters/qm_aon_counters.c)
zephyr_library_sources_ifdef(CONFIG_DMA_QMSI             drivers/dma/qm_dma.c)
zephyr_library_sources_ifdef(CONFIG_GPIO_QMSI            drivers/gpio/qm_gpio.c)
zephyr_library_sources_ifdef(CONFIG_GPIO_QMSI_SS         drivers/gpio/qm_ss_gpio.c)
zephyr_library_sources_ifdef(CONFIG_I2C_QMSI             drivers/i2c/qm_i2c.c)
zephyr_library_sources_ifdef(CONFIG_I2C_QMSI_SS          drivers/i2c/qm_ss_i2c.c)
zephyr_library_sources_ifdef(CONFIG_PINMUX_QMSI          drivers/pinmux/qm_pinmux.c)
zephyr_library_sources_ifdef(CONFIG_PWM_QMSI             drivers/pwm/qm_pwm.c)
zephyr_library_sources_ifdef(CONFIG_RTC_QMSI             drivers/rtc/qm_rtc.c)
zephyr_library_sources_ifdef(CONFIG_SOC_FLASH_QMSI       drivers/flash/qm_flash.c)
zephyr_library_sources_ifdef(CONFIG_SOC_WATCH            drivers/soc_watch.c)
zephyr_library_sources_ifdef(CONFIG_UART_QMSI            drivers/uart/qm_uart.c)
zephyr_library_sources_ifdef(CONFIG_WDT_QMSI             drivers/wdt/qm_wdt.c)
endif()
