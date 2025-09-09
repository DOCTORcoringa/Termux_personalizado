# Atualiza pacotes
pkg update && pkg upgrade -y

# Instala Python, pip e wget
pkg install python python-pip wget -y

# Instala bibliotecas Python necessárias
pip install rich pyfiglet

# Baixa script doctor.py do repo oficial
wget -O doctor.py https://raw.githubusercontent.com/DOCTORcoringa/Termux_personalizado/main/doctor.py

# Dá permissão para executar o script
chmod +x doctor.py

# Executa o script
./doctor.py
