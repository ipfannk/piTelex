## List of changes relevant to the user in the  "testing" branch compared to the "master" branch 

###  New config file parameter `block_ascii`

* Module: ITelexSrv

* Description:

Added config file parameter 
````json
	"block_ascii" : true/false    # default true) 
````
to avoid port scans and such being printed at the TTY, if set to true. 

Hint: On an incoming ASCII-connection, the teleprinter may or may not be switched on for a few seconds, but printing is suppressed.



###  Added support for USB-Keypads

* Module: `Keypad` (new)

* Dependencies: python-evdev, Linux ONLY!

* Description:  

  Added device `keypad`, allows connection of a numeric keypad via usb
  KeyPad input for text shortcuts and test teletypes.


### Added a "hand type simulator"

* Module: MCP

* Description:
 
   Added a simulator for manual typing, i.e insert random delays between "keystrokes".
   Toggle on/off in Screen via `<ESC> T`.
   When enabled, prints "lorem ipsum" dummy text with pseudo random delay between keystrokes until disabled.
   To output the text to the teleprinter, first activate it by either dialling '009' or entering `<ESC> A`in `screen`.

### Added modes "AGT-TWM", "AGT-TW39"

* Module: RPiTTY

* Description:

  Introduce more specific modes `AGT-TW39`, `AGT-TWM` (module RPiTTY) for use with Ö-AGT's (will probably replace general mode `AGT` in a later version)

### Added LED_Z

* Module: RPiCtrl
* Description:

  Added config parameter 
  ```json
  "pin_LED_Z": 0    # integer > 0, default 0 
  ```
  which defines a connection pin for a LED which is lit in "Z" and "ZZ" mode.

### Added heartbeat function for LED_Z

* Module: RPiCtrl
* Description:

  Added config parameter
  ```
  "LED_Z_heartbeat": 6     # integer > 0, default 6)
  ```
  which defines the pause in steps of 500ms between two flashes of the  LED. A value of 0 means "no flashing".

  Flashing is only for ZZ mode! Z mode is indicated with steady light of the LED.

### Switch off current loop in "ZZ" mode

* Module: RPiTTY
* Description:

  Added config file parameter 
  ```json
  "txd_powersave": true/false #default is false
  ```
  If set to true, loop current will be switched off in ZZ status. 
  
  Mostly useful in context with `"pin_power"` option of module RPiCtrl. 
  
  Especially useful for t68d machines; in standby, they pull a current of 5mA, but when mains is diconnected, raise it to 40mA, 
  which means unnecessary thermal loss.
  
### Added feature to insert text files into the character stream

* Module: MCP
* Description:

  Entering five or more subsequent `WR` (carriage return) at the telerpinter lets pitelex 
  interpret the following number from 0 to 9 as a file name located in the subdirectory 
  "read" of pitelex. The file must have the extension `.txt`.
  
  If the fie exists, pitelex will insert the contents of the file.
  

### TNS hosts now configurable
* Module: ITelex
* Description:

Up to now, the TNS hostnames were hardcoded in the sources.
The new config file option `tns_srv`, which defaults to

```json
"tns_srv": ['tlnserv.teleprinter.net','tlnserv2.teleprinter.net','tlnserv3.teleprinter.net'],
```
allows to configure the TNS hosts in telex.json. Under normal circumstances, there should be no need to change the default. But 
piTelex' default mechanism of selecting a TNS host is rather rudimentary and cannot cope with nonexistent or faulty servers. In such cases, the list can be reduced to functional addresses.

