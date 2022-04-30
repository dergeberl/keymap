VERSION 0.6
FROM qmkfm/qmk_cli

ARG KEYBOARD="splitkb/kyria/rev2"
ARG KEYMAP=dergeberl

all:
    BUILD +kyria-lint
    BUILD +kyria-build
    BUILD +kyria-json

qmk-setup:
    RUN qmk setup -y

kyria-copy:
    FROM +qmk-setup
    COPY kyria /qmk_firmware/keyboards/splitkb/kyria/keymaps/$KEYMAP/

kyria-lint:
    FROM +kyria-copy
    RUN qmk lint -km $KEYMAP -kb $KEYBOARD

kyria-build:
    FROM +kyria-copy
    RUN qmk compile -km $KEYMAP -kb $KEYBOARD
    SAVE ARTIFACT ./qmk_firmware/*.hex AS LOCAL ./builds/

kyria-json:
    FROM +kyria-copy
    RUN qmk c2json -km $KEYMAP -kb $KEYBOARD -o ./qmk_firmware/keymap.json ./qmk_firmware/keyboards/splitkb/kyria/keymaps/$KEYMAP/keymap.c
    SAVE ARTIFACT ./qmk_firmware/keymap.json AS LOCAL ./json/

kyria-flash:
    FROM +kyria-build
    LOCALLY
    RUN dfu-programmer atmega32u4 erase --force
    RUN dfu-programmer atmega32u4 flash builds/splitkb_kyria_rev2_dergeberl.hex
    RUN dfu-programmer atmega32u4 reset