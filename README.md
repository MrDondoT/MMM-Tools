# MMM-Tools
Display stats and remote controlling MagicMirror on SBC(ATB &amp; RPI), MMM-TelegramBot supported.

## Screenshots
![](https://github.com/bugsounet/MMM-Tools/blob/master/capture/capture2.jpg)

on `MagicMirror`

![](https://github.com/bugsounet/MMM-Tools/blob/master/capture/capture1.jpg)

on `Telegram`

## Feature
- Display system status on `MagicMirror`
- `MMM-TelegramBot` commands, `/status` and `/screen on|off` are supported.
- You can set alert threshold for abnormal status of `MagicMirror`. You can get warning message by `notification` and `TelegramBot`

## new Updates

### v1.1.3 : 2020-07-07
- Add record Uptime info (optional)
- Del AssistantSay feature (deprecied)

### v1.1.2 : 2020-07-06
- Add OS info
- Fix uptime

### v1.1.1 : 2020-05-23
- change uptime script for RPI

### v1.1.0 : 2020-05-12
- owner change
- move warning_text to translate files
- delete capture function (already in TelegramBot core)
- delete GPU info of rpi (same of cpu)
- add assistantSay feature (need myMagicWord feature of MMM-AssistantMk2)

## Install
```sh
cd ~/MagicMirror/modules
git clone https://github.com/bugsounet/MMM-Tools
```

## Configuration
```javascript
{
  module: 'MMM-Tools',
  position: 'bottom_right',
  config: {
    device : "RPI", // "ATB" is also available
    refresh_interval_ms : 5000,
    warning_interval_ms : 1000 * 60 * 5,
    enable_warning : true,
    recordUptime: false,
    warning : {
      CPU_TEMPERATURE : 65,
      GPU_TEMPERATURE : 65,
      CPU_USAGE : 75,
      STORAGE_USED_PERCENT : 80,
      MEMORY_USED_PERCENT : 80
    },
    uptime: { // display uptime in your language (RPI only)
      day: "day",
      hour: "hour",
      minute: "minute",
      plurial: "s"
    }
  }
}
```

### Detailed Configuration
|field | default | description
|--- |--- |---
|device | `"RPI"` | `"ATB"` for **Asus TinkerBoard (TinkerOS)**, <br/>`"RPI"` for **Raspberry Pi (Raspbian)**.
|refresh_interval_ms | `5000` | Milliseconds for refreshing status information on `MagicMirror`
|warning_interval_ms | `300000` | Milliseconds for preventing multiple warning message. After passing this duration from previous warning messages, same warning message will be sent.
|enable_warning | `true` | Set for sending warning message (notification and `TelegramBot` message)
|recordUptime | `true` | Display record uptime
|warning | See the below | Threshold values for warning message
|uptime | See the below | Display in your language uptime value

#### warning
|fields | default | description
|--- |--- |---
| CPU_TEMPERATURE | `65` | Set CPU or SoC temperature for warning
| GPU_TEMPERATURE | `65` | Set GPU temperature for warning
| CPU_USAGE | `75` | Set % of CPU Usage (ref. `/proc/stat`) for warning
| STORAGE_USED_PERCENT | `80` | Set % of used space of storage(SD Card) for warning
| MEMORY_USED_PERCENT | `80` | Set % of used space of memory(RAM) for warning

#### uptime
|fields | default | description
|--- |--- |---
| day | "day" | Display `day` in your language
| hour | "hour" | Display `hour` in your language
| minute | "minute" | Display `minute` in your language
| plurial | "s" | Display plurial `s`. If not needed you can set it with `""`
## Commands (For `MMM-TelegramBot`)
|command | description
|--- |---
|`/status` | Show system status
|`/screen on` | Turn display on
|`/screen off` | Turn display off

## Customizing view
You can customize view of this module with `CSS`. See the `MMM-Tools.css`
```css
 .Tools .status_item.status_ip {
   order: 1; // change order
   /* display : none; */ //set display
 }
```

## For Asus Tinker Board user
- on current TinkerOs (v 1.9), there is no `vgcencmd`, `vbetool`, `tvservice` or equivalents. So I should use `xset` for controlling screen.
- First you should set your xset dpms and screensaver on boot like this.
```sh
xset s noblank
xset s off
xset -dpms

xset s 0 0
xset dpms 0 0 0
```
There is no `/boot/config.text` in TinkerOS unlike Raspbian. I use `Xfce Power Manager` on TinkerOS LXDE desktop. (`Preference > Power Manager` menu)
Set `Blank after` and `Put to sleep after` and `Switch off after` by `Never`. It works.
- If you have any good idea for controling screen, please tell me.


## For Raspberry Pi(Raspbian) user
- Don't forget setting `device:"RPI",` in `config.js`

## Old Updates
### 2019-01-13
- Voice warning alert be enabled with MMM-AssistantMk2 (need myMagicWord feature of MMM-AssistantMk2)
```js
warning_text: {
  CPU_TEMPERATURE : "The temperature of CPU is over %VAL%",
  GPU_TEMPERATURE : "The temperature of GPU is over %VAL%",
  CPU_USAGE : "The usage of CPU is over %VAL%",
  STORAGE_USED_PERCENT : "The storage is used over %VAL% percent",
  MEMORY_USED_PERCENT : "The memory is used over %VAL% percent",
}

//%VAL% will be replace with current value of system.
```
> It's better to use with `noChimeOnSay: true` of MMM-AssistantMk2

### 2017-10-16
- Indonesian translations added (Thanks to @slametps)
- some bugs fixed.

### 2017-09-01
- Some bugs are fixed
- `/capture` command is added.
