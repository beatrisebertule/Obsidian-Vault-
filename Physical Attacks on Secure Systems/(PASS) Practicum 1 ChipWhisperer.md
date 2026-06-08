
Questions
....

### Fix connection to the chip whisperer

find the device name
`ls /dev/ 

change privileges
`sudo chmod 666 /dev/ttyACM0` where ttyACM0  is the device name


### Code explanation

Scope API
`import chipwhisperer as cw
`scope = cw.scope()

The scope object is used to control the capture/glitch portion of the ChipWhisperer device.



### Activate environment
`source ~/.cwvenv/bin/activate
`cd ~/chipwhisperer
`jupyter notebook
`
