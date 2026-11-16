idf_component_register(SRCS "esp_ot_br.c"
                       INCLUDE_DIRS "."
                       EMBED_TXTFILES ${project_dir}/server_certs/ca_cert.pem
                       )

# Link the console's non-blocking REPL implementation (esp_console_repl_internal.c),
# which ESP-IDF only pulls in when esp_console_stop_repl is referenced. Without it
# the weak, blocking linenoiseSetReadCharacteristics() is used and the console
# task blocks when no USB host reads the USB-Serial-JTAG port (e.g. on a USB
# charger), stalling networking. See espressif/esp-thread-br#165.
target_link_libraries(${COMPONENT_LIB} INTERFACE "-u esp_console_stop_repl")
