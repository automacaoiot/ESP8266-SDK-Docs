Exemplos SDK-ESP
=====================

.. _Exemplos de uso do SDK-ESP:

Exemplos de uso do SDK-ESP
~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Recurso Rele ESP8266* ::

	from Device import Device
	from Rele import Rele
	from Led import LedRGB


	device = Device("PUBLIC_KEY","SECRET_KEY",1000)
	device.SYS_CPU_160MHZ()
	device.presenceLed(device.GPIO14,device.GPIO12,device.GPIO13,LedRGB.CATODO)
	device.setNetworkConfig('SSID','PWD')
	device.setDebug(True)

	rele = Rele(idRELE,device.GPIOrele,Rele.OPEN,1000)

	device.start()
	
*Recurso Rele ESP32* ::

	from Device import Device
	from Rele import Rele
	from Led import LedRGB


	device = Device("PUBLIC_KEY","SECRET_KEY",1000)
	device.SYS_CPU_240MHZ()
	device.presenceLed(device.GPIO25,device.GPIO26,device.GPIO27,LedRGB.CATODO)
	device.setNetworkConfig('SSID','PWD')
	device.setDebug(True)

	rele = Rele(idRELE,device.GPIOrele,Rele.OPEN,1000)

	device.start()


*Recurso Dht ESP8266* ::

	from Device import Device
	from Led import LedRGB
	#from Dht import Dht

	device = Device("PUBLIC_KEY","SECRET_KEY",1000)
	device.SYS_CPU_160MHZ()
	device.presenceLed(device.GPIO14,device.GPIO12,device.GPIO13,LedRGB.CATODO) # 14 (D5- RED) 12 (D6 - GREEN) 13 (D7 - BLUE)
	device.setNetworkConfig('SSID','PWD')
	device.setDebug(False)

	dht = Dht(idTemperature,idHumidity,device.GPIOdht,Dht.type,True,1000)

	device.start()

*Recurso Ds1820 ESP32* ::

	from Device import Device
	from Ds1820 import Ds1820
	from Led import LedRGB

	device = Device("PUBLIC_KEY","SECRET_KEY",1000)
	device.SYS_CPU_240MHZ()
	device.presenceLed(device.GPIO25,device.GPIO26,device.GPIO27,LedRGB.CATODO)
	device.setNetworkConfig('SSID','PWD')
	device.setDebug(True)

	rele = Rele(idRELE,device.GPIOrele,Rele.OPEN,1000)
	ds = Ds1820(idDS1820,device.GPIOds1820,Ds1820.type,device.escala,True)

	device.start()
	