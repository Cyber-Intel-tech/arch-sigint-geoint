#!/bin/bash

# OSINT, SIGINT, GEOINT, CYBERINT Arch Linux Setup Script
# Author: Cyber-Intel-tech
# GitHub: https://github.com/Cyber-Intel-tech/arch-sigint-geoint/edit/main/README.md
# Questo script installa e configura una distribuzione Arch Linux con tutti gli strumenti avanzati per OSINT, SIGINT, GEOINT e CYBERINT.

# Aggiornamento del sistema
echo "[+] Aggiornamento del sistema..."
pacman -Syu --noconfirm

# Installazione di pacchetti di base
echo "[+] Installazione di pacchetti di base..."
pacman -S --noconfirm base-devel git wget curl vim python python-pip

# Creazione della struttura delle cartelle
mkdir -p ~/SIGINT ~/GEOINT ~/CYBERINT ~/OSINT ~/Databases ~/Scripts

# Installazione strumenti SIGINT
echo "[+] Installazione strumenti SIGINT..."
pacman -S --noconfirm gnuradio gqrx multimon-ng direwolf baudline
pip install urh

git clone https://github.com/merbanan/rtl_433.git ~/SIGINT/rtl_433
cd ~/SIGINT/rtl_433 && mkdir build && cd build && cmake .. && make && make install

# Installazione strumenti GEOINT
echo "[+] Installazione strumenti GEOINT..."
pacman -S --noconfirm qgis opendronemap google-earth-pro sentinel-toolbox

git clone https://github.com/sat-utils/satdump.git ~/GEOINT/satdump
cd ~/GEOINT/satdump && chmod +x install.sh && ./install.sh

# Installazione strumenti CYBERINT
echo "[+] Installazione strumenti CYBERINT..."
pacman -S --noconfirm tor nmap wireshark-qt
pip install maltego spiderfoot recon-ng theHarvester

# Installazione strumenti OSINT
echo "[+] Installazione strumenti OSINT..."
pacman -S --noconfirm jq whois dnsutils
pip install sherlock holehe

git clone https://github.com/laramies/theHarvester.git ~/OSINT/theHarvester
cd ~/OSINT/theHarvester && pip install -r requirements.txt

# Configurazione database personalizzati
echo "[+] Configurazione database personalizzati..."
pacman -S --noconfirm mariadb postgresql
systemctl enable mariadb postgresql
systemctl start mariadb postgresql

echo "[+] Creazione database intelligence..."
mysql -u root -e "CREATE DATABASE sigint_db; CREATE DATABASE geoint_db; CREATE DATABASE cyberint_db;"

# Fine dello script
echo "[+] Installazione completata! Riavvia il sistema per applicare tutte le modifiche."
