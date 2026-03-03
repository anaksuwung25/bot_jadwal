#!/bin/bash

# ==========================
# CONFIG
# ==========================
FILE_LOG="/$path/employee.txt"
API_URL="https://api.telegram.org/bot<TOKEN_TELEGRAM>"
CHAT_ID="<CHAT_BOT>"

# ==========================
# CEK FILE
# ==========================
if [ ! -f "$FILE_LOG" ]; then
    echo "File tidak ditemukan!"
    exit 1
fi

# ==========================
# FORMAT TANGGAL HARI INI
# ==========================
TODAY=$(date +"%-d %b %Y")

# ==========================
# CARI DATA HARI INI
# ==========================
LINE=$(grep -m 1 "^$TODAY" "$FILE_LOG")

if [ -z "$LINE" ]; then
    echo "Data untuk tanggal $TODAY tidak ditemukan."
    exit 0
fi

DATA=$(echo "$LINE" | cut -d',' -f2-)

# ==========================
# PROSES DATA
# ==========================
PAGI_LIST=""
SORE_LIST=""

IFS=',' read -ra ARR <<< "$DATA"

for ITEM in "${ARR[@]}"; do
    ITEM=$(echo "$ITEM" | xargs)

    NAMA=$(echo "$ITEM" | cut -d'|' -f1 | xargs)
    SHIFT=$(echo "$ITEM" | cut -d'|' -f2 | xargs | tr '[:upper:]' '[:lower:]')

    if [[ "$SHIFT" == "pagi" ]]; then
        PAGI_LIST+="$NAMA"$'\n'
    elif [[ "$SHIFT" == "malam" ]]; then
        SORE_LIST+="$NAMA"$'\n'
    fi
done
# ==========================
# SUSUN PESAN (REAL NEWLINE)
# ==========================
MESSAGE="Semangat pagi!!!"$'\n\n'
MESSAGE+="Diinformasikan berikut jadwal piket Manchester United pada hari $(TZ="Asia/Jakarta" date)"$'\n'
MESSAGE+="================"$'\n\n'
MESSAGE+="Shift Pagi 08.00 - 16.00 WIB"$'\n'
MESSAGE+="$PAGI_LIST"$'\n'
MESSAGE+="Shift Sore 16.00 - 24.00 WIB"$'\n'
MESSAGE+="$SORE_LIST"$'\n'
MESSAGE+="Terimakasih"

# ==========================
# KIRIM PESAN (POST - TANPA ENCODE MANUAL)
# ==========================
wget -O- "${API_URL}/sendMessage?chat_id=${CHAT_ID}&text=${MESSAGE}"


echo "Selesai kirim pesan untuk tanggal $TODAY."
