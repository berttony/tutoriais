1. Enable Hardware Virtualization in your BIOS/UEFI settings. Restart your PC and enter BIOS (usually by pressing Del, Esc, F1, F2, or F4 during startup). Look for a setting related to virtualization (it might be called Intel VT-x, AMD-V, Hyper-V, or SVM) and enable it.
2. Instalar a distro Ubuntu 24.04.1 LTS da Microsoft Store
3. Verifica se é wsl2: wsl --set-default-version 2

# Abra o PowerShell como administrador e execute:
wsl --install
# Se já tiver o WSL instalado, atualize-o:
wsl --update
# Defina o WSL2 como a versão padrão:
wsl --set-default-version 2
# Após reiniciar, instale uma distribuição Linux da Microsoft Store.

# lista as distro
wsl --list --verbose

# Use para listar distribuições disponíveis
wsl --list --online