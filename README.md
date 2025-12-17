#!/usr/bin/env bash
set -e

# Formato: URL | destino
REPOS=(
  "https://github.com/LineageOS/android_device_motorola_miami|device/motorola/miami"
  "https://github.com/LineageOS/android_device_motorola_sm6375-common|device/motorola/sm6375-common"

  "https://github.com/LineageOS/android_kernel_motorola_sm6375|kernel/motorola/sm6375"

  "https://github.com/TheMuppets/proprietary_vendor_motorola_sm6375-common|vendor/motorola/sm6375-common"
  "https://github.com/TheMuppets/proprietary_vendor_motorola_miami|vendor/motorola/miami"

  "https://github.com/LineageOS/android_hardware_motorola|hardware/motorola"
)

echo "==> Sincronizando repositorios Motorola"
echo

for entry in "${REPOS[@]}"; do
  REPO_URL="${entry%%|*}"
  DEST="${entry##*|}"

  echo "--------------------------------------------"
  echo "Repo   : $REPO_URL"
  echo "Destino: $DEST"

  mkdir -p "$(dirname "$DEST")"

  if [ ! -d "$DEST/.git" ]; then
    echo "[+] Clonando (shallow)"
    git clone --depth=1 "$REPO_URL" "$DEST"
  else
    echo "[*] Actualizando repo existente"
    (
      cd "$DEST"
      git fetch origin --prune
      git pull --ff-only
    )
  fi
done

echo
echo "✅ Todos los repos sincronizados correctamente"
