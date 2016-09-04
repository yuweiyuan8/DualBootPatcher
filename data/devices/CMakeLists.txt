if(NOT MBP_TOP_LEVEL_BUILD)
    return()
endif()

set(files
    ark.yml
    asus.yml
    dexp.yml
    google.yml
    huawei.yml
    jiayu.yml
    lenovo.yml
    lg.yml
    motorola.yml
    nexus.yml
    oneplus.yml
    pantech.yml
    samsung/00_galaxy_s.yml
    samsung/01_galaxy_note.yml
    samsung/02_galaxy_j.yml
    samsung/03_galaxy_ace.yml
    samsung/04_galaxy_mega.yml
    samsung/05_galaxy_tab_tablet.yml
    samsung/06_galaxy_note_tablet.yml
    samsung/07_other.yml
    sony.yml
    wileyfox.yml
    xiaomi.yml
)

set(target_file "${CMAKE_CURRENT_BINARY_DIR}/devices.json")

add_custom_command(
    OUTPUT "${target_file}"
    COMMAND "${YAML2JSON_COMMAND}"
        ${files}
        -o "${target_file}"
        #--styled
        --flatten-root-array
    DEPENDS hosttools ${files}
    WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
    COMMENT "Generating device defition JSON file"
    VERBATIM
)

install(
    FILES "${target_file}"
    DESTINATION "${DATA_INSTALL_DIR}/"
    COMPONENT Libraries
)

add_custom_target(
    run_yaml2json
    ALL
    DEPENDS ${target_file}
)