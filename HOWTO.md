# SZMELC-GSM Handbook

<img src="https://github.com/user-attachments/assets/9d2cf65c-4dcb-43d9-a8b3-72470905748b" width="200" height="auto">

## Packages: 
> (not all required, simply listed all related) {Installable via APT / Pacman}
```
usb-modeswitch gammu modemmanager network-manager gammu-smsd smstools modemmanager libgsmme-dev gnokii picocom minicom ckermit python3-serial
```

## Getting started:
> Start by identifying your device and ports with `lsusb` command: \
> Example GSM module might look like this:
```
Bus 001 Device 048: ID 1a86:7523 QinHeng Electronics CH340 serial converter
```

Then identify bus ID with: `ls /dev/ttyUSB*` \
Example output:
```
/dev/ttyUSB1
```
Then you might want to add yourself to dialout group to make syscalls smoother with `sudo usermod -aG dialout $USER` \
Then finally, pick your preffered way of serial communication. (examples below)
> (MAKE SURE WHICH BAUD YOU ACTUALLY USING. [DEFAULT: 115200])
```
Minicom: sudo minicom -D /dev/ttyUSB0
Picocom: sudo picocom -b 9600 /dev/ttyUSB0
Screen: sudo screen /dev/ttyUSB0 115200
Kermit: sudo kermit -> <kermit setup commands>
```


## Commands:
```
Reboot:
- AT+CFUN=1,1

Enter PIN: (XXXX is SIM card PIN)
- AT+CPIN="XXXX"
Verify the SIM Status:
- AT+CPIN?

Check Network Registration:
(later set mode for each type by changing ? to =X (where X is 0-2))
- AT+CREG? - [GSM] 
- AT+CGREG? - [GPRS] 
- AT+CEREG? - [LTE]

Check current set service number: - AT+CSCA?
Set service number:
- AT+CMGS="+491760000443"
- AT+CSCA="+491760000443"

Check Signal Strength:
- AT+CSQ
Check if registered:
- AT+CREG?
Check network operators: (list all available)
- AT+COPS=?

Ensure sms settings are correct:
- AT+CSMS=1
FORCE REGISTER MODULE:
- AT+CREG=1
Set operator manually: (where 26202 is ID of operator) 
- AT+COPS=1,2,"26202"
Set network mode to gsm:
- AT+CNMP=13

Send SMS: (RECEIVERNUMBER is number to receive SMS)
- AT+CMGS="+RECEIVERNUMBER"
Then when `>`, type a message, and press Ctrl+Z to send it.
```

## Additional notes:
> IMPORTANT!!! - The SIM800L needs a 2Amp current while searching for the signal. It can run at 3.6v to 4.5v. \
> Any value outside of this range, will simply not work... \
> You should see {"CALL READY" "SMS READY.} messages after passing PIN. \
> If you don't see anything, then make sure module is plugged directly into main USB port, not via extender. \
> Get to know your SMS service number (can be found deep in android settings) \
`Example: [SMS SERVICE NUMBER (germany): +491760000443]` \
> Also nake sure your SIM is configured for your GSM module, shitty cheap modules support only G2, so you have to set it accordingly.

## Quick rundown:
> To check current values, do `AT+CREG?` & `AT+CGREG?`. You want to see `0,1` or `0,5` \
> You change values by `AT+CREG=X` & `AT+CGREG=X` (where X is allowed value) (0-2) \
> To see allowed values, do `AT+CREG=?`

## [CREG, CGREG, CEREG] Details:
> Here's a breakdown of the possible values for +CREG responses: \
> The response +CREG: 1,0 from the AT+CREG? command indicates the following: \
> The first number (1) indicates the registration status update is enabled. \
> The second number (0) indicates that the module is not registered with the network and is not currently searching for a new operator to register to. \
```
0: Not registered, the module is not currently searching for a new operator. 
1: Registered, home network. 
2: Not registered, but the module is currently searching for a new operator. 
3: Registration denied. 
4: Unknown (e.g., out of coverage). 
```

## Documentation
# [1](https://www.4itk.de/sms-zentralen/) [2](https://m2msupport.net/m2msupport/atcreg-network-registration/) [3](https://docs.eseye.com/Content/ELS61/ATCommands/ELS61CREG.htm) [4](https://onomondo.com/blog/at-command-cgreg/#creg-vs-cgreg-vs-cereg) [5](https://forum.arduino.cc/t/gsm-module-sim800l-no-signal/479829/9)
