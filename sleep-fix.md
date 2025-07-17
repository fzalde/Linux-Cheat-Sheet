# Sleep Fix

## Steps

- `cat /proc/acpi/wakeup | grep enabled`: Get list of devices that are either enabled or disabled to execute wake from suspend mode
- `cd /usr/lib/systemd/system-sleep`: Linux directory of files to execute when it goes into
- `sudo vim disable-some-wakeup`: Script to disable devices

  ```Bash
  #! /bin/bash
  case $1 in
      pre)
          declare -a devices=(RP09 PXSX) # <-- Add your entries here

          for device in "${devices[@]}"; do
              if $(grep -qw ^${device}.*enabled /proc/acpi/wakeup); then
                  echo ${device} > /proc/acpi/wakeup
              fi
          done
      ;;
  esac
  ```

- `sudo chmod 755 /usr/lib/systemd/system-sleep/disable-some-wakeup`: Change permissions

## Useful Links

- https://askubuntu.com/questions/1395148/pc-wakes-up-immediately-after-suspend
- https://www.youtube.com/watch?v=kUYyzlixfDg
