# hoge - my zmk test kbd

- board: `xiao_ble`
- shield:
  - `hoge2`: bluetooth and usb-logging

# install
```
(host)
cd ~/work/zmk/zmk-configs/
git clone git@github.com:hidsh/hoge-zmk-config.git
```

# build
```
(host)
docker volume create --driver local -o o=bind -o type=none -o device="$HOME/zmk/" zmk-config
devcontainer up --workspace-folder $HOME/zmk
docker exec -w /workspaces/zmk -e TZ=Asia/Tokyo -it $(docker ps -ql) /bin/bash

(container)
cd /workspaces/zmk-config/zmk-configs/hoge-zmk-config
west build -s /workspaces/zmk/app -p -b xiao_ble -S zmk-usb-logging -- -DSHIELD=hoge2 -DZMK_CONFIG=$PWD/config
```

# check and flash
```
(host)
cd ~/work/zmk/zmk-configs/
cd hoge-zmk-config
less build/zephyr/.config       # check build configurations
file build/zephyr/zmk.uf2       # check which mcu the uf2 is for

sudo cp -f build/zephyr/zmk.uf2 <mounting-folder>/CURRENT.UF2
```

# check usb-log
```
tio /dev/ttyACM1
```
