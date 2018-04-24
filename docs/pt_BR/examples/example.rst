Exemplos SDK-ESP8266
=====================

.. _Modo de Configuração Manual:

Modo de Configuração Manual
~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Recurso Beep* ::


	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "Device.h"
	#include "Beep.h"

	void _Device();
	void _Resource();

	Device *device;
	Beep *beep;

	void _Device()
	{
		device->start();
		_Resource();
	}

	void _Resource()
	{
		beep->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{
		debugf("Connected");
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		device = new Device("PUBLIC_KEY","SECRET_KEY");
		beep = new Beep(IDbeep,GPIObeep);

		device->setNetworkConfig("SSID","PASSWD");
	}


*Recurso Dht* ::

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "Device.h"
	#include "Dht.h"

	Device *device;
	Dht *dht;

	void _Device();
	void _Resource();

	void _Device()
	{
		device->start();
		_Resource();
	}

	void _Resource()
	{
		// DHT
		dht->setFilter(false);
		dht->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{
		device->_ledRGB->color(GREEN);
		debugf("Connected");
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		device = new Device("PUBLIC_KEY","SECRET_KEY");
		dht=new Dht(idTemperature,idHumidity,GPIOdht, DHTtype);

		device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_DISABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		device->setNetworkConfig("SSID","PASSWD");
	}


*Recurso Flame* ::

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "Device.h"
	#include "Flame.h"


	void _Device();
	void _Resource();

	Device *device;
	Flame *flame;

	void _Device()
	{
		device->start();
		_Resource();
	}

	void _Resource()
	{
		flame->setFilter(true);
		flame->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{
		debugf("Connected");
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		device = new Device("PUBLIC_KEY","SECRET_KEY");
		flame = new FLAME(IDflame,GPIOflame);

		device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_DISABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		device->setNetworkConfig("SSID","PASSWD");
	}

*Recurso Rele* ::

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "Device.h"
	#include "Rele.h"

	Device *device;
	Rele *rele;

	void _Device();
	void _Resource();

	void _Device()
	{
		device->start();
		_Resource();
	}

	void _Resource()
	{
		rele->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{
		device->_ledRGB->color(GREEN);
		debugf("Connected");
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		device = new Device("PUBLIC_KEY","SECRET_KEY");
		rele= new Rele(IDrele,GPIOrele);

		//Set GPIO LED RGB
    device->ledRGB->set(GPIOred,GPIOgreen,GPIOblue,ANODO-OR-CATODO);
    device->ledRGB->color(RED);

		device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_DISABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		device->setNetworkConfig("SSID","PASSWD");
	}

.. _Modo de Configuração WIFI:

Modo de Configuração WIFI
~~~~~~~~~~~~~~~~~~~~~~~~~

*Recurso Rele com Led RGB* ::

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "Device.h"
	#include "Rele.h"

	Device *device;
	Rele *rele;

	void _Device();
	void _Resource();

	void _Device()
	{
		device->start();
		_Resource();
	}

	void _Resource()
	{
		rele->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{
		debugf("Connected");
		device->_ledRGB->color(GREEN);
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		device=new Device();
		rele= new Rele(GPIOrele);

		//Set GPIO LED RGB
		device->_ledRGB->set(GPIOred,GPIOgreen,GPIOblue,ANODO-OR-CATODO);
		device->_ledRGB->color(RED);

		device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_ENABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		device->setNetworkConfig();
	}

*Recurso WaterLevel* ::

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "Device.h"
	#include "Waterlevel.h"

	void _Device();
	void _Resource();

	//CLASSES
	Device *device;
	Waterlevel *waterLevel;

	void _Device()
	{
		// Ativa Device
		device->start();
		_Resource();
	}

	void _Resource()
	{

		//WATER LEVEL
		waterLevel->setFilter(true);
		waterLevel->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{
		device->ledRGB->color(GREEN);
		debugf("Connected");
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		device = new Device();
		waterLevel = new Waterlevel(GPIO4,);

		//Set GPIO LED RGB
		device->_ledRGB->set(GPIOred,GPIOgreen,GPIOblue,ANODO-OR-CATODO);
		device->_ledRGB->color(RED);

		device->setNetworkConfig();
	}
