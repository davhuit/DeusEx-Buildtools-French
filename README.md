```bash
# Prérequis (une fois par PC) : WSL2 + Ubuntu, Nix, Git, Visual Studio Build Tools (avec "Développement Desktop en C++")

Activer WSL (PowerShell/cmd en Administrateur)
wsl --install -d Ubuntu

Outils dans WSL
sudo apt update && sudo apt install -y git rsync dos2unix unzip

Nix
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install

Visual Studio Build Tools (côté Windows)

Télécharge depuis https://visualstudio.microsoft.com/fr/downloads/ → "Build Tools pour Visual Studio"
→ coche "Développement Desktop en C++". Redémarre si demandé. Vérifie ensuite (dans "Activer
ou désactiver des fonctionnalités Windows") que "Sous-système Windows pour Linux" et "Plateforme de
machine virtuelle" (ou "Plateforme d'ordinateur virtuel") sont bien cochées, sinon coche-les et redémarre.

Une fois Ubuntu installé, crée ton nom d'utilisateur/mot de passe Linux quand demandé.

# 1. Cloner les deux dépôts
mkdir -p /mnt/c/dev
cd /mnt/c/dev
git clone https://github.com/davhuit/DxController-Davhuit.git
git clone https://github.com/davhuit/DeusEx-BuildTools-Davhuit.git

# 2. Relier gamedir
cd /mnt/c/dev/DxController-Davhuit
rm -f gamedir
ln -s /mnt/c/dev/DeusEx-BuildTools-Davhuit gamedir

# 3. Localiser MSBuild (depuis cmd.exe)
dir /s /b "C:\Program Files (x86)\Microsoft Visual Studio\*MSBuild.exe" 2>nul

# 4. Build
export MSBUILD="/mnt/c/Program Files (x86)/Microsoft Visual Studio/18/BuildTools/MSBuild/Current/Bin/MSBuild.exe"
nix run .#sync-and-build

# 5. Installer dans le jeu
cp gamedir/System/DeusEx.u gamedir/System/DXController.u gamedir/System/DeusEx.exe gamedir/System/SDL3.dll "/mnt/c/Program Files (x86)/Steam/steamapps/common/Deus Ex/System/"
